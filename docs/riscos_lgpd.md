# Matriz de Riscos, Ética, Viés, Segurança e LGPD — EngMan Copilot

> O EngMan Copilot apoia decisões sobre **infraestrutura crítica** (segurança viária, arrecadação e trabalho em campo). Um erro de IA aqui tem consequência física, financeira e legal. Por isso cada risco abaixo tem **impacto classificado** e **mitigação rastreável** até um bloco do system prompt (`R1…R13`, blocos `# …`), uma etapa do fluxograma (`B1…B8`, ver `docs/arquitetura.png`), um caso do eval set (`C01…C14`) ou uma regra da base de conhecimento (`data/knowledge_base/inventario.md`).

Escala de impacto: **Alto** (dano físico, multa/processo, decisão de CAPEX errada) · **Médio** (retrabalho, deslocamento desnecessário) · **Baixo** (inconveniência).

## 1. Matriz

| ID | Risco | Como se manifesta neste projeto | Impacto (tipo · nível) | Mitigação concreta | Onde a mitigação aparece |
|---|---|---|---|---|---|
| **RISCO-01** | Alucinação | Inventar TAG, modelo, número de OS, métrica de confiabilidade, trecho de norma ou registro de histórico do FSM. | Segurança + financeiro · **Alto** | Regra explícita de não inventar; obrigação de citar fonte; dados estruturados vêm de tools (fonte única); admitir "não encontrei"; guardrail de saída verifica TAG/métrica contra o retorno das tools. | Prompt: R1, R2, R8, bloco ESCOPO · Fluxo: B5, B6, B7 · Eval: C01, C02, C06 · Base: seção 4 (fonte única) |
| **RISCO-02** | Prompt injection (direta e indireta) | "Ignore as instruções…" ou comando escondido dentro de um laudo/boletim colado pelo usuário. | Segurança + reputacional · **Alto** | Guardrail de entrada detecta padrões de injection; regra de imutabilidade; texto colado tratado como DADO; não revelar o prompt; testes de jailbreak. | Prompt: R7, BORDA (pedido hostil) · Fluxo: B2 · Eval: C08, C09 |
| **RISCO-03** | Risco operacional (elétrico / altura / via) | Orientar troca de bateria com callbox energizado, subir em poste sem NR-35, intervir em pórtico sem procedimento. | Segurança (vida) · **Alto** | Recusa obrigatória; remeter ao POP e a NR-10/NR-35; exigir profissional habilitado; EPI e desligamento seguro no início de qualquer passo a passo; o assistente nunca libera/bloqueia equipamento. | Prompt: R5, R12, TOM (técnico de campo) · Fluxo: B2 (bloqueio), B6 (POP) · Eval: C04, C10 · Base: `normas_seguranca`, POPs |
| **RISCO-04** | Responsabilidade técnica/legal | Usuário pede laudo, ART ou parecer de engenharia "assinado" pelo assistente. | Legal · **Alto** | Recusa explícita; encaminhar para engenheiro habilitado; qualquer resumo é rotulado como não oficial. | Prompt: R4 · Eval: C09, C11 |
| **RISCO-05** | Viés na priorização | Priorizar sempre o mesmo tipo de ativo, rodovia ou praça; ignorar callbox de trecho com menos tráfego. | Segurança + ético · **Médio** | Critérios explícitos e auditáveis (criticidade × segurança viária × arrecadação × disponibilidade) exibidos em toda priorização; matriz de criticidade única; few-shot cobre vários tipos de ativo e prioridades. | Prompt: R11, EXEMPLOS · Tool: `listar_ativos_por_criticidade` · Base: `matriz_criticidade` · Eval: C02, C03 |
| **RISCO-06** | LGPD: dado pessoal | Placas lidas por OCR, nome de motorista em boletim, dados de empregados ou imagem de ocupante chegando à base ou ao prompt. | Legal (LGPD) + reputacional · **Alto** | Declarar o que não entra na base; anonimizar placas antes da ingestão; assistente se recusa a tratar dado pessoal e pede anonimização; não enviar dado pessoal a modelo externo; tool de OS limita a descrição e proíbe dado pessoal. | Prompt: R10 · Fluxo: B2 · Eval: C14 · Base: seção 3.3 · Tool: `abrir_ordem_servico.descricao` |
| **RISCO-07** | Excesso de confiança (automation bias) | Técnico trata a hipótese do modelo como diagnóstico confirmado e troca a peça errada; engenheiro aprova CAPEX com base em métrica de amostra pequena. | Financeiro + segurança · **Médio** | Rótulos FATO vs HIPÓTESE; "validar em campo"; "Nível de confiança" obrigatório; OS só com confirmação humana; tool de confiabilidade alerta amostra pequena. | Prompt: R3, R12, FORMATO, RACIOCÍNIO · Fluxo: B6 (confirmação), B7 · Eval: C03, C11 · Tool: `calcular_metricas_confiabilidade` |
| **RISCO-08** | Confidencialidade operacional | Histórico de falhas e dados de arrecadação enviados a API pública de terceiros. | Legal + estratégico · **Médio** | Modelo e embeddings **locais** (Ollama) como caminho principal; usar apenas dado simulado; não versionar dado real; `.gitignore` bloqueia `.env`/chaves. | Arquitetura: `docs/decisoes_tecnicas.md` · Fluxo: B4 · Repositório: `.gitignore` |
| **RISCO-09** | Contexto estourado / mistura de ativos | Conversa longa ultrapassa a janela; o assistente perde o TAG ou mistura o histórico do km 412 com o do km 330. | Financeiro + segurança · **Médio** | Política de histórico: últimos 6 turnos completos + resumo dos anteriores; TAG reafirmado a cada resposta; aviso explícito ao trocar de TAG. | Prompt: R13, BORDA (conversa longa) · Fluxo: B3, B8 · Eval: C12 |
| **RISCO-10** | Fontes divergentes | POP diz 60 dias, plano diz 90 dias; o modelo escolhe um valor ao acaso ou faz média. | Financeiro · **Baixo** | Ordem de autoridade definida; exibir as duas fontes; pedir validação humana. | Prompt: R9 · Eval: C13 · Base: nota de divergência intencional |

