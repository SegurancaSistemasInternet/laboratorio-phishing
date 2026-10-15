# 🛡️ Checklist de Análise Defensiva

Use este checklist no **Passo 6** do laboratório — e depois, na vida real, sempre
que uma tela de login chegar por um link.

## Diante de uma tela de login, pergunte:

### 1. A URL

- [ ] O **domínio** é exatamente o oficial da instituição? (leia da direita do
      domínio para a esquerda: `...github.io` ≠ `...facul.edu.br`)
- [ ] Há letras trocadas, hífens ou palavras a mais? (`faculdade-login.com`,
      `facu1dade.com` com "1" no lugar de "l")
- [ ] O endereço tem subdomínios estranhos antes do nome real?
      (`facul.edu.br.algumacoisa.com` — o domínio real é `algumacoisa.com`!)
- [ ] No laboratório, compare com a **URL pública do alvo** confirmada pelo
      professor no [Passo 2](../README.md#passo-2--obter-os-materiais-e-acessar-o-site-alvo-público).
      Confira o host completo e o caminho: compartilhar o sufixo `github.io`
      não significa compartilhar o responsável.

### 2. O cadeado / HTTPS

- [ ] Tem cadeado? **Isso NÃO prova nada sobre confiança.** Cadeado = conexão
      cifrada; qualquer site, inclusive golpista, consegue um.
- [ ] A pergunta certa não é "tem cadeado?", e sim **"o domínio é o certo?"**

### 3. Como você chegou aqui

- [ ] Você **clicou num link** de e-mail/mensagem para chegar nesta tela? 🚩
      (Esse é o vetor nº 1 de phishing.)
- [ ] A mensagem criava **urgência** ("bloqueio", "última chance", "em 24h")?
- [ ] O remetente é realmente quem diz ser? (o nome exibido é fácil de forjar)

### 4. Detalhes da página

- [ ] Há imagens quebradas, links que não funcionam, textos estranhos?
      (cópias estáticas costumam deixar rastros)
- [ ] Alguma função "não funciona de verdade"? Isso pode ser uma pista, mas não
      uma prova: neste laboratório, o alvo original também é estático e não
      autentica usuários. Não teste com credenciais reais.
- [ ] O português/inglês está impecável, ou há erros sutis?

---

## ✅ As defesas que ficam para a vida

1. **Nunca digite senha a partir de um link recebido.** Acesse digitando o
   endereço oficial você mesmo, ou por um favorito que você salvou.
2. **Confira a URL inteira** antes de digitar qualquer coisa.
3. **Desconfie de urgência.** Golpes correm; instituições sérias dão tempo.
4. **Cadeado não é selo de confiança.** HTTPS é o piso, não o teto.
5. **Ative o 2FA** (verificação em duas etapas) onde for possível.
6. **Na dúvida, pare e confirme** por um canal oficial (telefone do site real,
   app oficial, presencialmente).

---

> 💬 **Regra de ouro:** *um link nunca é motivo suficiente para digitar uma senha.*
> Se a mensagem quer que você faça login, abra o site do seu jeito, não pelo link.
