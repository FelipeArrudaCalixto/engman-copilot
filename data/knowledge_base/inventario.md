# Inventário da Base de Conhecimento — EngMan Copilot (Recorte A)

> **Aviso de origem dos dados.** A Motiva não libera base real para trabalho acadêmico. Todos os documentos marcados como **simulado** serão criados pelo grupo com **dados fictícios plausíveis** e identificados como fictícios (campo `simulado: true` nos metadados e cabeçalho de cada arquivo). Documentos **real** são extraídos do briefing "Inovação Aberta FIAP — EngMan Motiva" ou de normas públicas. Nenhum dado real da Motiva é usado ou inventado como real.

Ativos do recorte: **CFTV (câmeras PTZ)**, **PMV**, **Callbox**.
Formato de TAG (fictício): `TIPO-RODOVIA-KMxxx-NN` — ex.: `CFTV-SP330-KM412-01`, `PMV-SP330-KM205-01`, `CBX-SP330-KM118-01`.

## 1. Lista de documentos

| # | ID | Documento | Conteúdo e campos | Origem | Quantidade mínima | Usado por |
|---|---|---|---|---|---|---|
| 1 | `catalogo_ativos` | Catálogo de ativos | TAG, tipo, fabricante, modelo, km, praça/pórtico, data de instalação, vida útil (anos), criticidade (alta/média/baixa) | simulado | 30+ linhas (ex.: 12 CFTV, 10 PMV, 8 Callbox) | tool `consultar_ativo`, RAG |
| 2 | `PMP-CFTV-001` e `PMP-PMV-001`, `PMP-CBX-001` | Plano de manutenção preventiva | ativo, tarefa, periodicidade, responsável, tempo estimado, recursos, condição de execução. Ex.: limpeza de lente PTZ a cada **90 dias** (valor fictício) | simulado | 1 por tipo de ativo (3) | RAG (caso C01, C13) |
| 3 | `historico_falhas_fsm` | Histórico de falhas do FSM | TAG, data, sintoma, causa identificada, tipo_falha (enum da tool), tempo de reparo (h), peça trocada, reincidência (s/n) | simulado | 50+ registros | tool `consultar_historico_falhas`, tool de confiabilidade |
| 4 | `POP-CFTV-001`, `POP-PMV-001`, `POP-CBX-001` | Procedimentos operacionais (POPs) | passo a passo de inspeção, limpeza de lente, ajuste de foco, teste de comunicação, troca de módulo LED, teste de botão/áudio — com pré-requisitos de segurança (EPI, NR-10, NR-35) | simulado | 3 | tool `consultar_procedimento`, RAG |
| 5 | `fichas_tecnicas` | Fichas técnicas dos equipamentos | especificações do CFTV PTZ, do PMV e do Callbox: alimentação, interfaces, faixa de operação, modos de falha típicos (a partir do briefing) | real (briefing, nível conceitual) + simulado (modelos/valores) | 1 por tipo de ativo do recorte (3) | RAG |
| 6 | `matriz_criticidade` | Matriz de criticidade | ativo → impacto em arrecadação, segurança viária e disponibilidade → prioridade (P1–P4) | simulado | 1 | RAG, tool `listar_ativos_por_criticidade`, regra R11 |
| 7 | `glossario` | Glossário de siglas | CFTV, PMV, SAT, SCA, CCO, CBM, RCM, WCM, FSM, RGA, LDA, λ, MTBF, MTTR, WIM, OCR, EPI, POP, OS | real | 1 | RAG |
| 8 | `normas_seguranca` | Normas e requisitos de segurança | resumo de trechos aplicáveis de **NR-10** (segurança em instalações elétricas) e **NR-35** (trabalho em altura) e regras internas de trabalho em via (genéricas) | real (resumo, com citação da fonte) | 1 | RAG, regra R5 |
| 9 | `briefing_motiva` | Documentos do briefing Motiva | contexto da empresa, ondas de inovação, descrição dos Temas 1, 2 e 2.1 (câmeras de CFTV: lente suja, teia, ninho, desfoque), PRIME e MIND | real | 1 | RAG |
| 10 | `boletins_ocorrencia` | Boletins de ocorrência / laudos de campo | texto livre do que o técnico relatou ao atender, incluindo condição ambiental (chuva, neblina, glare). **Sem placas, sem nomes** | simulado | 10+ | RAG, casos C03, C09, C14 |