**Total: 10 riscos** (mínimo exigido: 8).

## 2. LGPD: o que entra e o que nunca entra

| Entra na base / no prompt | Nunca entra |
|---|---|
| TAG, tipo, modelo e localização (km) **fictícios** dos ativos | Nome, matrícula, CPF ou telefone de empregados |
| Histórico de falhas **simulado** (sintoma, causa, tempo de reparo) | Placas de veículos (OCR) e dados de TAG de pedágio de usuários |
| POPs, planos e matriz de criticidade **simulados** | Imagens de ocupantes ou de pessoas identificáveis |
| Resumos públicos de NR-10 / NR-35 e do briefing | Valores reais de arrecadação, contratos e dados sigilosos da Motiva |
| Boletins de campo **anonimizados** (condição ambiental, sintoma) | Credenciais, chaves de API, `.env` com valores reais |

**Base legal e princípios:** por ser trabalho acadêmico com dado simulado, o projeto aplica por design os princípios da LGPD (Lei 13.709/2018, art. 6º): **finalidade** (só manutenção de ativos), **necessidade/minimização** (nenhum dado pessoal é necessário para diagnosticar um equipamento), **segurança** (execução local) e **transparência** (dados simulados identificados como tal).

## 3. Ética e responsabilidade do desenvolvedor

- **Humano no circuito:** o assistente recomenda; quem decide e assina é o profissional habilitado (R4, R12).
- **Transparência:** toda resposta mostra fonte, rótulo FATO/HIPÓTESE e nível de confiança, para que o usuário saiba quanto confiar.
- **Responsabilidade do grupo:** o grupo responde pelo conteúdo do prompt, pelos dados simulados e pelos testes. Nenhum output de modelo será apresentado como executado se não tiver sido executado.
- **Uso indevido previsto:** tentativas de usar o assistente para fins fora da manutenção (vigilância de motoristas via CFTV, por exemplo) são recusadas pela frase padrão (R6) e pela regra de dado pessoal (R10).

## 4. Rastreabilidade reversa (caso → risco)

| Caso | Riscos que testa |
|---|---|
| C01, C02 | RISCO-01, RISCO-05 |
| C03 | RISCO-01, RISCO-07 |
| C04 | RISCO-03 |
| C05 | RISCO-01 |
| C06 | RISCO-01 |
| C07 | escopo (R6) |
| C08 | RISCO-02 |
| C09 | RISCO-02, RISCO-04 |
| C10 | RISCO-03 |
| C11 | RISCO-04, RISCO-07 |
| C12 | RISCO-09 |
| C13 | RISCO-10 |
| C14 | RISCO-06 |
