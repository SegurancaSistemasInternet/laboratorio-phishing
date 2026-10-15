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
| Passo 1–2 | 20 min | Preparar Windows; clonar os materiais; acessar o alvo público |
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
- [ ] Teste o **WinHTTrack ZIP sem instalador** indicado no
      [Passo 1 do roteiro](../README.md#passo-1--preparar-o-ambiente-windows),
      usando uma **conta de aluno sem privilégios de administrador**. Confirme
      que ela consegue extrair o pacote, abrir `WinHTTrack.exe` e salvar o espelho
      em uma pasta do próprio usuário.
- [ ] Confirme com o suporte de TI que o **Microsoft Visual C++ Redistributable
      2015–2022**, na arquitetura do pacote escolhido, está disponível nas máquinas.
      Se faltar, peça ao suporte que o instale antes da aula. O ZIP dispensa a
      instalação do WinHTTrack, mas não essa dependência nem as políticas de
      execução da instituição.
- [ ] Confirme que todos conseguem clonar o repositório localmente com `git clone`.
- [ ] Tenha algumas contas GitHub de reserva, caso alguém não consiga criar na hora.
- [ ] Publique o portal fictício no repositório separado
      [SegurancaSistemasInternet/site-alvo](https://github.com/SegurancaSistemasInternet/site-alvo),
      conforme as instruções abaixo. Confirme o acesso público antes da aula.
- [ ] Cada aluno espelha **a URL pública do alvo**, e não o repositório de código
      nem um servidor local. Teste as regras do WinHTTrack do
      [Passo 3](../README.md#passo-3--espelhar-o-site-com-o-httrack): somente a
      URL base autorizada e seus arquivos devem ser baixados.

## Preparar o alvo público no GitHub Pages

Esta etapa é do **professor**, antes da aula. Os alunos não precisam de acesso
de escrita ao alvo.

1. Crie ou prepare o repositório público
   [SegurancaSistemasInternet/site-alvo](https://github.com/SegurancaSistemasInternet/site-alvo).
   Copie o **conteúdo** da pasta de apoio [`site-alvo/`](../site-alvo/) deste
   laboratório para a raiz dele: `index.html`, `sobre.html` e `css/estilo.css`.
   Não envie a pasta contêiner, para evitar um caminho extra `/site-alvo/`.
2. Confirme que a identidade continua fictícia e que o formulário não envia
   dados nem autentica usuários. Use somente dados fictícios nos testes.
3. Em **Settings → Pages**, escolha **Deploy from a branch**, branch `main`
   e pasta `/ (root)`. Salve e aguarde a conclusão da publicação.
4. Confira a URL exibida pelo GitHub. O endereço previsto é
   `https://segurancasistemasinternet.github.io/site-alvo/`.
   Abra-o sem autenticação e teste **Início**, **Sobre** e o carregamento do CSS.
   Se houver 404, resolva a publicação antes da atividade.
5. Se a URL efetiva for diferente, atualize o endereço do alvo e a regra de
   inclusão no [roteiro](../README.md). Mantenha os recursos dentro da URL base,
   com links relativos, para que a regra restritiva também permita baixar o CSS.
6. Teste o espelhamento completo a partir da rede dos alunos. Se necessário,
   escalone o início dos downloads para evitar muitas requisições simultâneas.
   O Live Server só deve servir **as cópias locais**, não o alvo.

No encerramento, cada aluno despublica apenas a própria cópia de treino.
A manutenção ou retirada do alvo público é responsabilidade do professor.

## Erros comuns e como contornar

| Sintoma | Causa provável | Solução |
|---|---|---|
| Go Live não aparece | extensão ausente ou pasta não aberta | instale Live Server (`ritwickdey.LiveServer`) e abra a pasta clonada no VS Code |
| WinHTTrack pede administrador ou é bloqueado | uso do instalador `.exe` ou política da instituição | cancele; confira se foi baixado o ZIP `noinst` e extraído em uma pasta do usuário; se a política bloquear a execução, acione o suporte de TI |
| WinHTTrack não abre e informa DLL ausente | Visual C++ Redistributable ausente ou incompatível com a arquitetura do pacote | peça ao suporte de TI que instale a dependência oficial; não baixe DLLs avulsas |
| Alvo público retorna 404 | Pages ainda não publicado ou URL incorreta | professor deve conferir Settings → Pages no repositório separado; não substituir por um site real |
| HTTrack baixa "nada" ou erro de conexão | URL do repositório em vez do Pages, falha de rede ou regra incorreta | teste a URL pública no navegador e confira a URL base e a ordem das regras do Passo 3; não desative as restrições |
| A cópia abre sem estilo (sem cores) | caminho do CSS quebrou | abra a pasta gerada no VS Code e visualize o `index.html` com Go Live; não mova arquivos soltos |
| GitHub Pages dá 404 | arquivos não estão na raiz, ou Pages ainda processando | `index.html` tem que estar no topo do repo; espere ~1 min |
| Página publicada sem a faixa de treino | publicaram o clone cru, não o `exemplo-espelho/` | reforce: publica-se a versão **marcada** |

## Perguntas para fomentar a discussão (Passo 6)

- Se o cadeado não garante confiança, para que ele serve?
- O alvo também é estático e não autentica: por que um login sem funcionar não
  basta para identificar uma cópia?
- Alvo e cópia publicada têm HTTPS: quais diferenças de host e caminho permitem
  distinguir seus endereços?
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
  também funciona comparando o **alvo público** com a versão marcada no
  **Go Live** do VS Code. Apenas a publicação da cópia pelo aluno é dispensada;
  o alvo continua no GitHub Pages. Publicar a cópia permite comparar duas URLs
  com HTTPS e discutir o cadeado "enganoso".
