> **DADO SIMULADO — uso acadêmico.** Critérios fictícios criados pelo grupo; não são os critérios oficiais da Motiva.

# Matriz de criticidade e prioridade

Tipo de documento: `matriz_criticidade` · Tipo de ativo: `geral`

Usada pelo assistente (regra R11) para recomendar prioridade com **critérios explícitos e auditáveis**, nunca por região, praça ou "sensação".

## 1. Impacto por tipo de ativo (base da criticidade do catálogo)

| Ativo | Segurança viária | Arrecadação | Disponibilidade da infraestrutura | Observação |
|---|---|---|---|---|
| Callbox | Alto (canal de socorro do usuário) | Nenhum | Médio | Falha pode atrasar um socorro |
| CFTV (câmera PTZ) | Alto em trechos de despacho de socorro; médio nos demais | Nenhum | Alto | Base do monitoramento do CCO |
| PMV | Alto quando exibe neblina, interdição ou acidente | Nenhum | Médio | Impacta a informação ao motorista |

A **criticidade do ativo** (alta, média ou baixa) vem do `catalogo_ativos` por TAG.

## 2. Estado da falha

| Estado | Definição | Exemplos |
|---|---|---|
| A · Perda total de função crítica | O ativo não cumpre sua função | Callbox com botão sem resposta; PMV apagado; câmera sem vídeo |
| B · Degradação relevante | A função falha parcialmente ou em parte do período | Câmera perde imagem à noite; PMV com mensagem ilegível; callbox com áudio unidirecional |
| C · Degradação parcial | A função continua utilizável | PMV com poucos módulos apagados e mensagem legível |
| D · Degradação leve | Sem impacto operacional imediato | Imagem levemente embaçada sem alarme |

## 3. Prioridade (estado da falha × criticidade do ativo)

| Estado | Criticidade alta | Criticidade média | Criticidade baixa |
|---|---|---|---|
| A | **P1_critica** | P2_alta | P3_media |
| B | **P2_alta** | P3_media | P3_media |
| C | **P3_media** | P3_media | P4_baixa |
| D | P4_baixa | P4_baixa | P4_baixa |

Significado: **P1_critica** = risco à segurança viária ou perda total de função crítica; atendimento imediato. **P2_alta** = atendimento no mesmo dia. **P3_media** = programar para os próximos dias. **P4_baixa** = incluir na próxima visita ou preventiva.

## 4. Regras de aplicação
1. Mostrar ao usuário o estado da falha e a criticidade usados na decisão.
2. A reincidência (3 ou mais falhas do mesmo tipo em 12 meses) **não altera a prioridade**; ela recomenda avaliar CBM, análise de causa raiz ou retrofit.
3. Em caso de dúvida sobre o estado da falha, o assistente pede a informação que falta em vez de assumir.
4. A prioridade é recomendação; a decisão final é humana (R12).
