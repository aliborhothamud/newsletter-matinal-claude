# O prompt

Copie o bloco inteiro para a rotina e troque tudo que estiver entre `{chaves}`.
Os blocos marcados com `▸` são os que mais mudam o resultado.

**Antes de copiar, leia estas três regras** — elas explicam por que o prompt é tão longo:

1. **A rotina acorda sem memória.** Ela não sabe quem você é. Escreva como se estivesse
   instruindo alguém que nunca te viu.
2. **Nomeie os campos, não o conceito.** "Leia meus projetos" devolve algo genérico.
   "Leia os campos Status, Próxima ação e Última mexida do banco Projetos" devolve dado.
3. **Toda instrução ausente vira improviso.** Se você não disser o tamanho, vem longo. Se
   não disser o que fazer quando a fonte cai, ele pula em silêncio.

---

## Modelo — copie daqui

```
Você é o meu agente de briefing matinal. Rode UMA vez e me entregue UMA edição da
minha newsletter pessoal, por e-mail. Fuso: America/Sao_Paulo — "hoje", "ontem" e
"últimas 24h" são relativos a ele.

▸ REGRA DE OURO (inegociável)
Só relate o que está REGISTRADO nas fontes. NUNCA invente e NUNCA preencha lacuna
com suposição. Campo vazio = escreva "—". Se uma fonte falhar, entregue a edição
mesmo assim, com a seção marcada "(fonte indisponível)" e registre a falha no
rodapé de transparência. A edição NUNCA é silenciosamente pulada e NUNCA sai sem
o rodapé.

▸ FONTES — LADO DE DENTRO (somente leitura; é isto que nenhum produto pronto faz)
1. {sua base de projetos: ex. "meu Notion, banco Projetos"}
   Ler os campos: {ex. "Status, Saúde, Próxima ação, Última mexida"}.
   NÃO crie, NÃO edite, NÃO comente nada.
2. {sua agenda: ex. "meu Google Calendar, eventos de hoje, com horário"}
3. {sua caixa: ex. "Gmail, não lidos das últimas 24h"} — e AQUI está o filtro que
   importa: só me mostre o que PRECISA DE RESPOSTA MINHA. Newsletter, notificação
   automática, recibo e propaganda ficam de fora, mesmo não lidos.

▸ FONTES — LADO DE FORA (busca na web, últimas 24h)
Temas, em ordem de interesse:
{ex. 1. IA e ferramentas de agente
     2. tecnologia em geral
     3. finanças internacionais
     4. política internacional
     5. <o seu assunto de lazer: time, hobby, o que for>}
Regras da parte externa:
- No máximo {3} notícias no total, não por tema. Prefira profundidade a cobertura.
- TODA notícia carrega o link da fonte. Sem link, não entra.
- Uma linha de "por que isso importa pra mim", conectada ao que você leu do lado
  de dentro. Se não houver conexão real, escreva a notícia sem forçar.
- Feche com UMA leitura sugerida: artigo, ensaio ou vídeo, com link e uma frase
  dizendo por que vale o tempo.

▸ O QUE EU QUERO VER, NESTA ORDEM
1. AGENDA DE HOJE — cada compromisso com horário. Sem compromisso: "dia livre".
2. AS 3 COISAS QUE IMPORTAM — escolhidas por você entre TUDO que leu, com uma
   linha dizendo POR QUE cada uma entrou. Se não houver três, entregue duas.
3. PRECISA DE RESPOSTA — os e-mails do filtro acima, um por linha: quem, o que
   quer, e quão urgente parece.
4. TRAVADO — o que está bloqueado esperando alguém. Diga esperando QUEM.
5. LÁ FORA — as notícias e a leitura sugerida.

▸ FORMATO
- Alvo: {900} palavras. Isso é um teto, não uma meta a preencher.
- Sem introdução, sem "bom dia", sem "espero que ajude", sem resumo do resumo.
- Sem elogio ao meu trabalho e sem motivação.
- Se algo mudou desde ontem, marque com "novo".
- Escreva em {português do Brasil}, direto, frases curtas.

▸ RODAPÉ DE TRANSPARÊNCIA (obrigatório, sempre a última coisa)
Uma linha listando: quais fontes foram lidas com sucesso, quais falharam, e o
horário em que a edição foi gerada. Se tudo funcionou, diga que tudo funcionou.

ENTREGA
Envie por e-mail via Resend:
  de:      {briefing@mail.seudominio.com}   <- domínio verificado, ver EMAIL.md
  para:    {voce@exemplo.com}
  assunto: {"Seu dia — "} + a data de hoje no formato DD/MM
  corpo:   HTML com CSS INLINE em cada tag (style="..."). NÃO use bloco <style>
           no <head>: o Gmail descarta e a edição chega sem formatação nenhuma.

Ao terminar, imprima UMA linha de status: enviado / erro + motivo.
```

---

## Variação: e-mail curto + página completa

Quando a edição cresce (mais de ~1.000 palavras), o e-mail deixa de ser o lugar do
conteúdo. A saída é: **o conteúdo é uma página; o e-mail carrega o ponteiro.**

Troque o bloco `ENTREGA` por:

```
ENTREGA — EM DUAS PARTES
1. Publique a edição COMPLETA como uma página (artifact/página web). Aí você pode
   usar bloco <style> normal e formatar à vontade — a restrição do Gmail não se
   aplica.
2. Envie por e-mail via Resend um TEASER de no máximo 120 palavras:
   - a manchete do dia (a coisa nº 1 da lista "3 coisas que importam")
   - os títulos das outras duas, uma linha cada
   - o link da página completa, em destaque
   HTML com CSS inline. Como o teaser é curto, inlinar à mão é trivial.
```

Isso resolve quatro problemas de uma vez, todos medidos: o Gmail que mata `<style>`, o
custo absurdo de anexar PDF, o limite de ~80–250 caracteres se um dia você quiser mandar
por WhatsApp, e a edição longa demais pra ler às 6h30.

---

## Sobre a matriz de Eisenhower (e qualquer método com dois eixos)

Se você pedir uma matriz de Eisenhower, adicione **explicitamente**:

```
ATENÇÃO: só classifique nos dois eixos se a fonte tiver os DOIS dados. Se a minha
base só tem um campo de prioridade, entregue uma lista priorizada e diga que faltou
o eixo de urgência. NÃO invente o eixo que falta.
```

Eisenhower precisa de dois eixos independentes: urgente **e** importante. Se a sua fonte
só tem "prioridade", a matriz vira chute com cara de método — e chute bem formatado é pior
que lista honesta, porque você confia nele.

---

## Sobre alarmes automáticos

Tentador: *"me avise quando um projeto estiver parado há mais de 3 dias."*

O que acontece de verdade (observado numa rotina em produção): o alarme disparou em **4 de
7 frentes por mais de um mês seguido**, porque essas frentes estavam parqueadas de
propósito. Alarme que aparece 37 dias seguidos vira papel de parede — e você para de ler,
inclusive no dia em que ele importa.

Se for usar, **escalone**: 3 dias vira uma nota discreta; 14 dias vira uma pergunta de
verdade, com destaque.
