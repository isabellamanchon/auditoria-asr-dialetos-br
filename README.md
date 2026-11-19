Whisper large-v3 Benchmark: Auditoria Sociolinguística de Dialetos Regionais (PT-BR)

Este repositório contém um pipeline de auditoria em Python/Google Colab desenvolvido para avaliar o desempenho do modelo de Reconhecimento Automático de Fala (ASR) OpenAI Whisper (large-v3) na transcrição de 13 fragmentos de 80 segundos de fala do Português Brasileiro: seis subfalares da classificação de Nascentes, com dois falantes cada, mais um falante paulistano como controle.

O objetivo da pesquisa é analisar empiricamente se há variações significativas de precisão (bias algorítmico) associadas a sotaques, regionalismos e especificidades fonéticas do Brasil.

Métricas Avaliadas

O benchmark quantifica duas dimensões fundamentais do modelo:

Word Error Rate (WER): Taxa de Erro de Palavras calculada via alinhamento de distância de Levenshtein (utilizando a biblioteca jiwer). A hipótese gerada pela IA é confrontada dinamicamente contra a Ground Truth humana (transcrição manual de referência).
$$\text{WER} = \frac{S + D + I}{N}$$
(Onde $S$ = Substituições, $D$ = Deleções, $I$ = Inserções e $N$ = Total de palavras da referência).
Real-Time Factor (RTF): Eficiência computacional medida pela razão entre o tempo de inferência e a duração total do áudio.
$$\text{RTF} = \frac{\text{Tempo de Processamento (s)}}{\text{Duração do Áudio (s)}}$$
Estrutura do Pipeline

O notebook principal (whisper_dialetos.ipynb) está dividido em 6 etapas lógicas e modulares:

Configuração do Ambiente: Verificação da aceleração de hardware (GPU T4) e instalação do openai-whisper e jiwer.
Ingestão e Processamento dos Áudios: Coleta automatizada de amostras via yt-dlp (corte em 16 kHz mono) ou upload do arquivo audios.zip.
Ingestão da Ground Truth: Carregamento das transcrições humanas de referência (GSGT_transcricoes.zip).
Cálculo do WER (com Normalização em Memória): Limpeza dinâmica dos textos (conversão para minúsculas, remoção de pontuações e padronização de espaços) e alinhamento via jiwer.
Análise Qualitativa (Gemini): Geração, via API do Gemini (gemini-3-flash-preview), de uma análise de risco de cada fragmento a partir da transcrição humana.
Consolidação do Dataset: Cruzamento dos resultados computacionais (WER e RTF) com os metadados sociolinguísticos (dialeto, região, cidade e falante), exportando a base final base_dashboard.csv.
📂 Estrutura de Diretórios
text
├── whisper_dialetos.ipynb         # Notebook principal do pipeline no Colab
├── docs/
│   ├── relatorio_final.docx       # Relatório final do estudo
│   └── sintese_metodologica.pdf   # Síntese metodológica (2 páginas)
├── saida/
│   └── base_dashboard_final.json  # Base final usada pelo dashboard, com as métricas reais
└── README.md                      # Documentação do projeto

Os demais arquivos de saida/ (tempos.csv, wer_detalhado.csv, base_dashboard.csv, base_dashboard.json e as transcrições do Whisper) são gerados ao executar o notebook. Os áudios (audios/) também são gerados por ele, a partir dos links e tempos de corte em clips.csv; eles não são versionados aqui por pertencerem a vídeos públicos de terceiros.
