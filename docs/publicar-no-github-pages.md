# 🚀 Publicar no GitHub Pages — passo a passo com imagens mentais

Guia detalhado do Passo 5, para quem nunca usou o GitHub Pages.

## O que é GitHub Pages?

Um serviço **gratuito** do GitHub que pega os arquivos HTML/CSS de um repositório e
os serve como um site de verdade, com endereço público e HTTPS. Perfeito para
sites estáticos — como o nosso espelho.

## Antes de começar

Você vai publicar o conteúdo da pasta **`exemplo-espelho/`** (a versão marcada como
treino), **não** o clone cru que o HTTrack gerou.

---

## Opção A — Pela interface web (sem terminal)

1. **Crie o repositório.** No GitHub, clique no `+` (canto superior direito) →
   *New repository*.
   - **Repository name:** `treino-phishing-ficticia`
   - Visibilidade: **Public**
   - Marque *Add a README* (pode ser qualquer coisa; sobrescrevemos depois)
   - *Create repository*

2. **Envie os arquivos.** No repositório → *Add file* → *Upload files*.
   - Arraste `index.html`, `sobre.html` e a pasta `css/` de dentro de
     `exemplo-espelho/`.
   - ⚠️ **Importante:** o `index.html` precisa ficar na **raiz** do repositório
     (não dentro de uma subpasta), senão o Pages dá 404.
   - Escreva uma mensagem de commit (ex.: "Publica pagina de treino") → *Commit changes*.

3. **Ative o Pages.** *Settings* (aba do repositório) → menu lateral *Pages*.
   - Em **Source**, selecione *Deploy from a branch*.
   - **Branch:** `main`, pasta `/ (root)` → *Save*.

4. **Pegue a URL.** Recarregue a página após ~1 minuto. Aparecerá:
   > Your site is live at `https://SEU-USUARIO.github.io/treino-phishing-ficticia/`

   Clique e confira. Deve aparecer a página de treino **com a faixa vermelha**.

---

## Opção B — Pelo PowerShell no Windows

Abra o **PowerShell na raiz da cópia local `laboratorio-phishing`**, clonada no
Passo 2. Substitua `SEU-USUARIO` pelo seu usuário do GitHub. O repositório de
publicação deve estar criado no GitHub com a branch `main` (marque **Add a README**
ao criá-lo, como na Opção A).

```powershell
# 1. Clone o repositório de publicação
git clone https://github.com/SEU-USUARIO/treino-phishing-ficticia.git

# 2. Copie o conteúdo da versão marcada para a raiz dele
Copy-Item -Path .\exemplo-espelho\* -Destination .\treino-phishing-ficticia\ -Recurse
Set-Location .\treino-phishing-ficticia

# 3. Confirme que index.html está na raiz
Get-ChildItem   # deve listar: index.html, sobre.html, css (além do README)

# 4. Envie
git add .
git commit -m "Publica pagina de treino de phishing (didatico)"
git push origin main
```

Execute os comandos um de cada vez e só avance se o anterior concluir sem erro.
No primeiro commit, se o Git solicitar sua identidade, configure seu nome e
e-mail conforme a orientação do professor. No `git push`, conclua a autenticação
do GitHub quando solicitada.

Depois, ative o Pages em *Settings → Pages* como na Opção A, passo 3.

---

## A lição do cadeado (não pule)

Ao abrir a URL do `github.io`, repare: **tem cadeado e HTTPS**. A sua página de
treino — tecnicamente idêntica a uma de phishing — exibe o cadeadinho "de
seguro".

Isso é a prova prática de que **o cadeado não diz que o site é confiável**. Ele só
cifra a conexão. Um golpista obtém HTTPS em minutos e de graça. A pergunta que
protege a vítima nunca é *"tem cadeado?"*, e sim *"a URL é mesmo a oficial?"*.

---

## 🧹 Despublicar (faça ao fim da atividade)

Não deixe a página no ar depois da aula:

- **Settings → Pages → Source → Disable**, ou
- **Settings → (final da página) → Delete this repository**.

---

## Problemas comuns

| Problema | Solução |
|---|---|
| **404** ao abrir a URL | `index.html` não está na raiz, ou o Pages ainda está processando (espere 1–2 min) |
| Página **sem estilo** | a pasta `css/` não foi enviada, ou veio para subpasta errada |
| Faixa de treino **sumiu** | você publicou o clone cru em vez do `exemplo-espelho/` |
| URL não aparece em Settings → Pages | a branch selecionada não tem os arquivos; confirme que o push foi para `main` |
