# 🎣 Laboratório Didático: Como Funcionam os Ataques de Phishing

> **Disciplina:** Segurança de Sistemas
> **Objetivo:** entender, *na prática e com segurança*, como um atacante cria uma
> página falsa (phishing) a partir de um site real — para então saber reconhecer
> e se defender desse tipo de golpe.

---

## ⚖️ Leia primeiro: as regras do laboratório

Este exercício ensina a **mecânica** do phishing com fins de **defesa**. Para que
isso seja feito de forma ética e legal, o laboratório foi desenhado com três
travas de segurança. **Elas não são opcionais.**

1. **O alvo é fictício.** Você vai espelhar o site `Portal Acadêmico — Fictícia S.A.`,
   que está incluído neste repositório (pasta [`site-alvo/`](site-alvo/)). É uma
   instituição inventada. **Você nunca deve espelhar o site real de um banco, de
   uma empresa, da faculdade ou de qualquer instituição de verdade.** Clonar a
   identidade de uma organização real e publicá-la é crime (estelionato,
   falsidade ideológica), mesmo "só para testar".

2. **A página publicada é marcada como treino.** Sua cópia leva uma faixa visível
   de `AMBIENTE DE TREINO — PHISHING SIMULADO`.

3. **Nenhum dado é coletado.** O formulário da sua cópia **não envia senha para
   lugar nenhum**. Ao clicar em "Entrar", ele mostra uma tela educativa e
   descarta o que foi digitado. Uma página de phishing de verdade roubaria esses
   dados — aqui, nós substituímos o roubo pela lição.

> 📌 **Por que fazer isso então?** Porque entender como a isca é montada é o que
> permite reconhecê-la. Você vai sair deste laboratório sabendo exatamente onde
> olhar para não cair num phishing — e isso vale para a sua vida toda.

---

## 🧠 Antes de começar: o que é phishing?

**Phishing** é um golpe em que o atacante se passa por alguém confiável (um banco,
a faculdade, um serviço de e-mail) para enganar a vítima e roubar informações —
normalmente **usuário e senha**.

O ataque típico tem três peças:

```
  [1] A ISCA                [2] A PÁGINA FALSA           [3] A COLHEITA
  e-mail/mensagem    →      cópia visual de um    →      o que a vítima digita
  com um link e             site real, hospedada         é enviado ao atacante
  um senso de urgência      num endereço do atacante
```

Neste laboratório, nosso foco é a peça **[2]**: como a página falsa é construída.
Vamos trocar a peça **[3]** (a colheita) por uma tela educativa, e a peça **[1]**
(a isca) será só discutida em aula, sem envio real de mensagens a ninguém.

A ferramenta central é um **web crawler** (ou "aranha"): um programa que lê uma
página, baixa tudo o que ela usa (HTML, CSS, imagens) e segue os links para baixar
as páginas vizinhas. O resultado é uma **versão estática** — uma cópia que funciona
sem o servidor original. Usaremos o **HTTrack**, feito exatamente para isso.

---

## 🧰 Pré-requisitos

