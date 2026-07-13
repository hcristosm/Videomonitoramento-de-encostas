# Viabilidade Técnica de Videomonitoramento de Baixo Custo para Detecção de Instabilidades em Encostas da Serra do Mar

Este repositório armazena o ecossistema de dados, planilhas cinemáticas e rotinas computacionais interativas desenvolvidos no âmbito da dissertação de mestrado de **Mateus Hcristos Leptokarydis**, junto ao Programa de Pós-Graduação em Geociências do Instituto de Geociências da Universidade Estadual de Campinas (IG-UNICAMP).

O projeto consiste no desenvolvimento e auditoria de um sistema de instrumentação óptica superficial contínua voltado à detecção da deflagração de movimentos de massa em encostas vegetadas, utilizando sensores (câmeras) convencionais de segurança patrimonial  e técnicas híbridas de Visão Computacional.

---

## 📁 Estrutura Real do Repositório

```text
Videomonitoramento-de-encostas/
├── data/
│   ├── processed/
│   │   ├── Resultados_Diurno/      # Séries temporais (06/03/2025)
│   │   │   ├── tracking_ch1_MHDX_ch1_main_20250306110000_20250306113000.csv
│   │   │   ├── tracking_ch2_MHDX_ch2_main_20250306110000_20250306113000.csv
│   │   │   ├── tracking_ch3_MHDX_ch3_main_20250306110000_20250306113000.csv
│   │   │   ├── validacao_ch1_MHDX_ch1_main_20250306110000_20250306113000.txt
│   │   │   ├── validacao_ch2_MHDX_ch2_main_20250306110000_20250306113000.txt
│   │   │   ├── validacao_ch3_MHDX_ch3_main_20250306110000_20250306113000.txt
│   │   │   └── validacao_ch4_MHDX_ch4_main_20250306110000_20250306113000.txt
│   │   └── Resultados_Noturno/     # Séries temporais (21/03/2025)
│   │       ├── tracking_ch1_MHDX_ch1_main_20250321000000_20250321003000.csv
│   │       ├── tracking_ch2_MHDX_ch2_main_20250321000000_20250321003000.csv
│   │       ├── tracking_ch4_MHDX_ch4_main_20250321000000_20250321003000.csv
│   │       ├── validacao_ch1_MHDX_ch1_main_20250321000000_20250321003000.txt
│   │       ├── validacao_ch2_MHDX_ch2_main_20250321000000_20250321003000.txt
│   │       ├── validacao_ch3_MHDX_ch3_main_20250321000000_20250321003000.txt
│   │       └── validacao_ch4_MHDX_ch4_main_20250321000000_20250321003000.txt
│   └── reference/                  # Diretório de gabaritos e ROIs de calibração
├── notebooks/                      # Código-fonte interativo (.ipynb)
│   ├── pipeline_opencv.ipynb       # Execução da rotina clássica quadro a quadro
│   ├── pipeline_sam2.ipynb         # Segmentação por aprendizado profundo
│   └── validação_sam2.ipynb        # Scripts de auditoria, DBSCAN e matriz de confusão
├── requirements.txt                # Dependências do ecossistema computacional
└── src/                            # Diretório de suporte estrutural
