# Newsletter matinal automática, sem código e sem servidor

Como montar uma rotina do Claude que **acorda sozinha todo dia**, lê as suas fontes
(Notion, agenda, e-mail, notícias), escreve um briefing e **manda no seu e-mail** —
com o seu computador desligado.

Sem GitHub Actions. Sem VPS. Sem cron na máquina. Sem uma linha de código.

> **Não é uma newsletter de blog.** É um briefing pessoal: os *seus* projetos, a *sua*
> agenda, os *seus* temas. O que nenhum produto pronto faz, porque nenhum produto tem
> acesso ao seu Notion e à sua cabeça ao mesmo tempo.

---

## Índice

| Arquivo | Pra quê |
|---|---|
| [COMECE-AQUI.md](COMECE-AQUI.md) | **O atalho:** um prompt pra colar no Claude e ele te guiar por tudo, perguntando |
| **README.md** (você está aqui) | O passo a passo, do zero até a primeira edição chegar |
| [PROMPT.md](PROMPT.md) | O prompt pronto pra copiar e preencher |
| [EMAIL.md](EMAIL.md) | A parte que ninguém te conta: **por que o e-mail cai em spam** e como resolver |
| [ARMADILHAS.md](ARMADILHAS.md) | Os erros já pagos — cada um com o sintoma e o conserto |

**Ordem sugerida:** leia este README inteiro (10 min), depois `EMAIL.md` **antes** de
configurar qualquer coisa. A parte de e-mail tem um pré-requisito que leva algumas horas
para ficar pronto (verificação de domínio) e você não quer descobrir isso no fim.

---

## Como isso funciona (o modelo mental)

A peça central chama **Cloud Routine**. É um agendamento que vive na infraestrutura da
Anthropic e dispara uma sessão do Claude num horário fixo. Essa sessão:

1. acorda **sem nenhuma memória** das suas conversas anteriores;
2. lê as fontes que você autorizou, via **conectores** (OAuth);
3. escreve o texto seguindo o seu prompt;
4. entrega no canal que você mandou;
5. morre.

```
   ⏰ cron (UTC)
        │
        ▼
   ☁️  sessão do Claude na nuvem  ──lê──▶  Notion · Agenda · Gmail · Web
        │
        └──escreve──▶  ✉️ Resend  ──▶  sua caixa de entrada
```

**As duas consequências que decidem tudo:**

> **1. A nuvem não enxerga o seu disco.**
> Apontar para uma pasta do seu computador é o erro que quebra a maioria das tentativas.
> A fonte precisa ser um **conector** (Notion, Google Drive, Gmail, Calendar) ou um
> **repositório git**. Se a sua base de conhecimento hoje é uma pasta local, suba ela
> antes.
>
> **2. A rotina não lembra de nada.**
> Ela não sabe quem você é, o que você faz, nem o que combinaram ontem. Tudo que ela
> precisa saber tem que estar escrito no prompt. Por isso o prompt é longo — e por isso
> um prompt curto devolve um briefing genérico e inútil.

---

## Antes de começar

| Você precisa de | Observação |
|---|---|
| **Conta Claude paga** | Rotinas agendadas não existem no plano gratuito |
| **Pelo menos uma fonte de leitura** | Notion, Google Drive, Gmail ou Google Calendar |
| **Uma saída de e-mail** | **Resend** — ver [EMAIL.md](EMAIL.md) |
| **Um domínio próprio** | Obrigatório para o e-mail chegar de verdade. Ver [EMAIL.md](EMAIL.md) |

> ⚠️ **O conector do Gmail não envia e-mail.** Ele só cria **rascunho**, e o rascunho
> nasce na caixa de quem criou a rotina. Para envio automático de verdade, é Resend.

---

## Passo a passo

### 1. Ligue os conectores

Vá em `claude.ai/customize/connectors` e conecte **duas categorias**:

- **De onde ele lê:** Notion, Google Drive, Gmail, Google Calendar — o que você usar.
- **Por onde ele entrega:** Resend.

Cada conector é um login OAuth seu. Isso é importante e tem um limite: **conector é
pessoal e não se empresta.** Se você quer que outra pessoa receba um briefing lendo o
Notion *dela*, a rotina tem que ser criada na conta *dela*. Você não consegue ler a base
de outra pessoa com o seu login.

### 2. Crie a rotina

Vá em `claude.ai/code/routines` → nova rotina.

Marque os conectores do passo 1 e deixe o campo de **pasta/repositório vazio**. É isso
que faz a rotina rodar na nuvem em vez de tentar rodar num contexto local.

### 3. Escreva o prompt

