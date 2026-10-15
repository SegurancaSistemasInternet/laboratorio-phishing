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
   publicado pelo professor no **GitHub Pages**, a partir do repositório separado
   [SegurancaSistemasInternet/site-alvo](https://github.com/SegurancaSistemasInternet/site-alvo).
   É uma instituição inventada, embora seu site tenha uma URL pública.
   **Você nunca deve espelhar o site real de um banco, de
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
- [ ] **Git for Windows** e **Visual Studio Code** instalados, e **WinHTTrack**
      disponível na versão **ZIP sem instalador** (instruções no Passo 1).
- [ ] A extensão **Live Server**, de **Ritwick Dey**, instalada no VS Code.
      Ela adiciona o botão **Go Live**.
- [ ] Uma conta no **GitHub** (gratuita — crie em [github.com](https://github.com)).
- [ ] Este repositório **clonado localmente com Git** (instruções no Passo 2).
- [ ] Acesso à internet e ao **site-alvo fictício público**, previamente publicado
      e autorizado pelo professor (endereço no Passo 2).

Nenhuma experiência prévia com terminal é necessária: usaremos o **PowerShell**
apenas para clonar o repositório de materiais. O **site público** será espelhado
pelo WinHTTrack, com interface gráfica, e a visualização local da **cópia** será
feita pelo VS Code com **Go Live**, sem instalar Python.

---

## Passo 1 — Preparar o ambiente Windows

1. Instale o **Git for Windows** pelo site [git-scm.com](https://git-scm.com/downloads/win).
2. Instale o **Visual Studio Code** pelo site [code.visualstudio.com](https://code.visualstudio.com).
3. No VS Code, abra **Extensões** (`Ctrl+Shift+X`), procure **Live Server**, de
   **Ritwick Dey** (identificador `ritwickdey.LiveServer`), e clique em **Install**.
   O botão para iniciar o servidor se chama **Go Live**.
4. Prepare o **WinHTTrack**, a versão do HTTrack com interface gráfica para
   Windows, **sem precisar de privilégios de administrador**:
   - Abra a [página oficial de downloads](https://www.httrack.com/page/2/en/index.html)
     e escolha a opção **without installer (eg: USB key)**, em formato **ZIP**.
     **Não baixe o instalador `.exe`.**
   - Escolha o pacote conforme o **Tipo de sistema** mostrado em
     **Configurações → Sistema → Sobre**: **64-bit** para processador x64,
     **32-bit** para Windows de 32 bits ou **ARM64** para Windows em ARM.
     Os pacotes sem instalador são identificados por `noinst` no nome
     (por exemplo, `httrack_x64-noinst-3.50.5.zip` para x64).
   - No Explorador de Arquivos, clique com o botão direito no ZIP e escolha
     **Extrair Tudo…**. Extraia para uma pasta em que seu usuário possa gravar,
     como `Documentos\Ferramentas\WinHTTrack`. Não use `C:\Program Files`.
   - Abra a pasta extraída e, dentro da subpasta **`httrack`** do pacote 3.50.5,
     execute **`WinHTTrack.exe`** com um duplo clique.
     Não use **Executar como administrador**. Se desejar, crie um atalho para
     esse arquivo na Área de Trabalho. Mantenha os demais arquivos do pacote
     junto dele; não execute de dentro do ZIP.

> ⚠️ **Dependência e máquinas do laboratório:** o ZIP oficial requer o
> **Microsoft Visual C++ Redistributable 2015–2022** da arquitetura correspondente
> ao pacote. Ele deve estar disponível na máquina antes da aula; sua instalação
> pode exigir administrador. Se aparecer erro de DLL ausente (como
> `VCRUNTIME140.dll` ou `MSVCP140.dll`), avise o professor ou o suporte de TI.
> Não baixe DLLs avulsas. Se houver pedido de elevação ou bloqueio por política
> da instituição, cancele e peça orientação; não tente contornar a restrição.

> 💡 Depois de instalar o Git, feche e abra novamente o VS Code e o PowerShell
> para que reconheçam o comando `git`. Todo o roteiro abaixo usa Windows.

---

## Passo 2 — Obter os materiais e acessar o site-alvo público

Neste laboratório há **dois repositórios com papéis diferentes**:

- [laboratorio-phishing](https://github.com/SegurancaSistemasInternet/laboratorio-phishing):
  contém este roteiro e a versão marcada para treino. Você o clona com Git.
- [site-alvo](https://github.com/SegurancaSistemasInternet/site-alvo):
  contém o portal fictício publicado pelo professor no GitHub Pages.
  **Não o clone com Git:** o objetivo é copiar o site servido pela URL pública
  usando o WinHTTrack, como um navegador acessaria o site.

**URL pública prevista do alvo:** https://segurancasistemasinternet.github.io/site-alvo/

> 📌 **Antes da aula:** o professor deve publicar o alvo e confirmar essa URL.
> Se ela retornar 404 ou o professor informar outro endereço, não prossiga até
> receber a URL pública correta. O endereço `github.com/.../site-alvo` é o
> repositório de código, **não** a URL que deve ser usada no WinHTTrack.

1. Abra o **PowerShell** na pasta em que deseja guardar o laboratório (pelo
   Explorador de Arquivos, clique com o botão direito na pasta e escolha
   **Abrir no Terminal**; selecione PowerShell, se necessário).
2. Clone apenas o repositório de materiais:

   ```powershell
   git clone https://github.com/SegurancaSistemasInternet/laboratorio-phishing.git
   ```

3. No VS Code, use **File → Open Folder** (**Arquivo → Abrir Pasta**) e selecione
   a pasta **`laboratorio-phishing`** criada pela clonagem.
4. No **navegador**, abra a URL pública do alvo informada acima e confira se
   aparece a tela de login do *Portal Acadêmico Fictícia S.A.* e a página **Sobre**.
   Use apenas dados fictícios; o alvo não autentica nem coleta credenciais.
5. Anote a **URL base completa**, terminando em `/`, e faça uma captura da página
   para comparação posterior. Se houver redirecionamento, confirme o endereço
   final com o professor antes de usá-lo no WinHTTrack.
6. Mantenha o acesso à internet durante o espelhamento. **Não inicie Go Live
   para servir o alvo**: ele já está hospedado publicamente no GitHub Pages.
   O Go Live será usado somente para visualizar sua cópia nos próximos passos.

> 🔎 **Observe o alvo com calma.** Esta é a página "legítima" do nosso cenário.
> Repare no host (`segurancasistemasinternet.github.io`), no caminho
> `/site-alvo/` e no **HTTPS**. Daqui a pouco vamos comparar com a cópia.
> Ser público não torna o alvo uma instituição real nem autoriza copiar outros
> sites: o escopo continua sendo apenas o portal fictício indicado pelo professor.

---

## Passo 3 — Espelhar o site com o HTTrack

Agora o passo central: criar a **versão estática** (o espelho).

### No WinHTTrack 3.50.5 (com janelas)

Os nomes abaixo seguem a **interface em inglês** do pacote oficial 3.50.5,
conferidos nos recursos e nas capturas que acompanham essa versão. A aplicação
pode iniciar no idioma do Windows. Para acompanhar os mesmos nomes, use
**Preferences → Language preference...** (em português brasileiro,
**Opções → Preferências do idioma...**) e selecione **English**.
Se for solicitado, reabra o programa. Os botões de navegação também podem
seguir o idioma do Windows.

1. Abra **`WinHTTrack.exe`** (ou seu atalho). Na tela **Welcome to WinHTTrack
   Website Copier!**, clique em **Next >**.
2. Na tela do projeto, preencha:
   - **New project name:** `espelho-portal`. Use um nome novo; selecionar um
     projeto existente pode carregar as configurações de uma cópia anterior.
   - **Project category:** pode ficar em branco.
   - **Base path:** escolha uma pasta **fora do repositório clonado** e em que
     seu usuário possa gravar, por exemplo,
     `C:\Users\SEU-USUARIO\Documents\Espelhos` (substitua pelo caminho real da
     sua pasta). Use o botão **`..`** ao lado do campo para procurar a pasta.
     O WinHTTrack acrescenta o nome do projeto: nesse exemplo, a cópia ficará
     em `C:\Users\SEU-USUARIO\Documents\Espelhos\espelho-portal`.

   Clique em **Next >**.
3. Na tela de endereços, em **Action:**, selecione **Download web site(s)**.
   Não escolha as opções de atualização, retomada ou download de arquivos isolados.
4. Na caixa grande **Web Addresses: (URL)**, cole a **URL pública base**
   confirmada no Passo 2:
   `https://segurancasistemasinternet.github.io/site-alvo/`.
   Não é necessário usar **Add URL...**; a URL pode ser digitada diretamente.
   Deixe **URL list (.txt):** em branco. Não use a URL do repositório no GitHub
   nem um endereço `localhost`.
5. Clique em **Set options...** e abra a aba **Scan Rules**. Na caixa de texto
   de regras, apague o conteúdo existente e cole estas duas linhas,
   **nesta ordem**:

   ```text
   -*
   +https://segurancasistemasinternet.github.io/site-alvo/*
   ```

   A primeira exclui todas as URLs; a segunda permite somente o alvo e seus
   arquivos. Assim, a cópia não segue links para outros projetos do mesmo host
   nem para sites externos. Se o professor mudar a URL base, ajuste também a
   segunda regra, preservando a `/` antes de `*`. Não amplie a permissão para
   todo o `github.io`.

   Não é necessário usar **Exclude link(s)..** nem **Include link(s)..**:
   esses botões abrem assistentes para montar regras. Deixe desmarcadas as
   caixas de grupos de arquivos acima do campo, para não acrescentar permissões
   globais. Na aba **Limits**, mantenha os valores padrão para nosso site
   pequeno. Clique em **OK** para voltar à tela de endereços.
6. Clique em **Next >** para chegar à tela de início do espelhamento:
   - Mantenha selecionada a primeira opção, que começa com
     **Please adjust connection parameters if necessary...**.
   - Em **Remote connect → Connect to this provider**, mantenha
     **Do not use remote access connection** para a conexão de rede já ativa.
   - Deixe **Disconnect when finished** e **Shutdown PC when finished**
     desmarcados e não programe um horário em **On hold**.
   - **Não selecione Save settings only, do not launch download now.** Essa
     opção só salva a configuração e não baixa o site.

   Clique em **Finish** para iniciar o download. Esse botão inicia a operação;
   não significa que a cópia já terminou.
7. Aguarde a tela **Site mirroring finished!**. Confira os arquivos baixados e
   use **View log** para examinar avisos ou erros. Se não houver páginas HTML
   ou o download falhar, confira a URL e as regras antes de prosseguir;
   não remova a restrição de escopo para tentar resolver.

### O que observar enquanto ele roda

Preste atenção nas mensagens: o HTTrack baixa `index.html`, depois descobre o link
para `sobre.html` e o segue, depois pega `css/estilo.css`. **É a aranha seguindo
os links** — exatamente como um atacante captura as páginas de um site.

Ao terminar, use **Arquivo → Abrir Pasta** no VS Code para abrir a pasta
`espelho-portal`, dentro do **Base path** escolhido. Abra o `index.html` dessa
pasta e clique em **Go Live**. Esse arquivo pode ser o índice gerado pelo
HTTrack; nesse caso, clique no link do portal para chegar à página espelhada.
Não abra apenas a pasta-base `Espelhos`, que pode conter outros projetos.

> 📖 **Referência da interface:** [guia oficial do WinHTTrack](https://www.httrack.com/html/guide.html).
> Os rótulos e a sequência acima foram conferidos também no
> [código oficial da versão 3.50-5](https://github.com/xroche/httrack-windows/tree/3.50-5/WinHTTrack)
> e nos arquivos de idioma do ZIP 3.50.5.

> 🧪 **Experimento rápido:** confira que a cópia agora abre em
> `http://127.0.0.1:5500/` (ou na porta escolhida pelo Go Live), e não no endereço
> público original. Se permitido na máquina, desconecte temporariamente a
> internet e recarregue **a cópia local**: confira se as páginas e o CSS baixados
> continuam funcionando. Reconecte antes de publicar. O alvo público permanece
> no ar; você não precisa desligá-lo nem ter acesso ao servidor dele.

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
   (`https://segurancasistemasinternet.github.io/site-alvo/`, ou a URL confirmada
   no Passo 2)? Compare o host e o caminho completos; duas páginas podem estar
   no GitHub Pages sem pertencer ao mesmo responsável.
2. **O cadeado te enganou?** Tanto o alvo quanto a cópia publicada usam HTTPS.
   O que ele realmente garante? Por que isso não distingue a página original
   da cópia?
3. **O que o espelho NÃO conseguiu copiar?** Funções que dependem do servidor
   (um login que de fato valida a senha, por exemplo) não existem na cópia. Que
   pistas isso deixa? **Neste cenário, nem o alvo autentica usuários:** ele
   também é estático. Portanto, um login sem funcionar não prova, sozinho,
   que a página é uma cópia.
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
Pages, ou exclua **seu repositório de treino**. Não altere o repositório nem a
publicação do alvo do professor. Não há motivo para manter uma página de
phishing (mesmo simulado) no ar depois da aula.

Pare também o Live Server clicando no indicador **Port: …** na barra de status
do VS Code.

---

## 📂 Estrutura deste repositório

```
laboratorio-phishing/
├── README.md                 ← este roteiro
├── site-alvo/                ← material-fonte de apoio ao professor (não espelhar localmente)
│   ├── index.html
│   ├── sobre.html
│   └── css/estilo.css
├── exemplo-espelho/          ← a cópia PRONTA e marcada (referência / publicar)
│   ├── index.html            ← com faixa de treino + formulário educativo
│   ├── sobre.html
│   └── css/estilo.css
└── docs/
    ├── checklist-analise-defensiva.md
    └── publicar-no-github-pages.md
```

O alvo usado pelos alunos é servido pelo **repositório separado**
[SegurancaSistemasInternet/site-alvo](https://github.com/SegurancaSistemasInternet/site-alvo),
via GitHub Pages. A pasta [`site-alvo/`](site-alvo/) acima é mantida apenas como
material-fonte para o professor preparar essa publicação; não é necessário
abri-la com Go Live nem enviá-la ao repositório de treino do aluno.

---

## 📜 Aviso de uso

Material **exclusivamente educacional**, para uso em ambiente controlado de ensino.
As técnicas aqui demonstradas devem ser usadas **somente** sobre o alvo fictício
fornecido. Aplicá-las contra sites, sistemas ou pessoas reais, sem autorização
expressa, é ilegal e antiético. Ao usar este material, você concorda em mantê-lo no
escopo defensivo e didático para o qual foi criado.
