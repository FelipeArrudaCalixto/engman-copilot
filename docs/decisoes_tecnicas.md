# Decisões Técnicas — Stack, Parâmetros e Política de Contexto

> Regra da disciplina: **caminho de execução 100% gratuito**. Nenhuma etapa depende de API paga. A tabela de verificação da seção 6 registra o que foi conferido e em que fonte.

## 1. Modelo (LLM): comparação

| Critério | **Qwen 2.5 7B Instruct** (Ollama local) | Llama 3.1 8B Instruct (Ollama local) | Gemini (Google AI Studio, free tier) |
|---|---|---|---|
| Custo de uso | Zero (roda local) | Zero (roda local) | Zero dentro da cota gratuita; sujeito a limites de requisição |
| Licença / termos | Apache 2.0 (variante 7B) | Llama 3.1 Community License | Termos do Google AI Studio; **conferir os termos vigentes** sobre uso dos dados enviados no plano gratuito |
| Qualidade em pt-BR | Boa: treino fortemente multilíngue | Boa, um pouco inferior em instruções longas em português | Muito boa |
| Janela de contexto | 32k nativo, até 128k com extensão (usaremos `num_ctx = 8192`) | Até 128k (idem) | Muito grande (≥ 1M em alguns modelos) |
| Privacidade (roda local?) | **Sim** | **Sim** | Não: os dados vão para a nuvem do Google |
| Tool calling | Suportado no Ollama | Suportado no Ollama | Suportado |
| Integração com Python | `ollama` SDK / LangChain | `ollama` SDK / LangChain | SDK `google-genai` / LangChain |
| Maturidade / documentação | Alta | Alta | Alta |
| Requisito de hardware | ~6–8 GB RAM (quantizado Q4) | ~6–8 GB RAM (quantizado Q4) | Nenhum |

**Escolha:** **Qwen 2.5 7B Instruct via Ollama local** (ou Ollama Cloud quando o computador do aluno não tiver RAM suficiente).
**Trade-off aceito:** abrimos mão da qualidade e da janela maiores do Gemini em troca de **privacidade** (RISCO-08: histórico de falhas não sai da máquina), custo zero sem cota e reprodutibilidade. A janela menor é compensada por RAG + tools + política de histórico (seção 5). **Alternativa documentada:** Llama 3.1 8B, com o mesmo código, caso o Qwen tenha desempenho pior no eval set.

## 2. Vector store: comparação

| Critério | **ChromaDB** | FAISS |
|---|---|---|
| Custo | Zero (open source, Apache 2.0) | Zero (open source, MIT) |
| Persistência em disco | Nativa (`PersistentClient`) | Manual (salvar/carregar índice) |
| Filtro por metadados | **Nativo** (`where={"tipo_ativo": "cftv_ptz"}`) | Não nativo: precisa de camada extra |
| Integração Python / LangChain | Simples | Simples |
| Desempenho em grande escala | Adequado para milhares de chunks | Excelente para milhões de vetores |
| Curva de aprendizado | Baixa | Média |

**Escolha:** **ChromaDB**. **Trade-off aceito:** perdemos o desempenho bruto do FAISS em escala (irrelevante para cerca de 200 chunks) em troca de **filtro nativo por metadados** (TAG, tipo de ativo, tipo de documento), que é essencial para não misturar ativos (RISCO-09).

## 3. Demais camadas

| Camada | Escolha | Justificativa |
|---|---|---|
| Embeddings | `nomic-embed-text` via Ollama (local) | Custo zero e privacidade: o texto não sai da máquina. Alternativa: Sentence Transformers (`paraphrase-multilingual-MiniLM-L12-v2`). |
| Orquestração | LangChain (LCEL) | Encadeia retriever, prompt, LLM e tools de forma declarativa; reaproveita `chamar_llm()`, histórico e `montar_prompt()` do Módulo 3. |
| Interface (prevista, não obrigatória) | Gradio | Chat com poucas linhas; roda local. Mockup em `docs/mockup_interface.png`. |
| Ambiente | Python 3.10+, VS Code ou Google Colab | Declarado no README. **Nenhum código nesta sprint.** |
| Versionamento | Git + GitHub público | Obrigatório desde a Sprint 01. |

## 4. Parâmetros do modelo

