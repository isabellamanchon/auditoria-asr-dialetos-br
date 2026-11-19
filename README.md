# Whisper large-v3 e os sotaques do Brasil

Auditoria sociolinguística do Whisper large-v3 (OpenAI) em dialetos do português brasileiro. O projeto mede a taxa de erro de palavras (WER) e a velocidade (RTF) do modelo em 13 trechos de 80 s de fala real: 12 de sotaques regionais (dois falantes por subfalar de Nascentes) e 1 de controle paulistano. O gabarito é uma transcrição humana de cada trecho.

## Resultados

| Medida | Resultado |
|---|---|
| WER do controle paulistano | 5,06% |
| WER médio dos 12 trechos regionais | 17,66% (cerca de 3,5 vezes mais) |
| RTF médio | 0,20 (cerca de 5 vezes mais rápido que o tempo real) |
| Erros por tipo | 55% substituições, 26% deleções, 19% inserções |

O Whisper erra mais nos sotaques regionais, mas não fica mais lento quando erra.

## Como rodar

1. Abra `ASR_dialetos_BR_benchmark.ipynb` no Google Colab e selecione GPU T4.
2. Guarde a chave do Gemini em Secrets, com o nome `GEMINI_API_KEY`.
3. Execute as células em ordem: envie os áudios (`audios.zip`) ou baixe-os com `yt-dlp`, e depois as transcrições de referência (`GSGT_transcricoes.zip`).

O notebook transcreve, calcula WER e RTF (mediana de 3 execuções) e consolida os resultados em `saida/`.

## Conteúdo

```text
├── whisper_dialetos.ipynb         # Pipeline completo
├── docs/                          # Relatório final e síntese metodológica
├── saida/base_dashboard_final.json  # Base usada no dashboard
└── README.md
```

Estudo exploratório, com um ou dois falantes por subfalar: os números descrevem esta amostra.

Texto e análise: Isabella Manchon. Transcrições de referência: Mariana Coan. ESPM, 2026.
