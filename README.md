# Videomonitoramento de Encostas - IG-UNICAMP

Este repositório armazena os códigos e dados da dissertação de mestrado de **Mateus Hcristos Leptokarydis** (Instituto de Geociências - UNICAMP).

O objetivo do trabalho é avaliar a viabilidade técnica de um sistema de videomonitoramento de baixo custo com foco em **detectar a deflagração de movimentos** em encostas vegetadas.
---

## 📁 Estrutura do Repositório

```text
Videomonitoramento-de-encostas/
├── data/
│   ├── processed/
│   │   ├── Resultados_Diurno/      # Séries temporais e validações (06/03/2025)
│   │   └── Resultados_Noturno/     # Séries temporais e validações (21/03/2025)
│   └── reference/                  # Gabaritos e ROIs de calibração
├── notebooks/                      # Códigos de execução interativa (.ipynb)
│   ├── pipeline_opencv.ipynb       # Rastreamento em OpenCV
│   ├── pipeline_sam2.ipynb         # Segmentação com SAM 2
│   └── validação_sam2.ipynb        # Scripts de auditoria e métricas (DBSCAN)
└── requirements.txt                # Dependências do projeto

---

## 💾 Acesso aos Vídeos Brutos

O banco de dados completo do projeto possui 100 GB (889 vídeos) e está salvo fisicamente no laboratório da UNICAMP. 

Para garantir a reprodutibilidade dos testes, as **19 amostras em vídeo** utilizadas na análise direta do trabalho estão disponíveis em nuvem:

🔗 **[Pasta de Vídeos no Google Drive](https://drive.google.com/drive/folders/1BJNxu7ApHlJl_VXuK1K5d1zV_L8b549b?usp=drive_link)**

---

## ⚙️ Abordagens Computacionais

### 1. Pipeline em OpenCV
Algoritmo leve desenvolvido para rodar quadro a quadro em CPU convencional:
* **Pré-processamento e Segmentação:** Recorte de Região de Interesse (ROI), escala de cinza, Filtro Gaussiano, detector de bordas de Canny e fechamento morfológico.
* **Filtragem Geométrica:** Seleção de contornos por restrição de área e cálculo do limiar de circularidade ($C > 0,50$) para identificar os alvos (bolas de 40 mm).
* **Rastreamento:** Associação temporal por menor distância Euclidiana com trava espacial limite de busca de **4 pixels** para evitar perda de rastreio (*ID Switch*) em oclusões.

### 2. Validação via SAM 2
Validação cruzada independente com aprendizado profundo operando em modo *zero-shot* (sem treino adicional):
* **Segmentação:** Uso do modelo baseado em transformadores de visão `sam2.1_b.pt` (Ultralytics) com amostragem temporal a cada 30 segundos.
* **Agrupamento:** Aplicação do algoritmo **DBSCAN** ($\epsilon = 4$ pixels) nas máscaras geradas pelo SAM 2 para validar espacialmente as detecções dos alvos legítimos.

---

## 🚀 Como Executar

### 1. Instalação Local
Requisitos: Python 3.10 ou superior.

No terminal, execute:
```bash
git clone [https://github.com/hcristosm/Videomonitoramento-de-encostas.git](https://github.com/hcristosm/Videomonitoramento-de-encostas.git)
cd Videomonitoramento-de-encostas
pip install -r requirements.txt
jupyter lab

2. Execução no Google Colab

Os notebooks da pasta notebooks/ podem ser importados e executados diretamente no Google Colab, aproveitando a GPU em nuvem para acelerar o processamento do SAM 2.

---

## 📊 Redução de Dados para IoT

O processamento na borda elimina a necessidade de transmitir fluxos densos de vídeo (113 MB por arquivo de 30 minutos) por conexões móveis instáveis. 

O sistema reduz a dimensionalidade das imagens gerando tabelas compactas `.csv` com as coordenadas X e Y dos alvos (cerca de 20 MB por sensor), facilitando o envio de dados e a geração de alertas em tempo real.

---

## 📜 Licença e Citação

Este projeto está sob a licença MIT. Para citar este trabalho em pesquisas acadêmicas:

```bibtex
@mastersthesis{leptokarydis2026viabilidade,
  author    = {Leptokarydis, Mateus Hcristos},
  title     = {Viabilidade Técnica de Videomonitoramento de Baixo Custo para Detecção de Instabilidades em Encostas da Serra do Mar},
  school    = {Instituto de Geociências, Universidade Estadual de Campinas (UNICAMP)},
  year      = {2026},
  address   = {Campinas, SP},
  type      = {Dissertação (Mestrado em Geociências)}
}