Total: **10 documentos** (mínimo exigido: 8).

### Documentos já produzidos nesta sprint (dados SIMULADOS)

Os 10 documentos do inventário foram produzidos e estão nesta pasta. Os marcados como real são resumos conceituais; nenhum dado real da Motiva é usado.

| # | Arquivo | Conteúdo | Tamanho |
|---|---|---|---|
| 1 | [`catalogo_ativos.csv`](catalogo_ativos.csv) | 12 CFTV PTZ, 10 PMV, 8 Callbox, com todos os campos do catálogo | 30 ativos |
| 2 | [`PMP-CFTV-001.md`](PMP-CFTV-001.md) · [`PMP-PMV-001.md`](PMP-PMV-001.md) · [`PMP-CBX-001.md`](PMP-CBX-001.md) | Planos de manutenção preventiva: tarefa, periodicidade, responsável, tempo, recursos e condição | 3 planos |
| 3 | [`historico_falhas_fsm.csv`](historico_falhas_fsm.csv) | Data, sintoma, causa, `tipo_falha` (mesmo enum da tool), tempo de reparo, peça, reincidência | 56 registros |
| 4 | [`POP-CFTV-001.md`](POP-CFTV-001.md) · [`POP-PMV-001.md`](POP-PMV-001.md) · [`POP-CBX-001.md`](POP-CBX-001.md) | POPs com pré-requisitos de segurança e as 4 seções de intervenção (inspeção, preventiva, corretiva, CBM) | 3 POPs |
| 5 | [`fichas_tecnicas.md`](fichas_tecnicas.md) | Fichas dos 3 tipos de ativo do recorte | 3 fichas |
| 6 | [`matriz_criticidade.md`](matriz_criticidade.md) | Estado da falha × criticidade do ativo → prioridade P1–P4 | 1 matriz |
| 7 | [`glossario.md`](glossario.md) | Siglas do briefing e de confiabilidade | 30 siglas |
| 8 | [`normas_seguranca.md`](normas_seguranca.md) | Resumo conceitual de NR-10 e NR-35 e regras de trabalho em via | 1 resumo |
| 9 | [`briefing_motiva.md`](briefing_motiva.md) | Contexto da Motiva e Temas 1, 2 e 2.1 | 1 resumo |
| 10 | [`boletins_ocorrencia.md`](boletins_ocorrencia.md) | Relatos de técnico sem dado pessoal, coerentes com o histórico | 12 boletins |

**Ativos-vitrine com histórico completo e realista** (prontos para o dataset simulado e usados nos casos de teste):

| TAG | Padrão do histórico | Usado em |
|---|---|---|
| `CFTV-SP330-KM412-01` | 4 perdas de imagem noturna em 12 meses (3× teia refletindo IR, 1× iluminador IR queimado) + 1 lente suja; instalado em 2016, com vida útil de 7 anos já ultrapassada | C03, C04, C09, C12, mockup |
| `CFTV-SP330-KM330-01` | 2 desfoques por vibração do poste + 1 falha de comunicação: padrão **diferente** do km 412 | C12 (não misturar TAGs) |
| `PMV-SP330-KM205-01` | 3 falhas de módulo LED (fonte degradada, reincidente) + 1 mensagem travada | few-shot, C11 |
| `CBX-SP330-KM118-01` | 2 botões sem resposta + 2 falhas de bateria de backup (reincidente) | few-shot, C02, C10, C14 |

Todos os fabricantes e modelos levam o sufixo "(ficticio)". O TAG usa o código de rodovia apenas como formato. Nenhum registro corresponde a dado real da Motiva. Cada documento terá o cabeçalho `> DADO SIMULADO — uso acadêmico` quando for simulado.

> **Divergência intencional para teste (regra R9 / caso C13):** o `PMP-CFTV-001` fixa a limpeza de lente em 90 dias, enquanto o `POP-CFTV-001` cita 60 dias. A inconsistência é proposital e documentada, para testar se o assistente apresenta as duas fontes e aplica a ordem de autoridade.

