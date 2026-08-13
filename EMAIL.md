# A entrega por e-mail — a parte que ninguém te conta

Fazer a rotina *escrever* o briefing é a parte fácil. Fazer ele **chegar na caixa de
entrada** é onde a maioria dos tutoriais para de funcionar — porque o problema não é
técnico, é de **reputação**, e reputação não aparece em nenhum log de erro.

Tudo aqui foi testado ponta a ponta em agosto de 2026. O que não foi testado está marcado
como não testado.

---

## Por que Resend, e não o Gmail

> **O conector do Gmail não envia e-mail.** Ele cria **rascunho**. E o rascunho nasce na
> caixa de quem criou a rotina — não vai para lugar nenhum sozinho.

Para envio automático de verdade você precisa de um serviço transacional. O Resend é o que
tem conector pronto e plano gratuito suficiente para uma newsletter pessoal.

---

## ⛔ As três coisas que quebram e por quê

### 1. O remetente de teste não entrega para outra pessoa

O Resend te dá um remetente de sandbox (`onboarding@resend.dev`) que funciona de imediato
— e é uma armadilha:

- ele só entrega **para você mesmo**, o dono da conta;
- e mesmo para você, **cai em Spam**. O Gmail carimba literalmente *"semelhante a mensagens
  identificadas anteriormente como spam"* — porque milhares de pessoas testam com esse
  mesmo remetente.

Se você quer mandar para outra pessoa (um sócio, um primo, uma lista), sandbox não serve.
Ponto final. **Domínio verificado não é boa prática aqui, é pré-requisito.**

### 2. "Entregue" não significa "chegou na caixa de entrada"

O detalhe mais perigoso deste guia inteiro:

> O painel do Resend reporta **`delivered`** mesmo quando o Gmail joga a mensagem em Spam.

Isso não é bug — a pasta é decisão do destinatário e é **invisível para o remetente**. Ou
seja: **a sua rotina nunca vai saber que falhou.** Ela vai reportar sucesso todo dia
enquanto ninguém lê nada. Você descobre semanas depois, quando alguém comenta "aquilo nunca
chegou".

Consequência prática: **nos primeiros dias, confira a caixa de verdade.** Se estiver
mandando pra outra pessoa, peça um print. Não confie no painel.

### 3. O Gmail destrói o seu CSS

Testado nos dois casos, sem margem de dúvida:

| No HTML | No Gmail |
|---|---|
| `<style>` dentro do `<head>` | ❌ **descartado inteiro** — chega texto cru |
| `style="..."` inline em cada tag | ✅ sobrevive |

Por isso o modelo de [PROMPT.md](PROMPT.md) exige CSS inline em maiúsculas. E por isso a
variação "teaser curto + página completa" é tão conveniente: no e-mail você inlina 10 tags à
mão; o conteúdo longo vive na página, onde `<style>` funciona normalmente.

---

## O caminho certo, na ordem

### 1. Escolha o domínio de envio — e use um **subdomínio**

Você precisa de um domínio próprio. `@gmail.com` **não pode** ser remetente: serviços
transacionais só enviam de domínio verificado por você, e você não controla o DNS do Gmail.

Use um **subdomínio dedicado**:

```
✅  mail.seudominio.com        ← envio de newsletter
❌  seudominio.com             ← a raiz, onde vive o seu e-mail de trabalho
❌  seuapp.com                 ← o domínio de um produto seu
```

**Por quê:** reputação de envio é por domínio. Se a newsletter der problema — e
newsletter pessoal dá, você vai testar coisas —, o estrago fica isolado no subdomínio. A
raiz, por onde passam os seus e-mails que importam, não é contaminada. E jamais use o
domínio de um app: o dia em que a newsletter for marcada como spam é o dia em que o e-mail
de "recuperar senha" do seu produto para de chegar.

### 2. Verifique o domínio no painel do Resend

No painel web (não pelo conector — ver o aviso abaixo), adicione o domínio. O Resend
devolve um punhado de registros DNS para você colar no seu provedor: chave **DKIM** (assina
suas mensagens), registro de **SPF** (autoriza o Resend a enviar em seu nome) e, opcional
mas recomendado, **DMARC** (diz aos servidores o que fazer quando algo não bater).

