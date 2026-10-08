# 👩‍🏫 Guia do Professor

Material de apoio para conduzir o laboratório de phishing. Leia antes da aula.

## Objetivos de aprendizagem

Ao final, o aluno deve ser capaz de:

- Explicar as três peças de um ataque de phishing (isca, página falsa, colheita).
- Usar um web crawler (HTTrack) para gerar a versão estática de um site.
- Publicar um site estático no GitHub Pages.
- **Reconhecer** sinais de phishing e enunciar as defesas práticas.

## Enquadramento ético (abra a aula com isto)

Este laboratório mostra a *construção* da isca para ensinar a *defesa*. Deixe
explícitas, logo no início, as três travas:

1. **Alvo fictício** — nunca um site real.
2. **Página marcada** como treino.
3. **Zero coleta de dados** — o formulário educa, não rouba.

Vale a analogia com outras áreas: aprende-se sobre venenos para tratar
intoxicações, não para envenenar. A diferença entre o profissional de segurança e
o criminoso é **consentimento, escopo e intenção**. Reforce que espelhar o site de
uma instituição real e publicá-lo é crime no Brasil, mesmo sem "colher" dados.

## Tempo sugerido (aula de ~2h)

| Bloco | Tempo | Conteúdo |
|---|---|---|
| Abertura | 15 min | O que é phishing; as três peças; regras do lab |
| Passo 1–2 | 20 min | Preparar Windows; clonar o repositório; abrir o alvo com Go Live |
| Passo 3 | 25 min | Espelhar com HTTrack; observar a aranha seguindo links |
| Passo 4–5 | 30 min | Travas de segurança; publicar no GitHub Pages |
| Passo 6 | 25 min | **Análise defensiva** (o clímax pedagógico) |
| Encerramento | 5 min | Despublicar; recado final |

> ⏱️ Se o tempo apertar, **não corte o Passo 6**. A técnica é o meio; a defesa é o
> fim. Prefira abreviar a publicação (podem publicar em casa) a abreviar a análise.

## Pré-aula: checklist do professor

- [ ] Teste o fluxo inteiro você mesmo uma vez, do zero.
- [ ] Confirme que as máquinas usam **Windows** e têm **Git for Windows** e **VS Code**.
- [ ] Instale a extensão **Live Server**, de **Ritwick Dey**, e teste o botão **Go Live**.
- [ ] Verifique se os alunos conseguem instalar o **WinHTTrack** (ou pré-instale).
- [ ] Confirme que todos conseguem clonar o repositório localmente com `git clone`.
- [ ] Tenha algumas contas GitHub de reserva, caso alguém não consiga criar na hora.
- [ ] Cada aluno espelha o próprio `http://127.0.0.1:5500/site-alvo/` na própria
      máquina, sem precisar acessar o computador de colegas. Confira a porta
      efetiva do Go Live e as regras do WinHTTrack que limitam a cópia a `site-alvo/`.

## Erros comuns e como contornar

| Sintoma | Causa provável | Solução |
|---|---|---|
| Go Live não aparece | extensão ausente ou pasta não aberta | instale Live Server (`ritwickdey.LiveServer`) e abra a pasta clonada no VS Code |
| HTTrack baixa "nada" ou erro de conexão | Live Server parado ou URL incorreta | ative Go Live e confira no navegador a porta efetiva e o caminho `site-alvo/` |
| A cópia abre sem estilo (sem cores) | caminho do CSS quebrou | abra a pasta gerada no VS Code e visualize o `index.html` com Go Live; não mova arquivos soltos |
| GitHub Pages dá 404 | arquivos não estão na raiz, ou Pages ainda processando | `index.html` tem que estar no topo do repo; espere ~1 min |
| Página publicada sem a faixa de treino | publicaram o clone cru, não o `exemplo-espelho/` | reforce: publica-se a versão **marcada** |

## Perguntas para fomentar a discussão (Passo 6)

- Se o cadeado não garante confiança, para que ele serve?
- Por que uma cópia estática não consegue "logar" de verdade? Que pistas isso dá?
- Que tipo de mensagem faria *você* clicar num link sem pensar?
- Como o 2FA protege mesmo quando a senha vaza?
- Onde, no seu dia a dia, você digita senhas vindas de links? (reflexão pessoal)

## Avaliação sugerida

Peça um relatório curto (1 página) com:
1. Um print da página de treino publicada (com a faixa visível).
2. A URL publicada e a análise de **três** diferenças em relação ao site oficial.
3. A lista de defesas que o aluno passou a adotar.

Critério central: o aluno demonstrou que sabe **reconhecer e evitar** phishing?

## Variações

- **Turma mais avançada:** adicionar inspeção de certificado (quem emitiu, validade),
  comparação de DNS/WHOIS do domínio, e análise de cabeçalhos de e-mail de phishing
  reais (anonimizados).
- **Sem publicação no GitHub:** após clonar o repositório, a análise defensiva
  também funciona visualizando a versão marcada com **Go Live** no VS Code.
  O Pages serve para demonstrar o HTTPS/cadeado "enganoso".