## 2. Como a base será usada no RAG

1. **Ingestão (offline):** cada documento é lido, limpo e dividido em *chunks*.
2. **Embeddings:** `nomic-embed-text` (Ollama, local) — roda localmente, custo zero, sem enviar dados a terceiros.
3. **Vector store:** ChromaDB (decisão em `docs/decisoes_tecnicas.md`), com metadados para busca filtrada.
4. **Consulta (online):** o guardrail de entrada identifica TAG e tipo de ativo; a busca recupera os **4 trechos** mais relevantes (top-k = 4), filtrando por `tipo_ativo` quando conhecido; os trechos entram no bloco "Montagem do contexto" (B3 do fluxograma).
5. **Dados estruturados** (ficha do ativo, histórico por TAG, métricas) **não passam pelo RAG**: vêm das tools, para evitar duas fontes de verdade (ver seção 4).

## 3. Decisões obrigatórias

### 3.1 Tamanho do chunk
- **Faixa:** 400 a 600 tokens por chunk, com sobreposição de 50 a 80 tokens (≈ 10–15%).
- **Por quê (janela de contexto, Aula 02):** com `num_ctx = 8192` (modelo local 7B), reservamos ≈ 2.000 tokens para os 4 trechos recuperados (4 × ≈ 500). Chunks maiores (> 800) consomem a janela rápido e trazem texto irrelevante; menores (< 200) quebram procedimentos (passo 3 separado do passo 4) e perdem pré-requisitos de segurança.
- **Exceções:** um POP é dividido por **seção lógica** (pré-requisitos de segurança sempre no mesmo chunk do primeiro passo); um registro de histórico é um chunk próprio.

### 3.2 Metadados guardados junto a cada trecho (filtro de busca)
`tipo_documento` · `tipo_ativo` (cftv_ptz | pmv | callbox | geral) · `tag` (quando houver) · `data` · `criticidade` · `fonte` (ID do documento) · `versao` · `simulado` (true/false).

### 3.3 O que NÃO entra na base (LGPD)
- Dados pessoais de empregados (nome, matrícula, CPF, telefone).
- Imagens de ocupantes de veículos e imagens das câmeras com pessoas identificáveis.
- Placas de veículos capturadas por OCR (se um boletim mencionar placa, ela é **anonimizada** antes de entrar).
- Dados contratuais sensíveis, valores de arrecadação reais e qualquer dado real da Motiva.
- Chaves de API, credenciais e dados de acesso a sistemas.

### 3.4 Política de atualização
O inventário é a fonte de verdade de *o que existe*. Alterações no número de documentos ou nos campos devem ser feitas neste arquivo antes de alterar tools ou prompt (para manter a coerência entre artefatos).

## 4. Fonte única de verdade por dado

| Dado | Fonte única | Observação |
|---|---|---|
| Ficha do ativo | tool `consultar_ativo` (catálogo) | Não duplicar no RAG; RAG traz só a ficha *técnica do tipo* |
| Histórico de falhas | tool `consultar_historico_falhas` (FSM simulado) | Boletins no RAG são narrativas, não substituem o histórico estruturado |
| Procedimento | tool `consultar_procedimento` (POP) / RAG para contexto | Se discordarem, vale a ordem de autoridade da regra R9 |
| Criticidade/prioridade | matriz de criticidade | Mesma tabela usada por RAG e tool de ranking |

## 5. Estimativa de tamanho (bônus — política de contexto quantificada)

Estimativa grosseira (≈ 1,4 token/palavra em pt-BR; validar com o tokenizer do modelo nas sprints seguintes):

| Componente | Tamanho estimado | Cabe inteiro na janela de 8.192 tokens? |
|---|---|---|
| Catálogo (30 linhas × ≈ 40 tokens) | ≈ 1.200 | parcialmente |
| Histórico (50 registros × ≈ 60 tokens) | ≈ 3.000 | parcialmente |
| POPs (3 × ≈ 800) | ≈ 2.400 | — |
| Plano preventivo, fichas, matriz, glossário, normas, briefing, boletins | ≈ 8.000–10.000 | — |
| **Base total** | **≈ 15.000–17.000** | **Não** → justifica RAG + tools |