| Parâmetro | Valor | Justificativa |
|---|---|---|
| `temperature` | **0.2** | Domínio técnico e factual: queremos respostas consistentes e menos criatividade (menos alucinação). Não usamos 0 para evitar repetições travadas na redação de OS. |
| `top_p` | 0.9 | Corta tokens improváveis, mantendo alguma fluidez de texto. |
| `max_tokens` (`num_predict`) | **700** | Suficiente para 4 parágrafos ou 8 itens + nível de confiança (bloco FORMATO); limita custo de contexto. |
| `num_ctx` | **8192** | Cabe em máquina de aluno (7B Q4) e comporta o orçamento da seção 5. |
| Iterações de tool por pergunta | máx. 3 | Evita loop infinito de chamadas (B5 → B6 → B3). |
| Regeneração no guardrail de saída | máx. 1 | Se falhar de novo, devolve resposta segura "não encontrei / encaminho". |

## 5. Política de contexto quantificada (bônus)

Estimativa com ≈ 1,4 token/palavra em pt-BR (a validar com o tokenizer do modelo na Sprint 03):

| Componente por chamada | Tokens estimados |
|---|---|
| System prompt (≈ 1.100 palavras com few-shot, medidas no arquivo) | ≈ 1.600 |
| Histórico: últimos 6 turnos (≈ 150 tokens por turno) + resumo dos anteriores (≈ 250) | ≈ 1.150 |
| Trechos do RAG: top-k 4 × ≈ 500 | ≈ 2.000 |
| Retorno de tools (ficha + histórico filtrado) | ≈ 1.000 |
| Pergunta do usuário | ≈ 100 |
| Reserva para a resposta (`max_tokens`) | 700 |
| **Total** | **≈ 6.550 de 8.192 (≈ 80%)**, com folga de ≈ 1.600 |

**Estratégia de recorte:** quando o histórico passar de 6 turnos, os turnos mais antigos são substituídos por um **resumo** (TAG ativo, hipóteses, ação recomendada); o **TAG ativo** é reinjetado em todo turno (R13). Se o retorno de tool for grande (ex.: 36 meses de histórico), ele é filtrado por `tipo_falha` ou resumido antes de entrar no contexto. Como a base inteira tem ≈ 15–17 mil tokens e não cabe na janela, o RAG é obrigatório, não opcional.

## 6. Verificação de licenças e janelas de contexto (07/11/2026)

| Item | Valor adotado | Status | Fonte oficial |
|---|---|---|---|
| ChromaDB: licença | Apache 2.0 | **Confirmado** | [LICENSE no GitHub](https://github.com/chroma-core/chroma/blob/main/LICENSE) |
| FAISS: licença | MIT | **Confirmado** | [Repositório no GitHub](https://github.com/facebookresearch/faiss) |
| Qwen 2.5 7B Instruct: licença | Apache 2.0 | **Confirmado** (`license: apache-2.0` no model card) | [Model card (Hugging Face)](https://huggingface.co/Qwen/Qwen2.5-7B-Instruct) |
| Qwen 2.5 7B Instruct: contexto | 131.072 tokens no total (geração até 8.192); `config.json` padrão em 32.768; acima disso exige YaRN, que pode afetar textos curtos no vLLM | **Confirmado** | idem |
| Qwen 2.5: português | Entre os mais de 29 idiomas suportados | **Confirmado** | idem |
| Llama 3.1 8B Instruct: contexto | 128k | **Confirmado** | [Model card oficial da Meta](https://github.com/meta-llama/llama-models/blob/main/models/llama3_1/MODEL_CARD.md) |
| Llama 3.1: português | Idioma oficialmente suportado (inglês, alemão, francês, italiano, português, hindi, espanhol, tailandês) | **Confirmado** | idem |
| Llama 3.1: licença | Llama 3.1 Community License (licença comercial personalizada, com Política de Uso Aceitável) | **Confirmado** (nome e existência do texto); termos detalhados não analisados | [LICENSE](https://github.com/meta-llama/llama-models/blob/main/models/llama3_1/LICENSE) |
| Ollama: Qwen 2.5 7B e Llama 3.1 8B | Disponíveis; ambos com suporte a tools; Qwen sob Apache 2.0, Llama sob a licença acima | **Confirmado** | [qwen2.5:7b](https://ollama.com/library/qwen2.5:7b) · [llama3.1:8b](https://ollama.com/library/llama3.1:8b) |
| Gemini (AI Studio, plano gratuito): uso de dados | Não afirmado | **Não verificado** (página de termos fora da rede disponível) | [Termos da Gemini API](https://ai.google.dev/gemini-api/terms) |

**Observações:**
- O limite adotado no projeto (`num_ctx = 8192`) é bem menor que os 32k nativos do Qwen e os 128k do Llama; essa escolha é de memória/hardware, não uma limitação dos modelos.
- Antes de alterar o `num_ctx` acima de 32.768 no Qwen, ler a seção de YaRN do model card.
- **Pendência:** ler os termos da Gemini API sobre uso dos dados enviados no plano gratuito. A decisão pelo Qwen local não depende disso.
