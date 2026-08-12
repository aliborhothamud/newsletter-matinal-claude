# Comece aqui — cole isto no seu Claude

Se você não quer ler o tutorial inteiro, tudo bem: **copie o bloco abaixo e cole numa
conversa nova com o Claude.** Ele vai te entrevistar, montar a rotina com você e te guiar
até a primeira edição chegar.

**Onde colar:** [claude.ai](https://claude.ai), conversa nova, conta paga.
**Quanto leva:** 5 a 10 minutos de conversa. Mais o tempo de DNS, se você for de e-mail.
**Você não precisa saber nada antes.** Ele pergunta.

---

## O prompt

```
Você vai me guiar na montagem de uma newsletter matinal automática: uma rotina
agendada (Cloud Routine) que roda todo dia de manhã na nuvem da Anthropic, lê as
minhas fontes e me entrega um briefing. Eu não sei nada disso ainda — me conduza
do zero. Responda sempre em português.

## COMO CONDUZIR ESTA CONVERSA
- Faça UMA pergunta por vez e espere minha resposta. No máximo 8 perguntas.
- Em toda pergunta, sugira uma opção padrão, para eu poder responder só "padrão".
- Não avance de fase sem eu responder. Não presuma resposta minha.
- Nunca me peça senha, chave de API ou token. Se algo precisar de credencial, ela
  entra como CONECTOR, nunca como texto no prompt.
- Se eu der uma resposta que não funciona, me diga na hora e por quê, em vez de
  seguir em frente. Prefiro parar agora do que descobrir depois.

## FATOS VERIFICADOS — não contradiga nenhum destes
Foram testados na prática. Se algo que eu pedir esbarrar num deles, me avise.
1. A rotina roda na nuvem e NÃO enxerga o meu computador. Fonte de leitura tem que
   ser um conector (Notion, Google Drive, Gmail, Google Calendar) ou um repositório
   git. Pasta local não funciona, mesmo que eu insista.
2. A rotina acorda SEM MEMÓRIA nenhuma da nossa conversa. Tudo que ela precisa
   saber tem que estar escrito dentro do prompt dela.
3. O conector do Gmail NÃO envia e-mail — ele só cria rascunho, na caixa de quem
   criou a rotina. Envio automático de verdade só com Resend.
4. O Resend só envia de domínio verificado por mim. O remetente de teste
   (onboarding@resend.dev) entrega SÓ para o dono da conta, e cai em spam.
5. O painel do Resend marca "delivered" mesmo quando a mensagem cai em spam. Nunca
   trate "delivered" como prova de que chegou.
6. O Gmail descarta bloco <style> do <head>. O HTML do e-mail precisa de CSS
   INLINE, em cada tag.
7. O agendamento é em UTC, não no meu fuso. Brasil = hora desejada + 3. Intervalo
   mínimo entre disparos: 1 hora. O disparo tem alguns minutos de atraso normal.
8. Não anexe PDF: vira base64 e custa dezenas de milhares de tokens por dia. Se eu
   quiser algo bonito, publique como página e mande o link.

## FASE 1 — DESCOBRIR (pergunte, uma por vez)
1. Onde vivem hoje os meus projetos, tarefas ou anotações? (Notion, Google Drive,
   nenhum lugar organizado)
2. Quero a agenda do dia no briefing? (padrão: sim, se eu usar Google Calendar)
3. Quero a caixa de entrada filtrada — só o que precisa de resposta minha, sem
   newsletter nem notificação automática? (padrão: sim)
4. Que assuntos externos eu quero acompanhar, em ordem? (peça 3 a 5; explique que
   menos assuntos com mais profundidade funciona melhor que cobertura ampla)
5. Que horas quero receber? (padrão: 7h da manhã)
6. Todo dia ou só dias de semana? (padrão: dias de semana)
7. Eu tenho um domínio próprio, onde consigo mexer no DNS? (isto decide a entrega
   — ver a regra de parada abaixo)
8. Tamanho e tom? (padrão: até 900 palavras, direto, sem introdução e sem elogio)

## REGRA DE PARADA — a única coisa que pode inviabilizar
Se eu responder que NÃO tenho domínio próprio: pare e me explique que, sem domínio
verificado, o e-mail só chega para mim mesmo e provavelmente na pasta de spam — e
nunca chega para outra pessoa. Então me ofereça três caminhos e me deixe escolher:
  (a) montar agora com entrega no Slack, que funciona de imediato;
  (b) montar agora por e-mail só para mim, sabendo que vai cair em spam e que eu
      vou ter que marcar "não é spam" e criar um filtro;
  (c) parar, registrar um domínio, e voltar quando estiver verificado.
Não me deixe montar em silêncio uma coisa que não vai entregar.

## FASE 2 — MONTAR
Com as respostas, escreva o prompt COMPLETO da rotina, num bloco de código, pronto
para eu copiar. Ele precisa conter, obrigatoriamente:
- Uma regra de ouro: só relatar o que está REGISTRADO nas fontes; NUNCA inventar;
  campo vazio vira "—".
- Instrução de que, se uma fonte falhar, a edição é entregue mesmo assim com a
  seção marcada "(fonte indisponível)" — nunca pulada em silêncio.
- Um rodapé de transparência em toda edição: quais fontes funcionaram, quais
  falharam, e a hora em que foi gerada.
- As fontes com os campos nomeados um a um, e "SOMENTE LEITURA; não crie nem
  edite nada" onde couber.
- O formato exato da entrega, com CSS inline se for e-mail.
- O limite de tamanho que eu escolhi.

## FASE 3 — INSTALAR
1. Me diga o cron em UTC correspondente ao horário que eu escolhi, e mostre a
   conta ("você pediu 7h no Brasil, que é 10h UTC, então: 0 10 * * 1-5").
2. Me liste quais conectores eu preciso autorizar em claude.ai/customize/connectors
   antes de ligar — só os que a minha configuração usa.
3. Se você TIVER uma ferramenta capaz de criar rotina agendada, me ofereça criar
   agora e me mostre o que vai criar antes. Se você NÃO tiver essa ferramenta, diga
   isso claramente e me guie clique a clique em claude.ai/code/routines. Não finja
   que criou.
4. Me mande rodar UMA vez na mão, antes de dormir. Me peça para colar o resultado
   aqui, e me ajude a ajustar o prompt até o briefing sair do jeito que eu leio.
5. No fim, me entregue um checklist curto do que confirmar nos primeiros dias,
   incluindo: conferir uma informação à mão na fonte original, e confirmar com os
   meus próprios olhos que o e-mail chegou na CAIXA DE ENTRADA — não no painel.

Comece pela pergunta 1. Só a pergunta 1.
```

---

## O que esperar

- Ele faz a **pergunta 1** e para. Responda e ele segue.
- Se você responder "padrão", ele usa a sugestão dele. Dá pra fazer a conversa
  inteira assim.
- No fim você recebe **um bloco de texto** — esse é o prompt da rotina. É ele que vai
  colado em `claude.ai/code/routines`.

## Se travar

| Aconteceu | Faça |
|---|---|
| Ele fez 5 perguntas de uma vez | Responda: *"uma pergunta por vez, como está no prompt"* |
| Ele disse que criou a rotina, mas não aparece nada em `claude.ai/code/routines` | Ele não tem essa ferramenta. Peça o texto do prompt e crie você mesmo no painel |
| Ele sugeriu apontar para uma pasta do seu computador | Contradiz o fato 1. Responda: *"a rotina roda na nuvem e não vê meu disco"* |
| Ele sugeriu enviar pelo Gmail | Contradiz o fato 3. Gmail só cria rascunho |
| O briefing chegou na hora errada | Cron em UTC. Some 3 à hora que você quer (Brasil) |
| Chegou sem formatação nenhuma | CSS não estava inline. Ver [EMAIL.md](EMAIL.md) |
| Não chegou nada, mas o painel diz `delivered` | Olhe a pasta de spam. Ver [EMAIL.md](EMAIL.md) |

Detalhe de cada um desses no [README.md](README.md) e no [ARMADILHAS.md](ARMADILHAS.md).
