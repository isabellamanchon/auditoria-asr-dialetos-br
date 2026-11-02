# Whisper large-v3 Benchmark: Auditoria Sociolinguística de Dialetos Regionais (PT-BR)

Este repositório contém um pipeline de auditoria em Python/Google Colab desenvolvido para avaliar o desempenho do modelo de Reconhecimento Automático de Fala (ASR) **OpenAI Whisper (large-v3)** na transcrição de **12 variações dialetais do Português Brasileiro**.

O objetivo da pesquisa é analisar empiricamente se há variações significativas de precisão (*bias* algorítmico) associadas a sotaques, regionalismos e especificidades fonéticas do Brasil.

---

## Métricas Avaliadas

O benchmark quantifica duas dimensões fundamentais do modelo:

1. **Word Error Rate (WER):** Taxa de Erro de Palavras calculada via alinhamento de distância de Levenshtein (utilizando a biblioteca `jiwer`). A hipótese gerada pela IA é confrontada dinamicamente contra a **Ground Truth humana** (transcrição manual de referência).
   $$\text{WER} = \frac{S + D + I}{N}$$
   *(Onde $S$ = Substituições, $D$ = Deleções, $I$ = Inserções e $N$ = Total de palavras da referência)*.

2. **Real-Time Factor (RTF):** Eficiência computacional medida pela razão entre o tempo de inferência e a duração total do áudio.
   $$\text{RTF} = \frac{\text{Tempo de Processamento (s)}}{\text{Duração do Áudio (s)}}$$

---

## Estrutura do Pipeline

O notebook principal (`whisper_dialetos.ipynb`) está dividido em 5 etapas lógicas e modulares:

1. **Configuração do Ambiente:** Verificação da aceleração de hardware (GPU T4) e instalação do `openai-whisper` e `jiwer`.
2. **Ingestão e Processamento dos Áudios:** Coleta automatizada de amostras via `yt-dlp` (corte em 16 kHz mono) ou upload do arquivo `audios.zip`.
3. **Ingestão da Ground Truth:** Carregamento das transcrições humanas de referência (`refs.zip`).
4. **Cálculo do WER (com Normalização em Memória):** Limpeza dinâmica dos textos (conversão para minúsculas, remoção de pontuações e padronização de espaços) e alinhamento via `jiwer`.
5. **Consolidação do Dataset:** Cruzamento dos resultados computacionais (WER e RTF) com os metadados sociolinguísticos (dialeto, região, cidade e falante), exportando a base final `base_dashboard.csv`.

---

## 📂 Estrutura de Diretórios

```text
├── whisper_dialetos.ipynb    # Notebook principal do pipeline no Colab
├── saida/
│   ├── tempos.csv             # Tempos de inferência por execução e medição do RTF
│   ├── wer_detalhado.csv      # Mapeamento fino de Substituições, Deleções e Inserções
│   └── base_dashboard.csv     # Dataset consolidado com metadados regionais
└── README.md                  # Documentação do projeto