Propagação de DNS costuma levar de minutos a algumas horas. **Comece por aqui** — é o único
passo do tutorial inteiro em que você espera algo que não depende de você.

> ⚠️ **O conector pode não deixar você fazer isso pelo Claude.** Num teste real, o token
> do conector tinha permissão só para `send-email`: chamadas de `list-domains` e
> `get-email` bateram em `Access token is missing required scopes: full_access`. Não é
> problema — verificação de domínio é tarefa de painel, feita uma vez. Só não perca tempo
> tentando fazer pelo chat.

### 3. Só depois, aponte a rotina para o novo remetente

No bloco `ENTREGA` do prompt:

```
de: briefing@mail.seudominio.com
```

### 4. Nos primeiros dias, treine o filtro

Mesmo com domínio verificado, um remetente novo começa sem histórico. Ajude:

- marque **"não é spam"** na primeira mensagem que cair errado;
- crie um filtro `de: briefing@mail.seudominio.com` → **nunca enviar para spam**;
- responda ou arquive (interação positiva conta).

**Paliativo de 30 segundos** enquanto o DNS não propaga: o mesmo filtro, apontando para
`onboarding@resend.dev`. Funciona só para você mesmo, e é temporário.

---

## Cabe no plano gratuito?

Sim, com folga enorme. Números verificados em **12/08/2026** na página de preços do Resend
— confira antes de decidir, preço muda:

| | Grátis | Pro |
|---|---|---|
| Preço | **$0** | **$20/mês** |
| E-mails por mês | **3.000** | 50.000 |
| E-mails por dia | **100** | — |
| Domínios | **1** | 10 |
| Excedente | — | $0,90 por mil |

**Uma newsletter diária para você mesmo gasta ~30 e-mails por mês** — 1% do plano gratuito.

Se um dia virar lista: os dois limites se encontram em **~96 destinatários diários**
(3.000 ÷ 31 dias). Até aí, grátis.

**Duas pegadinhas:**

1. **Um domínio só** no plano gratuito. Se você já usa o Resend para outra coisa, o slot
   está ocupado — e dividir o mesmo domínio entre um app e uma newsletter é justamente o
   que a seção de subdomínio acima manda evitar.
2. **O custo real não está aqui.** Cada disparo é uma sessão do Claude lendo fontes e
   escrevendo texto: isso consome o uso da sua assinatura do Claude, não o seu limite de
   e-mail. O envio é a parte barata da conta.

---

## Não anexe PDF

Tentador — um "PDF matinal" bonito na caixa de entrada. Medido, e o número é feio:

- PDF de **243 KB** → **~332 mil caracteres** em base64 (o conector só aceita base64 ou URL);
- isso é dezenas de milhares de tokens **de ida**, todo dia, para sempre;
- forçar uma fonte única na impressão economizou só **20%** (243 → 193 KB). O peso é o
  navegador embutindo o subset da fonte, não a quantidade de famílias.

**Faça isto:** publique a edição como página e mande o link. Mesmo resultado visual, custo
próximo de zero, e ainda funciona no celular.

---

## Checklist antes de ligar o automático

- [ ] Domínio de envio é um **subdomínio** (`mail.…`), não a raiz e não o domínio de um app
- [ ] Domínio **verificado** no painel do Resend (DKIM + SPF; DMARC se der)
- [ ] O prompt manda **CSS inline**, e o HTML não tem `<style>` no `<head>`
- [ ] Rodou **na mão** pelo menos 3 vezes e o texto está do jeito que você lê
- [ ] Confirmou com os **próprios olhos** que chegou na **caixa de entrada** — não no painel
- [ ] Se for para outra pessoa: ela confirmou o recebimento, na pasta certa
- [ ] Tamanho da edição está em torno de {5 minutos de leitura}, não 8
- [ ] Nenhuma senha, chave ou token escrito dentro do prompt
- [ ] Cron convertido para **UTC** (Brasil = hora desejada **+3**)
- [ ] Teste do **computador desligado** feito
