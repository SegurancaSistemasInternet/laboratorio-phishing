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

- [ ] Um computador com **Windows**. Este roteiro dá suporte apenas a esse ambiente.
- [ ] **Git for Windows**, **Visual Studio Code** e **WinHTTrack** instalados
      (instruções no Passo 1).
- [ ] A extensão **Live Server**, de **Ritwick Dey**, instalada no VS Code.
      Ela adiciona o botão **Go Live**.
- [ ] Uma conta no **GitHub** (gratuita — crie em [github.com](https://github.com)).
- [ ] Este repositório **clonado localmente com Git** (instruções no Passo 2).

Nenhuma experiência prévia com terminal é necessária: usaremos o **PowerShell**
apenas para clonar o repositório. O WinHTTrack tem interface gráfica, e a
visualização local será feita pelo VS Code com **Go Live**, sem instalar Python.

---

## Passo 1 — Preparar o ambiente Windows

1. Instale o **Git for Windows** pelo site [git-scm.com](https://git-scm.com/downloads/win).
2. Instale o **Visual Studio Code** pelo site [code.visualstudio.com](https://code.visualstudio.com).
3. No VS Code, abra **Extensões** (`Ctrl+Shift+X`), procure **Live Server**, de
   **Ritwick Dey** (identificador `ritwickdey.LiveServer`), e clique em **Install**.
   O botão para iniciar o servidor se chama **Go Live**.
4. Instale o **WinHTTrack**, a versão do HTTrack com interface gráfica para
   Windows, pelo site oficial [httrack.com](https://www.httrack.com).

> 💡 Depois de instalar o Git, feche e abra novamente o VS Code e o PowerShell
> para que reconheçam o comando `git`. Todo o roteiro abaixo usa Windows.

---

## Passo 2 — Clonar o repositório e abrir o site-alvo com Go Live

Antes de espelhar, precisamos que o site-alvo esteja "no ar" localmente, para o
HTTrack ter o que copiar. O **Live Server** fará isso a partir da cópia local do
repositório.

1. Abra o **PowerShell** na pasta em que deseja guardar o laboratório (pelo
   Explorador de Arquivos, clique com o botão direito na pasta e escolha
   **Abrir no Terminal**; selecione PowerShell, se necessário).
2. Clone o repositório:

   ```powershell
   git clone https://github.com/SegurancaSistemasInternet/laboratorio-phishing.git
   ```

3. No VS Code, use **File → Open Folder** (**Arquivo → Abrir Pasta**) e selecione
   a pasta **`laboratorio-phishing`** criada pela clonagem.
4. No Explorador do VS Code, abra **`site-alvo\index.html`** e clique em
   **Go Live** na barra de status. Se o navegador abrir a listagem da raiz,
   clique em **site-alvo** e depois em **index.html**.
5. Confira se aparece a tela de login do *Portal Acadêmico Fictícia S.A.*.
   O endereço padrão é **`http://127.0.0.1:5500/site-alvo/index.html`**.
   Anote a URL exibida: se a porta `5500` estiver ocupada, a extensão pode usar
   outra. **Use sempre a porta efetiva nos próximos passos.**
6. Deixe o VS Code aberto e o Live Server ativo enquanto o WinHTTrack copia
   o alvo. Não é necessário manter um terminal com um servidor Python.

> 🔎 **Observe o alvo com calma.** Esta é a página "legítima" do nosso cenário.
> Repare no endereço (`127.0.0.1`, a própria máquina), na porta, no caminho
> `site-alvo/` e no uso de HTTP, sem HTTPS. Daqui a pouco vamos comparar com a cópia.

---

## Passo 3 — Espelhar o site com o HTTrack

Agora o passo central: criar a **versão estática** (o espelho).

### No WinHTTrack (com janelas)

1. Abra o WinHTTrack e clique em **Next**.
2. **Project name:** `espelho-portal`. Escolha uma pasta de destino **fora da
   pasta do repositório clonado**, para separar o alvo da cópia. **Next**.
3. **Action:** deixe em *Download web site(s)*.
4. Em **Web Addresses (URL):** digite `http://127.0.0.1:5500/site-alvo/`,
   ajustando a porta para a que apareceu no Passo 2.
5. Clique em **Set options** → aba **Scan Rules**. Adicione
   `-http://127.0.0.1:5500/*` e, depois,
   `+http://127.0.0.1:5500/site-alvo/*` (ajuste a porta nas duas regras).
   Assim, a cópia fica restrita ao alvo fictício, sem incluir outras pastas
   servidas pelo VS Code. Na aba **Limits**, os padrões bastam para nosso site
   pequeno. **OK**.
6. **Next** → **Finish**. O HTTrack vai rodar e baixar os arquivos.

### O que observar enquanto ele roda

Preste atenção nas mensagens: o HTTrack baixa `index.html`, depois descobre o link
para `sobre.html` e o segue, depois pega `css/estilo.css`. **É a aranha seguindo
os links** — exatamente como um atacante captura as páginas de um site.

Ao terminar, pare o servidor do alvo clicando em **Port: 5500** (ou na porta
efetiva) na barra de status do VS Code. Use **Arquivo → Abrir Pasta** para abrir a
pasta `espelho-portal` gerada pelo WinHTTrack. Abra o `index.html` dessa pasta e
clique em **Go Live**. Se aparecer o índice de projetos do HTTrack, clique no
projeto para chegar à cópia do portal.

> 🧪 **Experimento rápido:** o servidor do alvo foi desligado, mas a cópia continua
> funcionando, agora servida pelo Go Live a partir da pasta do espelho. Ela não
> depende mais dos arquivos do repositório original. Não confunda o servidor
> usado para visualizar a cópia com o servidor do alvo.

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

Para visualizar a versão marcada antes de publicar, pare o Live Server do
espelho, reabra a pasta `laboratorio-phishing` no VS Code, abra
`exemplo-espelho\index.html` e clique em **Go Live**. Confira o endereço
`http://127.0.0.1:5500/exemplo-espelho/index.html` (ou a porta efetiva), a faixa de
treino e a tela educativa. Use apenas dados fictícios no formulário.

---

## Passo 5 — Publicar no GitHub Pages

O **GitHub Pages** hospeda sites estáticos de graça. É aqui que a cópia ganha um
endereço público — e é também onde veremos uma lição importante sobre o cadeado.

1. No GitHub, crie um repositório novo, por exemplo `treino-phishing-ficticia`.
   Marque como **Public**.
2. Envie o conteúdo da pasta `exemplo-espelho/` para a **raiz** do repositório
   (os arquivos `index.html`, `sobre.html` e a pasta `css/` devem ficar no topo).
   - Pela web: "Add file" → "Upload files" → arraste os arquivos → "Commit".
   - Pelo PowerShell: siga a **Opção B** do
     [guia de publicação](docs/publicar-no-github-pages.md), com comandos para Windows.
3. No repositório, vá em **Settings → Pages**.
4. Em **Source**, escolha **Deploy from a branch**; em **Branch**, selecione
   `main` e a pasta `/ (root)`. **Save**.
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

1. **Olhe a URL.** Em que ela difere da do site "oficial"
   (`http://127.0.0.1:5500/site-alvo/index.html`, ou a URL anotada no Passo 2)?
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

Pare também o Live Server clicando no indicador **Port: …** na barra de status
do VS Code.

---

## 📂 Estrutura deste repositório

```
laboratorio-phishing/
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