- [ ] Um computador com Windows, macOS ou Linux.
- [ ] **HTTrack** instalado (instruções no Passo 1).
- [ ] Uma conta no **GitHub** (gratuita — crie em [github.com](https://github.com)).
- [ ] Este repositório baixado ("Code" → "Download ZIP", ou `git clone`).

Nenhuma experiência prévia com terminal é necessária: o HTTrack tem uma versão
com janelas (WinHTTrack), e cada passo abaixo está explicado em detalhe.

---

## Passo 1 — Instalar o HTTrack

O HTTrack é gratuito e de código aberto. Site oficial: **https://www.httrack.com**

| Sistema | Como instalar |
|---|---|
| **Windows** | Baixe o **WinHTTrack** (instalador com interface gráfica) do site oficial. |
| **macOS** | No Terminal: `brew install httrack` (requer [Homebrew](https://brew.sh)). |
| **Linux (Debian/Ubuntu)** | No Terminal: `sudo apt update && sudo apt install httrack webhttrack` |

> 💡 No Windows, a versão **WinHTTrack** abre um assistente em janelas — ideal para
> quem está começando. Os prints mentais abaixo seguem essa versão; quem usar
> Linux/macOS encontra os mesmos campos na versão `webhttrack` (abre no navegador)
> ou na linha de comando (ao final deste passo).

---

## Passo 2 — Subir o site-alvo fictício na sua máquina

Antes de espelhar, precisamos que o site-alvo esteja "no ar" localmente, para o
HTTrack ter o que copiar. Vamos servi-lo em `http://localhost:8000`.

1. Abra o terminal (Prompt de Comando no Windows) **dentro da pasta `site-alvo/`**
   deste repositório.
2. Rode um servidor local simples com Python (já vem no macOS/Linux; no Windows,
   instale de [python.org](https://python.org) se preciso):

   ```bash
   python -m http.server 8000
   ```

3. Abra o navegador em **http://localhost:8000** — você deve ver a tela de login
   do *Portal Acadêmico Fictícia S.A.* Deixe esse terminal aberto; ele é o
   "servidor" do nosso alvo.

> 🔎 **Observe o alvo com calma.** Esta é a página "legítima" do nosso cenário.
> Repare no endereço (`localhost:8000`), no cadeado (não há — é HTTP), no layout.
> Daqui a pouco vamos comparar com a cópia.

---

## Passo 3 — Espelhar o site com o HTTrack

Agora o passo central: criar a **versão estática** (o espelho).

### No WinHTTrack (Windows, com janelas)

1. Abra o WinHTTrack e clique em **Next**.
2. **Project name:** `espelho-portal`. Escolha uma pasta de destino. **Next**.
3. **Action:** deixe em *Download web site(s)*.
4. Em **Web Addresses (URL):** digite `http://localhost:8000/`
5. Clique em **Set options** (opcional, para entender) → aba **Limits**: aqui se
   controla a profundidade. Para nosso site pequeno, os padrões bastam. **OK**.
6. **Next** → **Finish**. O HTTrack vai rodar e baixar os arquivos.

### Na linha de comando (macOS / Linux / quem preferir)

Dentro de uma pasta vazia onde você quer guardar o espelho:

```bash
httrack "http://localhost:8000/" -O "./espelho-portal" "+localhost:8000/*" -v
```

O que cada parte significa — **entender isto é metade do aprendizado**:

| Parte | O que faz |
|---|---|
| `"http://localhost:8000/"` | o endereço inicial que a aranha vai visitar |
| `-O "./espelho-portal"` | pasta de saída (**O**utput) onde a cópia é gravada |
| `"+localhost:8000/*"` | regra de filtro: "pode baixar tudo deste endereço" |
| `-v` | modo **v**erboso: mostra na tela cada arquivo baixado |

### O que observar enquanto ele roda

Preste atenção nas mensagens: o HTTrack baixa `index.html`, depois descobre o link
para `sobre.html` e o segue, depois pega `css/estilo.css`. **É a aranha seguindo
os links** — exatamente como um atacante captura um site inteiro com um comando.

Ao terminar, abra a pasta `espelho-portal/`. Dentro dela há um `index.html`. Abra-o
no navegador (duplo clique). Você verá a cópia do portal — **sem nenhum servidor
rodando**. Essa independência é o que torna o espelho perigoso em mãos erradas: ele
pode ser levado para qualquer lugar.

> 🧪 **Experimento rápido:** feche o terminal do Passo 2 (desligue o alvo) e abra o
> `index.html` da cópia de novo. Ele continua funcionando. Entenda por quê: a cópia
> não depende mais do original.

---

## Passo 4 — Transformar a cópia em "página de treino"

A cópia do Passo 3 é um clone cru. Antes de publicar, vamos aplicar as **duas
travas de segurança**: a faixa de treino e o formulário que não coleta dados.

Em vez de você editar na mão, este repositório **já traz a cópia pronta e marcada**
na pasta [`exemplo-espelho/`](exemplo-espelho/). Compare-a com o clone que você
gerou e copie para dentro dela os dois elementos (veja os comentários no
`exemplo-espelho/index.html`, que explicam cada um):

1. **A faixa `AMBIENTE DE TREINO`** — um `<div class="faixa-treino">` logo após
   `<body>`, com o CSS correspondente.

2. **O formulário que educa em vez de coletar** — um pequeno script que:
   - impede o envio dos dados (`e.preventDefault()`);
   - **apaga** o que foi digitado;
   - mostra a tela educativa com os sinais de phishing.

> ✅ **Regra:** a página que você publicar **tem** que ter essas duas travas. Se a
> sua cópia coletar dados ou se passar por um site real sem aviso, você saiu do
> laboratório e entrou no território de um ataque de verdade. Não faça isso.

Para o laboratório, você pode simplesmente **publicar a pasta `exemplo-espelho/`**,
que já está correta — e usar o clone que você mesmo gerou apenas para comparar e
entender a técnica.

---

## Passo 5 — Publicar no GitHub Pages

O **GitHub Pages** hospeda sites estáticos de graça. É aqui que a cópia ganha um
endereço público — e é também onde veremos uma lição importante sobre o cadeado.

1. No GitHub, crie um repositório novo, por exemplo `treino-phishing-ficticia`.
   Marque como **Public**.
2. Envie o conteúdo da pasta `exemplo-espelho/` para a **raiz** do repositório
   (os arquivos `index.html`, `sobre.html` e a pasta `css/` devem ficar no topo).
   - Pela web: "Add file" → "Upload files" → arraste os arquivos → "Commit".
   - Por git:
     ```bash
     git clone https://github.com/SEU-USUARIO/treino-phishing-ficticia.git
     cp -r exemplo-espelho/* treino-phishing-ficticia/
     cd treino-phishing-ficticia
     git add . && git commit -m "Publica pagina de treino de phishing"
     git push
     ```
3. No repositório, vá em **Settings → Pages**.
4. Em **Source**, escolha a branch `main` e a pasta `/ (root)`. **Save**.
5. Aguarde ~1 minuto. O GitHub mostrará a URL, algo como:
   `https://SEU-USUARIO.github.io/treino-phishing-ficticia/`

Abra essa URL. Pronto: a página de treino está publicada na internet.

> 🔐 **A lição do cadeado.** Repare que o endereço do GitHub Pages tem **HTTPS** e
> **cadeado**. A sua página de treino — e, por analogia, uma página de phishing —
> tem o cadeadinho "de segurança"! Isso prova, na prática, a mentira mais comum
> sobre segurança: **o cadeado só diz que a conexão é cifrada, não que o site é
> confiável.** Golpistas usam HTTPS o tempo todo. **O que importa é a URL**, não o
> cadeado.

---

## Passo 6 — A parte mais importante: análise defensiva

Agora que você construiu a isca, vamos virar a mesa e aprender a **não morder**.
Abra a sua página publicada e responda, usando o
[checklist de defesa](docs/checklist-analise-defensiva.md):

1. **Olhe a URL.** Em que ela difere da do site "oficial" (`localhost:8000`)?
   Um atacante contaria com você não reparar nisso.
2. **O cadeado te enganou?** Ele está lá, verde e bonito. O que ele realmente
   garante?
3. **O que o espelho NÃO conseguiu copiar?** Funções que dependem do servidor
   (um login que de fato valida a senha, por exemplo) não existem na cópia. Que
   pistas isso deixa?
4. **Como a isca chegaria?** Discuta em grupo: que e-mail ou mensagem faria alguém
   clicar? Que palavras criam urgência? (Não envie nada a ninguém — só analise.)

### 🛡️ As defesas que você leva para a vida

- **Nunca digite senha a partir de um link** recebido por e-mail/mensagem. Acesse
  sempre digitando o endereço oficial ou por um favorito confiável.
- **Confira a URL inteira**, da esquerda para a direita, com atenção ao domínio.
- **Desconfie de urgência** ("sua conta será bloqueada em 24h").
- **Cadeado ≠ confiança.** HTTPS é o mínimo, não um selo de autenticidade.
- **Ative a verificação em duas etapas (2FA)** onde puder: mesmo que a senha
  vaze, o atacante trava no segundo fator.

---

## 🧹 Encerramento

Terminada a atividade, **despublique** a página: em **Settings → Pages**, desative o
Pages, ou exclua o repositório de treino. Não há motivo para manter uma página de
phishing (mesmo simulado) no ar depois da aula.

---

## 📂 Estrutura deste repositório

```
phishing-lab/
├── README.md                 ← este roteiro
├── site-alvo/                ← o site FICTÍCIO que você vai espelhar
│   ├── index.html
│   ├── sobre.html
│   └── css/estilo.css
├── exemplo-espelho/          ← a cópia PRONTA e marcada (referência / publicar)
│   ├── index.html            ← com faixa de treino + formulário educativo
│   ├── sobre.html
│   └── css/estilo.css
└── docs/
    ├── guia-do-professor.md
    ├── checklist-analise-defensiva.md
    └── publicar-no-github-pages.md
```

---

## 📜 Aviso de uso

Material **exclusivamente educacional**, para uso em ambiente controlado de ensino.
As técnicas aqui demonstradas devem ser usadas **somente** sobre o alvo fictício
fornecido. Aplicá-las contra sites, sistemas ou pessoas reais, sem autorização
expressa, é ilegal e antiético. Ao usar este material, você concorda em mantê-lo no
escopo defensivo e didático para o qual foi criado.
