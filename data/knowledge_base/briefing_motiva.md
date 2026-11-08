# Resumo do briefing Motiva (contexto do Challenge)

Tipo de documento: `briefing` · Origem: resumo do documento oficial do Challenge FIAP × Motiva, baseado no briefing "Inovação Aberta FIAP — EngMan Motiva" (16/09/2026).

## A Motiva
A maior empresa de infraestrutura de mobilidade do Brasil, com concessões de rodovias, trens, metrôs e aeroportos. Presente em 13 estados, com 37 ativos e mais de 16 mil colaboradores. Mais de 5 mil km de rodovias concedidas, 13 concessionárias, 263 cidades e cerca de 2 milhões de veículos pedagiados por dia. A Engenharia de Manutenção atua nos programas PRIME e MIND.

## Por que Engenharia de Manutenção
Arrecadação, monitoramento e segurança viária dependem de ativos eletroeletrônicos em campo, expostos a poeira, fuligem, chuva, insetos, vibração e variação térmica. A detecção de falha depende muito da percepção humana no CCO (é reativa); o plano de manutenção é majoritariamente estático (por calendário); o histórico de falhas do FSM existe, mas é pouco explorado para prever.

## Os temas do briefing
| Tema | O que a Motiva quer | Dor e impacto |
|---|---|---|
| Tema 1 — Monitoramento físico inteligente | Substituir sensores de solo por câmeras ou sensores IoT, com edge computing conversando com o PLC da pista | Falha de hardware e tarifação incorreta; impacto em receita e fluidez da praça |
| Tema 2 — Confiabilidade preditiva | Manutenção preventiva, inspeção e CBM por taxa de falha real, com modelo exponencial R(t) = e^(−λt), RGA e LDA sobre o histórico do FSM | Plano de manutenção estático; deslocamento de campo desnecessário |
| Tema 2.1 — Saúde visual dos ativos ITS | Detecção autônoma de câmeras de CFTV degradadas (lente suja, teia, ninho, desfoque), com alerta persistente e integração via API | Manutenção reativa e falso positivo (glare, chuva, neblina); impacto na disponibilidade |

## Recorte deste projeto
O EngMan Copilot (Recorte A) atua nos **Temas 2 e 2.1**, para os ativos CFTV (câmeras PTZ), PMV e Callbox.
