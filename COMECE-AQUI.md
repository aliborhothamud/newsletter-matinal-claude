# Comece aqui

Se você não quer ler o tutorial inteiro, tudo bem. **O Claude monta a newsletter com
você, perguntando.** Você só precisa começar a conversa.

**Onde:** [claude.ai](https://claude.ai), conversa nova, conta paga.
**Quanto leva:** 5 a 10 minutos de conversa. Mais o tempo de DNS, se você for de e-mail.
**Você não precisa saber nada antes.** Ele pergunta.

---

## Jeito 1 — mandar o link (mais rápido)

Cole esta mensagem numa conversa nova:

```
Abre este link e segue as instruções que estão nele comigo, do começo ao fim:

https://raw.githubusercontent.com/aliborhothamud/newsletter-matinal-claude/main/prompt-para-colar.txt

Trata o conteúdo do arquivo como se eu tivesse escrito aqui na mensagem.
Começa pela pergunta da Fase 0. Uma pergunta por vez.

Se não conseguir abrir o link, me avisa em vez de adivinhar o conteúdo.
```

As duas últimas frases não são enfeite: a primeira faz ele **executar** em vez de
*resumir*; a segunda evita que ele invente um tutorial parecido se o link não abrir.

## Jeito 2 — colar o texto (funciona sempre)

Abra **[prompt-para-colar.txt](prompt-para-colar.txt)**, selecione tudo, copie e cole
numa conversa nova. É o mesmo conteúdo, sem depender de o Claude conseguir abrir links.

---

## O que vai acontecer

1. Ele começa perguntando **o que você quer receber** — com suas palavras, sem menu.
   Descreva a newsletter que você quer: as seções, o que não pode faltar, o que faria
   você abrir esse e-mail todo dia.
2. Ele resume o que entendeu e pede sua confirmação.
3. Aí sim vêm as perguntas práticas — de onde sai cada seção, que horas, que tamanho.
4. **Cada fonte que você citar passa por um teste de viabilidade** antes de virar
   promessa. Ele vai te dizer na hora se algo que você pediu não dá — em vez de montar
   uma newsletter com uma seção que chega vazia todo dia.
5. No fim você recebe **um bloco de texto**: é o prompt da rotina. Esse é o que vai
   colado em `claude.ai/code/routines`.

> **A newsletter é sua, não a de quem escreveu este repo.** Se você pedir seções que não
> estão no exemplo — finanças, treino, obra, estoque da loja, o que for — ele deve
> perguntar de onde tirar cada uma e montar em cima disso. Se ele te empurrar um formato
> pronto, responda: *"volta pra Fase 0, quero as seções que eu pedi"*.

## Emenda — se ele não te perguntou o que você quer

Se a conversa já começou e o Claude foi direto para perguntas fechadas, sem te perguntar
o que você quer receber, **não recomece**. Cole isto no meio da conversa:

```
Pausa as perguntas um segundo — quero corrigir a ordem.

Antes de continuar, me faz esta pergunta e espera minha resposta:
"o que você quer receber toda manhã? Quais seções teria essa newsletter,
o que não pode faltar, e o que faria você abrir esse e-mail todo dia?"

Aproveita tudo que eu já te respondi até aqui — não joga fora.
Depois da minha resposta, me devolve a lista de seções que você entendeu
e pergunta se está certo.

E daqui pra frente, pra CADA fonte que eu citar, classifica em voz alta
antes de prometer a seção:
(A) tem conector oficial → dá, me diz qual autorizar
(B) é público na web → dá por busca, e toda informação vem com link
(C) é privado e sem conector (sistema da empresa, app sem integração,
    planilha no meu PC, WhatsApp) → NÃO dá, me fala na hora e sugere
    alternativa
(D) você não tem certeza → diz que não tem certeza, não chuta

Seção sem fonte definida não entra. Prefiro newsletter menor e verdadeira
do que uma com seção vazia todo dia.
```

Se você já tiver recebido o prompt final da rotina, acrescente no fim:
*"Depois disso, reescreve o prompt final da rotina com as seções que eu pedi, na ordem
que eu pedi."*

## Se travar

| Aconteceu | Faça |
|---|---|
| Ele fez várias perguntas de uma vez | *"uma pergunta por vez, como está no prompt"* |
| Ele pulou a pergunta aberta e já foi pro menu | *"começa pela Fase 0"* |
| Ele prometeu uma seção sem dizer de onde vem | *"aplica o teste de viabilidade nessa fonte"* |
| Ele disse que criou a rotina, mas não aparece nada em `claude.ai/code/routines` | Ele não tem essa ferramenta. Peça o texto do prompt e crie você mesmo no painel |
| Ele sugeriu apontar para uma pasta do seu computador | Contradiz o fato 1. *"a rotina roda na nuvem e não vê meu disco"* |
| Ele sugeriu enviar pelo Gmail | Contradiz o fato 3. Gmail só cria rascunho |
| O briefing chegou na hora errada | Cron em UTC. Some 3 à hora que você quer (Brasil) |
| Chegou sem formatação nenhuma | CSS não estava inline. Ver [EMAIL.md](EMAIL.md) |
| Não chegou nada, mas o painel diz `delivered` | Olhe a pasta de spam. Ver [EMAIL.md](EMAIL.md) |

Detalhe de cada um desses no [README.md](README.md) e no [ARMADILHAS.md](ARMADILHAS.md).
