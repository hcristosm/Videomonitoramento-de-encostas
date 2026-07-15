💾 Acesso ao Dataset Experimental (Vídeos Brutos)

Devido às restrições físicas de volumetria de armazenamento do GitHub, o banco de dados visual bruto — que totaliza 100 Gigabytes distribuídos em 889 arquivos de vídeo individuais (resolução HD 720p, 30 fps, compressão H.265+, container .mp4) — encontra-se salvaguardado localmente no HDD de backup da universidade.

Para fins de reprodutibilidade científica e transparência metodológica, as 19 amostras em formato de vídeo correspondentes aos recortes temporais exatos submetidos à análise direta e auditoria cruzada deste trabalho encontram-se hospedadas permanentemente no armazenamento em nuvem.

🔗 Acesse aqui a pasta do Google Drive com as 19 Amostras em Vídeo Bruto
⚙️ Resumo Técnico das Abordagens Computacionais
1. Rotina Clássica (OpenCV)

Desenvolvida para operar de forma leve diretamente sobre Unidades Centrais de Processamento (CPUs) convencionais. O fluxo executa sequencialmente a 30 fps dentro de notebooks/pipeline_opencv.ipynb seguindo as etapas:

    Pré-processamento: Delimitação por Regiões de Interesse (ROI), conversão para matrizes em escala de cinza e aplicação de Filtro Gaussiano (9x9 pixels, sigma=0).

    Segmentação Estrutural: Extração de descontinuidades locais por Detector de Bordas de Canny (limiares de histerese 50/150) e fechamento morfológico via elemento estruturante elíptico (3x3 pixels).

    Filtragem Geométrica: Varredura topológica de contornos com base em restrição multidimensional de área (janela útil entre 20 e 5000 pixels quadrados) e cálculo do Limiar de Circularidade Geométrica (C) por meio da relação:
    C = (4 * pi * Area) / (Perímetro^2)
    Exige-se C > 0,50 para validação das projeções esféricas dos alvos artificiais (bolas de tênis de mesa de 40 mm).

    Rastreamento e Blindagem Cinemática: Associação temporal por menor distância Euclidiana em relação ao gabarito estável. Implementou-se uma trava espacial limite de busca parametrizada em 4 pixels. Em cenários de oclusão, o algoritmo retém a última coordenada válida conhecida na planilha, impedindo o fenômeno de troca de identidade (ID Switch).

2. Validação via Inteligência Artificial (SAM 2)

Implementada como um validador cruzado independente operando em modo zero-shot (sem ajuste fino) assistido por hardware gráfico (GPUs/CUDA) dentro de notebooks/pipeline_sam2.ipynb e notebooks/validação_sam2.ipynb:

    Utiliza o modelo de fundação baseado em transformadores de visão sam2.1_b.pt via biblioteca Ultralytics.

    Amostragem temporal discreta em intervalos regulares de Delta t = 30 segundos.

    Aplicação do algoritmo de agrupamento espacial baseado em densidade DBSCAN (epsilon = 4 pixels, persistência mínima de 5 quadros) para estruturar a identificação de marcadores legítimos na cena a partir das máscaras profundas binárias segmentadas.

🚀 Instruções de Execução
Instalação Local

Certifique-se de possuir o Python 3.10+ instalado no seu ambiente local.

    Clonar o Repositório:
    Bash

    git clone [https://github.com/hcristosm/Videomonitoramento-de-encostas.git](https://github.com/hcristosm/Videomonitoramento-de-encostas.git)
    cd Videomonitoramento-de-encostas

    Instalar as Dependências:
    Bash

    pip install -r requirements.txt

    Execução dos Notebooks:
    Inicie o ambiente do Jupyter Lab ou Jupyter Notebook para abrir e executar as células dos códigos experimentais:
    Bash

    jupyter lab

Execução via Google Colab

Caso prefira processar o modelo profundo (SAM 2) utilizando aceleração por hardware via GPU dedicada em nuvem, faça o upload dos arquivos contidos na pasta notebooks/ diretamente para o ambiente do Google Colab, certificando-se de montar o drive para acesso às planilhas e aos vídeos correspondentes.
📊 Redução de Dimensionalidade e IoT

O pipeline foi projetado sob os preceitos de Internet das Coisas (IoT) aplicada à gestão de riscos de desastres naturais. Em vez de transmitir fluxos densos e pesados de vídeo por redes de baixa taxa de transmissão comuns em taludes escarpados, o script reduz a dimensionalidade das matrizes de pixels brutas na borda do sistema, gerando arquivos tabulares .csv textuais altamente compactados:

    Arquivo de Vídeo Bruto (30 min - HD): ~113 Megabytes

    Planilha de Telemetria Cinemática (.csv): ~20 Megabytes por sensor

Essa otimização viabiliza o escoamento contínuo e em tempo real da telemetria para centrais remotas de monitoramento (Defesas Civis municipais, IPT e CEMADEN), contornando restrições logísticas de conectividade e largura de banda.
📜 Licença e Citação

Este repositório está sob a licença MIT. Caso utilize os algoritmos ou o conjunto de dados em pesquisas acadêmicas, cite a obra original:
Snippet de código

@mastersthesis{leptokarydis2026viabilidade,
  author    = {Leptokarydis, Mateus Hcristos},
  title     = {Viabilidade Técnica de Videomonitoramento de Baixo Custo para Detecção de Instabilidades em Encostas da Serra do Mar},
  school    = {Instituto de Geociências, Universidade Estadual de Campinas (UNICAMP)},
  year      = {2026},
  address   = {Campinas, SP},
  type      = {Dissertação (Mestrado em Geociências)}
}
}
