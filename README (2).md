# Whisper large-v3 e os sotaques do Brasil

Auditoria sociolinguística do Whisper large-v3 (OpenAI) em dialetos do português brasileiro.

## O que é este projeto

O projeto mede **quanto o Whisper erra** e **quão rápido ele transcreve** quando a fala tem sotaque regional. Foram usados 13 trechos de 80 segundos de fala real, tirados de vídeos públicos:

- 12 trechos de sotaques regionais: dois falantes para cada um dos seis subfalares da classificação de Nascentes (amazônico, baiano, nordestino, fluminense, mineiro e sulista);
- 1 trecho de controle, com um falante paulistano.

Cada trecho foi transcrito à mão por pessoas, e essa transcrição serve de gabarito (*ground truth*). O notebook compara o gabarito com o que o Whisper escreveu.

## Principais resultados

| Medida | Resultado |
|---|---|
| Erro de palavras (WER) no controle paulistano | 5,06% |
| Erro de palavras (WER) médio nos 12 trechos regionais | 17,66% (cerca de 3,5 vezes mais) |
| Velocidade (RTF) média | 0,20, ou cerca de 5 vezes mais rápido que o tempo real |
| Tipo de erro mais comum | Substituição de palavras (258 de 465 erros) |

Em resumo: o Whisper erra mais nos sotaques regionais, mas **não fica mais lento quando erra**. O erro não avisa, e o texto sai coerente mesmo errado.

## Como as medidas são calculadas

- **WER (taxa de erro de palavras):** `(substituições + deleções + inserções) ÷ palavras do gabarito`. Quanto menor, melhor.
- **RTF (fator de tempo real):** `tempo de processamento ÷ duração do áudio`. Abaixo de 1, o modelo é mais rápido que o áudio.

Antes de comparar, os dois textos passam pela mesma limpeza: letras minúsculas, sem pontuação e com espaços unificados. Os acentos são mantidos.

## Como o notebook funciona

O arquivo `whisper_dialetos.ipynb` roda no Google Colab, em seis etapas:

1. **Ambiente:** instala as bibliotecas e confere a GPU.
2. **Áudios:** baixa e corta os trechos com `yt-dlp` (opção 2.1) ou carrega um `audios.zip` pronto (opção 2.2).
3. **Transcrição:** o Whisper transcreve cada trecho. O tempo de cada um é a mediana de 3 execuções, depois de uma rodada de aquecimento descartada.
4. **Gabarito:** carrega as transcrições humanas (`GSGT_transcricoes.zip`).
5. **WER:** calcula os erros de cada trecho com a biblioteca `jiwer`.
6. **Análise com IA e consolidação:** o Gemini (`gemini-3-flash-preview`) gera uma análise de risco para cada trecho, a partir da transcrição humana. Depois, os resultados são juntos num arquivo que alimenta o dashboard.

## Como rodar

1. Abra o notebook no Google Colab.
2. Escolha **Ambiente de execução → Alterar tipo de ambiente de execução → GPU T4**.
3. Para a análise com o Gemini, guarde sua chave em **Secrets** (ícone de chave, na lateral esquerda) com o nome `GEMINI_API_KEY`.
4. Execute as células em ordem. Na etapa 2, use **apenas uma** das opções:
   - 2.1: substitua os links de exemplo em `clips.csv` pelos seus;
   - 2.2: envie o seu `audios.zip`.
5. Na etapa 4, envie o `GSGT_transcricoes.zip` quando o Colab pedir.

## O que há neste repositório

```text
├── whisper_dialetos.ipynb         # Notebook com todo o pipeline
├── docs/
│   ├── relatorio_final.docx       # Relatório final do estudo
│   └── sintese_metodologica.pdf   # Síntese metodológica (2 páginas)
├── saida/
│   └── base_dashboard_final.json  # Base final usada no dashboard
└── README.md
```

Os demais arquivos de `saida/` (`tempos.csv`, `wer_detalhado.csv`, `base_dashboard.csv`) e a pasta `audios/` são gerados ao rodar o notebook. Os áudios não estão aqui porque vêm de vídeos públicos de terceiros.

## Limites do estudo

É um estudo exploratório: são só 13 trechos e um ou dois falantes por subfalar. Os números descrevem esta amostra e não permitem generalizar para o dialeto inteiro.

## Autoria

Texto e análise: Isabella Manchon. Transcrições de referência: Mariana Coan. Trabalho feito na disciplina de Inteligência Artificial e Aplicações em Ciências Sociais (ESPM). Os códigos Python foram escritos com assistência de IA e ajustados pelas autoras.
