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
| Passo 1–2 | 20 min | Instalar HTTrack; subir o site-alvo local |
| Passo 3 | 25 min | Espelhar com HTTrack; observar a aranha seguindo links |
| Passo 4–5 | 30 min | Travas de segurança; publicar no GitHub Pages |
| Passo 6 | 25 min | **Análise defensiva** (o clímax pedagógico) |
| Encerramento | 5 min | Despublicar; recado final |

> ⏱️ Se o tempo apertar, **não corte o Passo 6**. A técnica é o meio; a defesa é o
> fim. Prefira abreviar a publicação (podem publicar em casa) a abreviar a análise.

## Pré-aula: checklist do professor

- [ ] Teste o fluxo inteiro você mesmo uma vez, do zero.
- [ ] Confirme que o `python -m http.server` funciona nas máquinas do laboratório.
- [ ] Verifique se os alunos conseguem instalar o HTTrack (ou pré-instale).
- [ ] Tenha algumas contas GitHub de reserva, caso alguém não consiga criar na hora.
- [ ] Se a rede do laboratório bloquear `localhost` entre máquinas, não tem
      problema: cada aluno espelha o próprio `localhost:8000` na própria máquina.

## Erros comuns e como contornar

| Sintoma | Causa provável | Solução |
|---|---|---|
| HTTrack baixa "nada" ou erro de conexão | servidor local não está rodando | confirme o `python -m http.server 8000` ativo e o terminal aberto na pasta `site-alvo/` |
| A cópia abre sem estilo (sem cores) | caminho do CSS quebrou | abra pelo `index.html` dentro da pasta gerada, não mova arquivos soltos |
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
- **Sem GitHub:** a análise defensiva funciona igual abrindo a cópia local; o Pages
  é só para demonstrar o HTTPS/cadeado "enganoso".