Copie o modelo de [PROMPT.md](PROMPT.md) e preencha os blocos marcados com `▸`.

Regra prática: **se você não escreveu, ela não sabe.** Nomes dos seus projetos, quais
campos ler, o que é "urgente" pra você, quantos caracteres cabem, o que fazer quando uma
fonte falha — tudo explícito.

### 4. Marque o horário — em **UTC**

O agendamento usa UTC, não o seu fuso. É o detalhe que mais gera briefing na hora errada.
Formato: `minuto hora * * *`.

| Você quer receber (Brasil, UTC−3) | Escreva |
|---|---|
| 6h00 | `0 9 * * *` |
| 6h30 | `30 9 * * *` |
| 7h00 | `0 10 * * *` |
| 7h00, só dias de semana | `0 10 * * 1-5` |

Dois detalhes medidos numa rotina em produção desde 03/08/2026:

- **Intervalo mínimo: 1 hora.** Mais frequente é rejeitado.
- **O disparo tem jitter de alguns minutos.** Um cron marcado em `:00` disparou às `:12`
  e agendou o próximo para `:05`. Se o horário for crítico ("antes da reunião das 9"),
  deixe 10 minutos de folga.

### 5. Rode uma vez na mão, antes de dormir

Não espere a manhã pra descobrir que errou. Use o botão de rodar agora, leia o que chegou,
ajuste o prompt, rode de novo. Três ou quatro voltas é normal.

**Ajuste o tamanho nesta fase.** Referência de um teste real: a primeira edição saiu com
**1.659 palavras ≈ 8,3 minutos de leitura**. Ninguém lê 8 minutos às 6h30. A meta boa é
**5 minutos, ~700 a 1.000 palavras** — e você chega lá cortando frentes (política de 3
assuntos pra 1) e rebaixando seções para semanais, não pedindo "seja mais curto".

### 6. Prove que está funcionando

Duas checagens honestas nos primeiros dias — as duas custam pouco e valem muito:

1. **Confira uma informação à mão.** Pegue um item do briefing e verifique na fonte. Se
   bater três dias seguidos, você pode confiar no resto.
2. **Desligue o computador.** Deixe desligado até depois do horário e veja se chegou. É a
   única prova de que roda na nuvem — e não uma coisa que só funcionava porque a sua
   máquina estava aberta.

E leia [EMAIL.md](EMAIL.md) antes de comemorar: **"entregue" no painel do Resend não
significa "chegou na caixa de entrada".**

---

## Sobre segurança

**A rotina não tem cofre de senha.** Não escreva chave de API, token ou senha dentro do
prompt — fica salvo em texto plano na configuração. Se um serviço precisa de credencial,
ele entra como **conector**, nunca como texto.

E dê à rotina o menor acesso possível. Se ela só precisa ler o Notion, diga isso no prompt
em maiúsculas (`SOMENTE LEITURA; NÃO crie nem edite páginas`) — o modelo respeita, e no dia
em que o prompt ganhar uma instrução ambígua, é essa linha que segura.

---

## Perguntas que aparecem sempre

**Preciso de GitHub Actions?**
Não. Actions é excelente pra rodar *código* — e aqui não há código pra rodar. Você trocaria
"editar um prompt" por "manter um repo com quatro integrações OAuth, chave de API e refresh
token" para ganhar exatamente nada. Além disso, o cron do Actions tem atraso documentado em
horário de pico.

**Funciona com o computador desligado?**
Sim, por arquitetura — roda na nuvem. Mas faça o teste do passo 6 mesmo assim: arquitetura
é argumento, teste é prova.

**Dá pra receber em PDF?**
Tecnicamente sim, mas **não faça**. Medido: um PDF de 243 KB vira ~332 mil caracteres em
base64, porque o conector só aceita base64 ou URL. São dezenas de milhares de tokens de ida
— todo dia, pra sempre. Forçar uma fonte só na impressão economizou apenas 20% (o peso é o
navegador embutindo o subset da fonte, não a família em si). O caminho certo está em
[EMAIL.md](EMAIL.md): **o conteúdo é uma página, o e-mail carrega o ponteiro.**

**E se uma fonte cair?**
Se o prompt não disser nada, o Claude pode simplesmente pular a seção — e você recebe um
briefing menor sem perceber que perdeu informação. O modelo de [PROMPT.md](PROMPT.md) força
o contrário: entrega mesmo assim, com a seção marcada `(fonte indisponível)`. Falha visível
é melhor que silêncio.

---

## Licença

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.pt-br) — use, adapte e publique,
citando a fonte.

Feito por [@alitocode](https://instagram.com/alitocode) · Ali Hamud.
