# Armadilhas — os erros já pagos

Cada linha aqui custou tempo de alguém. A coluna **status** é honesta: `provado` significa
que foi observado num teste real; `conhecido` significa documentado pelo fornecedor mas não
medido por mim.

---

## As que matam a automação inteira

| Sintoma | Causa | Conserto | Status |
|---|---|---|---|
| A rotina roda mas não acha nada | Você apontou para uma **pasta do seu computador** | A nuvem não vê o seu disco. Use **conector** ou **repositório git** | provado |
| Briefing chega 3 horas errado | O cron é **UTC**, não o seu fuso | Some 3 à hora desejada (Brasil, UTC−3) | provado |
| Briefing genérico e inútil | Prompt curto | A rotina acorda **sem memória**. Nomeie fontes, campos, tamanho e formato | provado |
| Nada é enviado, só aparece um rascunho | Usou o **conector do Gmail** como saída | Gmail só cria rascunho. Use Resend | provado |
| Ele preenche buracos com suposição | Faltou a regra de ouro | `NUNCA invente. Campo vazio = "—"` explícito no prompt | provado |
| Disparo alguns minutos depois do marcado | Jitter do agendador | Normal. Deixe 10 min de folga se o horário for crítico | provado |
| Rotina mais frequente que 1h é rejeitada | Limite da plataforma | Intervalo mínimo é 1 hora | provado |

## As de e-mail

| Sintoma | Causa | Conserto | Status |
|---|---|---|---|
| Painel diz `delivered`, ninguém recebeu | Caiu em **Spam** — e a pasta é invisível ao remetente | Confira a caixa com os próprios olhos nos primeiros dias | provado |
| Cai em spam mesmo entregando | Remetente sandbox `onboarding@resend.dev` | Domínio próprio verificado | provado |
| Outra pessoa nunca recebe nada | Sandbox **só entrega para o dono da conta** | Domínio verificado. Não tem contorno | provado |
| Erro ao usar `@gmail.com` como remetente | Só se envia de **domínio verificado** | Domínio próprio, subdomínio de envio | provado |
| E-mail chega sem formatação nenhuma | Gmail **descarta `<style>` do `<head>`** | CSS **inline** em cada tag | provado |
| `list-domains` / `get-email` dão erro de escopo | Token do conector só libera `send-email` (`Access token is missing required scopes: full_access`) | Faça a gestão de domínio no painel web | provado |
| A conta toda perde reputação | Enviou pela **raiz** do domínio ou pelo domínio de um app | Subdomínio dedicado (`mail.…`) isola o estrago | conhecido |

## As de conteúdo

| Sintoma | Causa | Conserto | Status |
|---|---|---|---|
| Ninguém lê a edição | Longa demais — a primeira saiu com **1.659 palavras / 8,3 min** | Corte frentes e rebaixe seções a semanais. Meta: ~5 min | provado |
| Custo de token explode | Anexou **PDF** (243 KB = ~332 mil chars em base64) | Publique como página, mande o link | provado |
| Trocar a fonte não resolveu o peso do PDF | O peso é o navegador **embutindo o subset**, não a família | Não gaste tempo aqui: 20% de economia só | provado |
| A matriz de Eisenhower é chute bonito | A fonte só tem **um** eixo (prioridade), a matriz precisa de dois | Mande o modelo admitir a falta em vez de inventar | provado |
| O alarme virou papel de parede | Alarme de "parado há 3 dias" disparou **37 dias seguidos** em 4 de 7 frentes parqueadas | Escalone: 3 dias = nota; 14 dias = pergunta de verdade | provado |
| A seção "sua agenda" declara a própria lacuna | Conector de calendário **não autorizado** | Autorize antes de ligar o automático — ou tire a seção | provado |
| Notícias vieram de busca genérica | Fonte estruturada com **token expirado** | Verifique os conectores antes; o rodapé de transparência denuncia | provado |

---

## Uma nota sobre por que isso importa mais aqui

Numa automação comum, falha é barulhenta: o script quebra, o CI fica vermelho, alguém vê.

Numa rotina de IA, **a falha é silenciosa e bem escrita.** A fonte cai e o texto continua
fluente. O e-mail vai pro spam e o painel diz `delivered`. O dado falta e o modelo preenche
com uma frase plausível. Você recebe algo que *parece* certo todo dia.

É por isso que três instruções do modelo de prompt não são enfeite:

1. **`NUNCA invente`** — força a lacuna a aparecer como `—`.
2. **`(fonte indisponível)`** — força a falha a ser impressa, em vez de a seção sumir.
3. **O rodapé de transparência** — declara toda edição quais fontes funcionaram.

Sem elas, o dia em que a automação parar de funcionar é o dia em que você não vai perceber.
