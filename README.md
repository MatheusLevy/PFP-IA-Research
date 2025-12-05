# Repositório de Pesquisa em Machine Learning para Detecção de Objetos (MLOps)

## Descrição

Este repositório funciona como um **ambiente integrado para pesquisa e desenvolvimento de modelos de Machine Learning**, focado especificamente em detecção de objetos utilizando a arquitetura YOLO. Ele oferece uma estrutura robusta para que pesquisadores possam conduzir experimentos, desde a ingestão e preparação de dados até o treinamento, otimização e versionamento de modelos, garantindo reprodutibilidade e rastreabilidade em todo o ciclo de vida do desenvolvimento de ML.

### Principais Funcionalidades

O programa oferece as seguintes funções específicas e relevantes:
*   **Injestão e Preparação de Dados**: Permite puxar e preparar versões específicas de conjuntos de dados anotados (gerenciados externamente via DVC), além de processá-los e dividi-los em conjuntos de treino, validação e teste.
*   **Análise Exploratória de Dados (EDA)**: Fornece notebooks para análise aprofundada dos datasets, incluindo distribuição de classes, tamanhos de bounding box, análise espacial e identificação de amostras negativas.
*   **Treinamento e Otimização de Modelos**: Facilita o treinamento de modelos YOLO para detecção de objetos e a otimização de seus hiperparâmetros utilizando Optuna.
*   **Rastreamento e Versionamento de Experimentos**: Integração com Weights & Biases (WandB) para rastreamento de métricas e visualização de experimentos, e com MLflow para o registro e versionamento de modelos, parâmetros e artefatos de cada experimento.
*   **Gestão de Modelos**: Permite o registro e recuperação de modelos versionados no MLflow para avaliação e implantação.

### Público-Alvo Primário

Este programa foi concebido para atender, primordialmente:
*   **Pesquisadores especializados em Machine Learning e Visão Computacional**, especialmente aqueles que trabalham com detecção de objetos.
*   **Engenheiros de Machine Learning (MLOps)** que buscam padronizar e automatizar o ciclo de vida de experimentos e modelos.
*   **Estudantes de graduação e pós-graduação** em disciplinas como Ciência da Computação e Engenharia de Machine Learning, que necessitam de uma plataforma prática para aprender e aplicar as melhores práticas de MLOps.

### Natureza do Projeto

Este repositório é uma **plataforma de pesquisa e um Módulo de Sistema em Desenvolvimento** (parte de uma suíte maior de projetos acadêmicos - PFP). Ele funciona como uma **prova de conceito funcional e simplificada**, demonstrando um fluxo de trabalho completo para o desenvolvimento de modelos de detecção de objetos em um ambiente MLOps, com foco em reprodutibilidade e rastreabilidade.

### Ressalvas Importantes

Este projeto, assim como os demais da suíte PFP, é um **Minimum Viable Product (MVP)**. Ele representa uma **versão inicial e demonstrativa** do pipeline de pesquisa, desenvolvida com fins educacionais, e será **atualizada e incrementada gradualmente**. Portanto, os interessados devem estar cientes de que:
*   Trata-se de uma ferramenta de caráter **primariamente didático e demonstrativo**.
*   Embora utilize ferramentas de nível industrial (WandB, MLflow, Optuna), a configuração local via `docker-compose.yaml` é para **desenvolvimento e testes em pequena escala**. Em ambientes de produção, estes serviços são acessados através de uma infraestrutura de cluster Kubernetes.
*   Pode não conter todas as otimizações e a robustez exigidas por um ambiente de pesquisa em escala industrial.
*   A estrutura e o código (especialmente os notebooks) estão sujeitos a evoluções e mudanças significativas ao longo do tempo.

## Visão de Projeto

Esta seção descreve a experiência dos usuários através de cenários, orientando o projeto, o uso e a eventual evolução do software.

### Cenário Positivo 1: Realizando um Novo Experimento de Treinamento

**Persona:** Dr. Lucas, um pesquisador de Machine Learning.
**Contexto:** Lucas precisa treinar uma nova versão de um modelo YOLO para um problema de detecção de objetos. Ele deseja utilizar a versão mais recente dos dados anotados, experimentar uma nova arquitetura YOLO (e.g., `yolo12n.pt`), otimizar os hiperparâmetros e registrar todo o processo e os resultados para futuras comparações e possível reutilização em um ambiente de produção. Ele trabalha com notebooks Jupyter para a flexibilidade de experimentação.

**Narrativa:** Dr. Lucas começa seu dia acessando o repositório de pesquisa. Primeiro, ele garante que sua cópia local dos dados está atualizada, puxando a última versão do dataset versionado via DVC. Em seguida, ele abre o notebook `02-model_training.ipynb`. Ele define os hiperparâmetros iniciais, aponta para a arquitetura YOLO desejada e configura a integração com o WandB para monitorar as métricas de treinamento em tempo real, além do MLflow para registrar o modelo treinado e seus artefatos. Ao executar as células do notebook, o treinamento começa. Lucas acompanha a curva de perda e as métricas de validação no painel do WandB, ajustando pequenas configurações se necessário. Ao final, o modelo treinado, junto com seus parâmetros e métricas finais, é automaticamente salvo e versionado no MLflow, associado a um experimento específico. Ele pode revisar facilmente este experimento mais tarde, comparar com outros e até baixar o modelo versionado diretamente do MLflow para testes ou implantação.

### Cenário Positivo 2: Otimizando Hiperparâmetros para Melhorar a Performance

**Persona:** Dra. Sofia, uma cientista de dados júnior na equipe de Lucas.
**Contexto:** Sofia foi incumbida de melhorar a performance do modelo YOLO existente. Ela acredita que a otimização dos hiperparâmetros pode trazer ganhos significativos. Ela precisa de uma ferramenta que a ajude a explorar eficientemente o espaço de hiperparâmetros, registrar os resultados de cada tentativa e identificar a melhor combinação.

**Narrativa:** Sofia inicia seu trabalho abrindo o notebook `02-model_training.ipynb`, que já possui seções preparadas para otimização com Optuna. Ela define o espaço de busca para os hiperparâmetros (e.g., taxa de aprendizado, tamanho de batch, *momentum*) e a métrica objetivo a ser minimizada/maximizada. Cada *trial* do Optuna é configurado para ser executado como um experimento separado no MLflow e suas métricas são enviadas para o WandB. Ao iniciar o processo, o Optuna automaticamente agenda e executa várias rodadas de treinamento com diferentes combinações de hiperparâmetros. Sofia pode visualizar no painel do WandB a evolução da performance em cada trial e, no MLflow, tem um registro detalhado de cada modelo treinado, com seus respectivos hiperparâmetros e métricas. Após algumas horas, o Optuna converge para uma combinação de hiperparâmetros que entrega a melhor performance de validação. Sofia utiliza os *logs* do MLflow para identificar e recuperar o modelo correspondente a esse *trial* otimizado, pronta para uma avaliação mais aprofundada ou *deploy*.

### Cenário Negativo 1: Dificuldade em Reproduzir um Experimento Antigo

**Persona:** Dr. Lucas, o pesquisador.
**Contexto:** Lucas precisa reproduzir os resultados de um experimento realizado há três meses por um colega, que gerou um modelo de performance promissora. Embora o modelo esteja versionado no MLflow e as métricas no WandB, o ambiente de execução original e a versão exata do dataset não foram explicitamente documentados no commit Git associado.

**Narrativa:** Lucas começa tentando puxar a versão do código e os dados do DVC que ele acredita que correspondem ao experimento. Ele usa `git checkout` para o commit associado ao registro do modelo no MLflow e `dvc pull` para sincronizar os dados. No entanto, ao tentar executar o notebook de treinamento, ele encontra erros de compatibilidade de bibliotecas Python. O `uv.lock` no commit antigo não funciona perfeitamente com a versão atual do Python em sua máquina. Além disso, o `data.dvc` que ele puxou parece estar faltando alguns arquivos ou a estrutura esperada para o notebook de treinamento. Ele perde horas tentando depurar as dependências do ambiente e rastrear a versão exata do dataset que o modelo foi treinado, percebendo que, embora as ferramentas de MLOps versionem artefatos, a falta de uma documentação mais granular sobre o ambiente e a exata proveniência dos dados (além do ponteiro DVC) leva a um gasto significativo de tempo e frustração, destacando a importância de registrar não só "o que" foi feito, mas "como" e "com o que".

## Documentação Técnica do Projeto: Repositório de Pesquisa em Machine Learning
### 1. Especificação de Requisitos

#### 1.1. Requisitos Funcionais
**RF01 - Acesso e Sincronização de Dados**: O sistema deve permitir que os pesquisadores puxem a versão mais atual ou uma versão específica de conjuntos de dados anotados, gerenciados pelo repositório de dados DVC.
**RF02 - Preparação e Divisão de Dados**: Deve fornecer ferramentas (via notebooks) para a preparação, processamento e divisão dos dados em conjuntos de treino, validação e teste (freeze, accumulative, drift) para o treinamento de modelos YOLO.
**RF03 - Análise Exploratória de Dados (EDA)**: Deve permitir a realização de análises descritivas e visualizações dos datasets (distribuição de classes, análise de bounding box, amostras negativas, etc.) para insights sobre os dados.
**RF04 - Treinamento de Modelos YOLO**: Deve possibilitar o treinamento de modelos YOLO para detecção de objetos, com flexibilidade para testar diferentes arquiteturas e configurações.
**RF05 - Otimização de Hiperparâmetros**: Deve integrar uma ferramenta de otimização de hiperparâmetros (Optuna) para encontrar as melhores configurações de modelo automaticamente.
**RF06 - Rastreamento de Métricas e Visualização de Experimentos**: Deve integrar-se com Weights & Biases (WandB) para registrar métricas de treinamento, visualizações e *logs* de forma centralizada.
**RF07 - Registro e Versionamento de Modelos e Artefatos**: Deve integrar-se com MLflow para registrar modelos treinados, seus parâmetros, métricas, código-fonte e artefatos, facilitando o versionamento e a rastreabilidade.
**RF08 - Recuperação de Experimentos e Modelos**: Deve permitir a recuperação de parâmetros, métricas e modelos registrados em experimentos anteriores através do MLflow.
**RF09 - Validação e Consistência de Splits**: Deve prover funcionalidades (via notebooks) para validar a consistência entre os metadados de divisão de dados (`dataset_info.csv`) e a estrutura física dos diretórios.

#### 1.2. Requisitos Não-Funcionais
**RNF01 - Reprodutibilidade**: Todos os experimentos (divisão de dados, treinamento, otimização) devem ser configuráveis para serem reproduzíveis, utilizando versionamento de código, dados e dependências.
**RNF02 - Usabilidade**: O fluxo de trabalho deve ser intuitivo, com a maioria das operações conduzidas através de notebooks Jupyter, acessíveis para pesquisadores e cientistas de dados.
**RNF03 - Escalabilidade (Infraestrutura MLOps)**: Embora o desenvolvimento local seja suportado por Docker Compose, o sistema deve ser compatível com a infraestrutura MLOps em cluster Kubernetes para os serviços de MLflow e WandB em produção.
**RNF04 - Manutenibilidade**: O código deve ser modular e bem documentado, facilitando a compreensão, a modificação e a extensão por outros colaboradores.
**RNF05 - Flexibilidade**: Deve permitir a fácil integração de novas arquiteturas de modelo, datasets e ferramentas de MLOps.
**RNF06 - Desempenho (Local)**: A configuração local deve permitir execução razoável de experimentos em máquinas de desenvolvimento para iterações rápidas.

### 2. Arquitetura e Modelo de Dados

#### 2.1. Arquitetura do Sistema
O repositório de pesquisa opera como um ecossistema MLOps, orquestrado principalmente por notebooks Jupyter, que interagem com diversas ferramentas e serviços:

*   **Git**: O sistema de controle de versão primário para todo o código-fonte, notebooks Jupyter, configurações (como `.dvc/config`, `.env`, `pyproject.toml`) e metadados de outras ferramentas.
*   **DVC (Data Version Control)**: Utilizado para puxar versões específicas de datasets do repositório de gerenciamento de dados. Ele gerencia o diretório `data/`, que contém os ponteiros (`data.dvc`) para os datasets reais armazenados em um remoto S3.
*   **Jupyter Notebooks**: São a interface principal para os pesquisadores. Cada notebook (`00-data_ingestion.ipynb`, `01-data_analysis.ipynb`, `02-model_training.ipynb`, `03-model-redeploy.ipynb`) encapsula uma etapa do fluxo de trabalho de pesquisa.
*   **Weights & Biases (WandB)**: Integra-se aos notebooks para rastrear e visualizar métricas de experimentos de treinamento (e.g., loss, mAP), curvas de validação e *logs* de forma interativa.
*   **MLflow**: Serve para o rastreamento de experimentos, registro e versionamento de modelos. Ele salva parâmetros, métricas e artefatos de modelos (como os arquivos `.pt` do YOLO) em um *artifact store* (Minio/S3) e metadados em um *backend store* (PostgreSQL).
*   **Optuna**: Uma biblioteca para otimização de hiperparâmetros, integrada aos notebooks e capaz de registrar cada *trial* como um experimento no MLflow/WandB.
*   **YOLO (You Only Look Once)**: O framework de detecção de objetos utilizado para o treinamento e avaliação dos modelos.
*   **Docker e Docker Compose**: Usados para configurar e orquestrar localmente os serviços de MLflow (servidor, PostgreSQL) e Minio (compatível com S3 para artefatos MLflow), simulando o ambiente de produção em Kubernetes.

#### 2.2. Fluxo de Dados

O fluxo de dados no repositório de pesquisa segue os seguintes passos:

```
[Repositório de Gerenciamento de Dados (externo)]
           ↓ (dvc pull)
[Repositório de Pesquisa (data/)]
           ↓ (00-data_ingestion.ipynb)
[Repositório de Pesquisa (dataset/ - splits de dados)]
           ↓ (01-data_analysis.ipynb)
[Repositório de Pesquisa (analysis/ - resultados EDA)]
           ↓ (02-model_training.ipynb - Treinamento YOLO + Optuna)
[MLflow Server (Metadados de Experimentos & Modelos Versionados)] <--------> [WandB (Métricas & Visualizações)]
           ↓
[MLflow Artifact Store (Minio/S3 - Modelos Treinados .pt)]
           ↓ (03-model-redeploy.ipynb)
[Modelo para Implantação/Uso]
```

#### 2.3. Modelo de Dados e Estrutura de Pastas

A estrutura de diretórios é projetada para organizar o código, os dados de entrada, os dados processados, os resultados de análise e os artefatos de experimentos.

```
.
├── .dvc/                   # Configuração interna e cache do DVC para os dados de entrada
├── .dvcignore              # Padrões de arquivos que o DVC deve ignorar
├── .env                    # Variáveis de ambiente para configurações locais (Docker Compose, credenciais)
├── .git/                   # Repositório Git para versionamento de código, notebooks e metadados
├── .gitignore              # Padrões de arquivos e diretórios que o Git deve ignorar
├── .python-version         # Especifica a versão do Python utilizada (3.14)
├── .venv/                  # Ambiente virtual Python para isolar dependências
├── 00-data_ingestion.ipynb # Notebook para puxar dados, processar e criar splits de treino/val/teste
├── 01-data_analysis.ipynb  # Notebook para análise exploratória de dados (EDA)
├── 02-model_training.ipynb # Notebook para treinamento de modelos YOLO, otimização (Optuna) e rastreamento (WandB/MLflow)
├── 03-model-redeploy.ipynb # Notebook para avaliação final e preparação para implantação de modelos
├── README.md               # Documentação principal do repositório
├── analysis/               # Contém resultados de EDA (gráficos, relatórios, imagens sem objetos)
│   ├── bbox_scatter_plots.png
│   ├── ... (outros arquivos de análise)
├── data/                   # Diretório de entrada para os dados brutos versionados pelo DVC (link simbólico ou checkout)
│   ├── data.yaml           # Arquivo YAML com informações do dataset (classes, etc.)
│   ├── images              # Imagens brutas
│   └── labels              # Anotações brutas
├── data.dvc                # Arquivo DVC que referencia o diretório 'data/' no repositório de dados
├── dataset/                # Diretório de saída para os dados após processamento e divisão em splits
│   ├── data_accumulative.yaml # Configuração YOLO para split acumulativo
│   ├── data_drift.yaml     # Configuração YOLO para split de drift
│   ├── data_freeze.yaml    # Configuração YOLO para split freeze
│   ├── dataset_info.csv    # Metadados detalhados dos splits (imagens, classes, iterações)
│   ├── dataset_info.csv.dvc # Arquivo DVC para versionar o CSV de metadados
│   ├── test_accumulative/  # Subdiretório para split de teste acumulativo
│   ├── test_drift/         # Subdiretório para split de teste de drift
│   ├── test_freeze/        # Subdiretório para split de teste freeze
│   ├── train/              # Subdiretório para split de treino
│   └── valid/              # Subdiretório para split de validação
├── docker-compose.yaml     # Configuração para serviços locais (MLflow, Minio, PostgreSQL)
├── mlflow/                 # Configuração para o serviço MLflow (e.g., Dockerfile)
│   └── Dockerfile
├── pyproject.toml          # Definições do projeto Python e dependências
├── runs/                   # Diretório para *logs* e artefatos temporários de treinamento YOLO
│   └── detect
├── uv.lock                 # Arquivo de bloqueio de dependências Python (gerado por `uv`)
├── wandb/                  # Diretório de *logs* e cache do Weights & Biases
│   └── ...
├── yolo11n.pt              # Modelo YOLO pré-treinado ou checkpoints
├── yolo12n.pt              # Modelo YOLO pré-treinado ou checkpoints
├── yolo12s.pt              # Modelo YOLO pré-treinado ou checkpoints
└── yolo_hyperparameter_optimization.db # Banco de dados do Optuna para otimização de hiperparâmetros
```

### 3. Modelo Funcional e Fluxos de Trabalho

Este repositório suporta um fluxo de trabalho de pesquisa iterativo e reprodutível, principalmente através da execução dos notebooks Jupyter em sequência:

#### 3.1. Fluxo de Ingestão e Preparação de Dados (00-data_ingestion.ipynb)
1.  **Pull de Dados Brutos**: O pesquisador inicia puxando a versão mais recente ou uma versão específica dos dados anotados do repositório de gerenciamento de dados utilizando `dvc pull`. Estes dados são armazenados na pasta `data/`.
2.  **Processamento e Divisão**: O notebook `00-data_ingestion.ipynb` é executado para:
    *   Analisar os dados brutos em `data/`.
    *   Aplicar uma estratégia de divisão estratificada para criar conjuntos de `train`, `valid` e vários tipos de `test` (freeze, accumulative, drift) na pasta `dataset/`.
    *   Gerar arquivos de configuração YOLO (`data_freeze.yaml`, etc.) para cada split.
    *   Registrar metadados detalhados sobre a divisão (`dataset_info.csv`) e as estatísticas dos dados.
3.  **Versionamento de Metadados de Dados**: O arquivo `dataset_info.csv` e os arquivos `.yaml` gerados em `dataset/` são versionados via DVC (`dataset/dataset_info.csv.dvc`) e Git, garantindo que a configuração de cada split seja rastreável.

#### 3.2. Fluxo de Análise Exploratória de Dados (01-data_analysis.ipynb)
1.  **Carga de Dados Processados**: O pesquisador utiliza o notebook `01-data_analysis.ipynb` para carregar os dados já divididos na pasta `dataset/`.
2.  **Geração de Visualizações e Relatórios**: O notebook realiza análises como:
    *   Distribuição de classes (global e por split).
    *   Tamanhos e proporções de *bounding boxes*.
    *   Análise de viés espacial (onde os objetos aparecem na imagem).
    *   Identificação de imagens sem objetos (amostras negativas).
3.  **Armazenamento de Resultados da Análise**: Os gráficos e relatórios gerados são salvos na pasta `analysis/` para documentação e referência.

#### 3.3. Fluxo de Treinamento e Otimização de Modelos (02-model_training.ipynb)
1.  **Configuração do Experimento**: O pesquisador configura os parâmetros de treinamento, a arquitetura YOLO a ser utilizada e as integrações com WandB e MLflow no notebook `02-model_training.ipynb`.
2.  **Treinamento do Modelo**: O treinamento do modelo YOLO é iniciado, utilizando os dados da pasta `dataset/train` e validado com `dataset/valid`.
3.  **Rastreamento de Métricas (WandB)**: Durante o treinamento, métricas (loss, mAP, etc.), *logs* e visualizações são automaticamente enviados para o painel do WandB.
4.  **Otimização de Hiperparâmetros (Optuna)**: Opcionalmente, o Optuna é ativado para conduzir múltiplos *trials* de treinamento, buscando a melhor combinação de hiperparâmetros. Cada *trial* é rastreado como um experimento separado ou como uma *run* aninhada no MLflow/WandB.
5.  **Registro e Versionamento de Modelos (MLflow)**: Ao final de cada treinamento (ou *trial* do Optuna), o modelo treinado (`.pt`), seus parâmetros, métricas e o código-fonte são registrados no MLflow, versionando efetivamente cada modelo.
6.  **Armazenamento de Artefatos**: Os artefatos do modelo (como o arquivo `.pt`) são armazenados no Minio (localmente) ou no S3 (em cluster), conforme configurado no MLflow.

#### 3.4. Fluxo de Avaliação e Reimplantação de Modelos (03-model-redeploy.ipynb)
1.  **Recuperação de Modelo**: O pesquisador usa o notebook `03-model-redeploy.ipynb` para carregar um modelo específico registrado no MLflow (identificado por ID de *run* ou versão do modelo).
2.  **Avaliação Detalhada**: O modelo é avaliado contra os splits de teste (`dataset/test_freeze`, `dataset/test_accumulative`, `dataset/test_drift`) para verificar sua performance em diferentes cenários de dados.
3.  **Inferência e Visualização**: Exemplos de inferência são gerados e visualizados para entender o comportamento do modelo em diferentes imagens.
4.  **Preparação para Implantação**: O notebook pode conter passos para exportar o modelo para formatos otimizados para implantação.

### 4. Sobre o Código e Implementação

#### 4.1. Linguagens e Tecnologias
*   **Linguagem Principal**: Python 3.14 (gerenciado por `.python-version` e `uv`).
*   **Ferramentas Principais**:
    *   **Git**: Para controle de versão do código.
    *   **DVC (Data Version Control)**: Para versionamento de dados de entrada (`data/`) e metadados de splits (`dataset/dataset_info.csv`).
    *   **uv**: Gerenciador de pacotes e ambiente virtual Python, garantindo instalações rápidas e reprodutíveis.
    *   **Jupyter Lab/Notebook**: Ambiente principal para execução dos notebooks (`.ipynb`).
    *   **Pandas, PyYAML, Scikit-learn**: Bibliotecas Python essenciais para manipulação de dados e operações auxiliares.
    *   **Weights & Biases (WandB)**: Plataforma para rastreamento de experimentos e visualização de métricas.
    *   **MLflow**: Plataforma para gerenciamento do ciclo de vida de ML (rastreamento de experimentos, registro de modelos, *model serving*).
    *   **Optuna**: Biblioteca para otimização de hiperparâmetros.
    *   **YOLO (Ultralytics)**: Framework de *deep learning* para detecção de objetos (modelos `.pt`).
    *   **Docker e Docker Compose**: Para orquestração de serviços locais (MLflow, PostgreSQL, Minio).

#### 4.2. Estrutura de Código e Arquivos Chave

A organização do repositório reflete o fluxo de pesquisa e as melhores práticas de MLOps.
*   **`.dvc/`**: Contém as configurações específicas do DVC para o repositório, incluindo o link para o remoto que armazena os datasets.
*   **`.env`**: Armazena variáveis de ambiente sensíveis (como credenciais de AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY e configurações de banco de dados PostgreSQL) para uso local com Docker Compose. **Este arquivo não deve ser versionado no Git.**
*   **`.git/`**: O repositório Git, rastreando todos os arquivos de código e configuração, exceto os ignorados pelo `.gitignore`.
*   **`.gitignore`**: Define explicitamente os arquivos e diretórios que devem ser ignorados pelo Git (e.g., `data/`, `runs/`, `wandb/`, `*.pt`, `*.env`, `*.db`).
*   **`.python-version`**: Garante a consistência da versão Python (3.14) em diferentes ambientes de desenvolvimento.
*   **`.venv/`**: O ambiente virtual Python criado por `uv`, contendo todas as dependências do projeto.
*   **Notebooks (00-*, 01-*, 02-*, 03-*)**: São os principais executáveis do projeto, guiando o pesquisador pelas etapas do pipeline de ML.
*   **`analysis/`**: Guarda os resultados estáticos da análise exploratória de dados (gráficos, tabelas).
*   **`data/`**: Contém os dados de entrada, os quais são puxados via DVC e tipicamente representados por ponteiros DVC.
*   **`data.dvc`**: O arquivo de metadados do DVC que aponta para o dataset em `data/` no repositório de gerenciamento de dados.
*   **`dataset/`**: Onde os dados são efetivamente preparados e divididos em *splits* para treinamento, validação e teste. Contém também o `dataset_info.csv` e as configurações YOLO dos *splits*.
*   **`dataset/dataset_info.csv.dvc`**: Rastreia o arquivo `dataset_info.csv` via DVC, garantindo o versionamento dos metadados dos *splits*.
*   **`docker-compose.yaml`**: Configuração para subir os serviços locais de MLOps (MLflow, PostgreSQL, Minio/S3-Ninja).
*   **`mlflow/`**: Contém arquivos relacionados à configuração do serviço MLflow, como seu `Dockerfile`.
*   **`pyproject.toml`**: Lista as dependências do projeto Python e metadados, incluindo `dvc[s3]`, `pandas`, `pyyaml`, `scikit-learn`.
*   **`runs/`**: Diretório onde o YOLO salva seus *logs* de treinamento e *checkpoints* temporários.
*   **`uv.lock`**: Garante a reprodutibilidade exata do ambiente Python, especificando as versões e *hashes* de todas as dependências.
*   **`wandb/`**: O diretório de cache e *logs* gerado pelo WandB.
*   **`yolo*.pt`**: Arquivos de pesos pré-treinados ou modelos YOLO específicos.
*   **`yolo_hyperparameter_optimization.db`**: O banco de dados SQLite usado pelo Optuna para armazenar os resultados dos *trials* de otimização de hiperparâmetros.

#### 4.3. Estratégia de Comentários e Documentação
*   **Docstrings Python**: Para funções e classes Python nos notebooks ou em scripts auxiliares, serão utilizadas *docstrings* claras (preferencialmente no formato Google Style) explicando propósito, argumentos e retorno.
*   **Comentários Inline**: Serão usados comentários inline para explicar seções de código complexas ou decisões de implementação.
*   **Markdown em Notebooks**: Os notebooks `.ipynb` serão amplamente documentados com células Markdown explicando cada etapa, o contexto e os resultados esperados.
*   **README Principal**: O `README.md` serve como o ponto de entrada principal, fornecendo uma visão geral do projeto, instruções de configuração e links para a documentação mais detalhada.
*   **Comentários em Arquivos de Configuração**: Arquivos como `docker-compose.yaml`, `.dvc/config` e `pyproject.toml` contêm comentários para explicar a finalidade de suas configurações.

#### 4.4. Diretivas de Execução e Dependências
*   **Gerenciamento de Dependências (uv)**: A instalação de todas as dependências Python é realizada via `uv sync` na raiz do projeto. O ambiente virtual `.venv` é criado e ativado automaticamente, garantindo isolamento e reprodutibilidade.
*   **Inicialização de Serviços Locais (Docker Compose)**: Os serviços de MLflow (servidor), PostgreSQL (backend do MLflow) e Minio (artifact store do MLflow) são iniciados localmente com `docker-compose up -d`. As credenciais são lidas do arquivo `.env`.
*   **Acesso a Dados (DVC)**: Os dados de entrada são acessados através de `dvc pull` no diretório `data/`, que puxa os dados do remoto S3 configurado no `.dvc/config`.
*   **Execução de Notebooks**: Os workflows são executados sequencialmente através dos notebooks Jupyter, que devem ser abertos e executados no ambiente virtual ativado.
*   **Configuração de Remoto DVC**: O remoto S3 para o DVC é configurado via `.dvc/config` para apontar para o `s3://data-labeling-s3` no endpoint local do S3-Ninja (`http://localhost:9444/`).
*   **Configuração de MLflow**: O MLflow usa o PostgreSQL como *backend store* e o Minio como *artifact store*. Estas configurações são definidas no `docker-compose.yaml` e no `.env`.

#### 4.5. Boas Práticas Implementadas
1.  **Reprodutibilidade End-to-End**: Versionamento de código (Git), dados (DVC), dependências (uv.lock) e experimentos (WandB/MLflow) garantem a reprodutibilidade completa de qualquer resultado.
2.  **Modularidade e Clareza**: O fluxo de trabalho é dividido em notebooks sequenciais, cada um com uma responsabilidade clara, facilitando a compreensão e manutenção.
3.  **Rastreabilidade Automática**: A integração com WandB e MLflow automatiza o registro de métricas, parâmetros e modelos, minimizando a intervenção manual e erros.
4.  **Ambiente Consistente**: O uso de `.python-version` e `uv.lock` garante que todos os colaboradores trabalhem com o mesmo ambiente de software.
5.  **Flexibilidade de Infraestrutura**: Suporte para desenvolvimento local (Docker Compose) e implantação em cluster Kubernetes para os serviços de MLOps.

#### 4.6. Limitações Conhecidas e Considerações
*   **Gestão de Versões de Modelos YOLO**: Embora o MLflow versiona os modelos treinados, o gerenciamento de diferentes versões das bibliotecas YOLO e suas compatibilidades requer atenção manual do pesquisador.
*   **Recursos Computacionais Locais**: Treinamentos de modelos YOLO e otimização de hiperparâmetros são intensivos em recursos. A execução local via Docker Compose pode ser lenta ou inviável para datasets muito grandes ou arquiteturas complexas.
*   **Proveniência de Dados no DVC**: O `data.dvc` aponta para um snapshot do dataset. Embora reprodutível, a proveniência exata de como aquele dataset foi *gerado* (transformações aplicadas, scripts) não é explicitamente rastreada *neste* repositório, mas sim no repositório de gerenciamento de dados.
*   **Complexidade de Configuração Inicial**: A configuração inicial do ambiente local, incluindo Docker, `uv`, DVC, e as variáveis de ambiente para MLflow e Minio, pode apresentar uma curva de aprendizado para novos usuários.
*   **Segurança de Credenciais Locais**: O uso do arquivo `.env` para credenciais locais é adequado para desenvolvimento, mas requer cuidado para não ser commitado no Git e garantir que as credenciais sejam apropriadas para um ambiente de teste.

## Manual de Utilização para Usuários Contemplados

Este manual foi desenvolvido para guiar cientistas e engenheiros de dados na utilização eficaz do repositório de pesquisa, que combina Git para código, DVC para dados, e ferramentas MLOps como WandB, MLflow e Optuna para o gerenciamento de experimentos e modelos. Ele é consistente com a documentação técnica e arquitetural do projeto.

## 1. Configurando o Ambiente de Pesquisa Localmente

Esta tarefa descreve como clonar o repositório, preparar seu ambiente Python e iniciar os serviços MLOps locais (MLflow, PostgreSQL, Minio) usando Docker Compose.

### Guia de Instruções:

Para **configurar e preparar seu ambiente de pesquisa local** faça:

1.  **Clone o repositório Git do projeto:**
    ```bash
    git clone <URL_DO_REPOSITORIO_DE_PESQUISA>
    cd <nome_do_diretorio_do_projeto>
    ```
2.  **Prepare o ambiente Python e instale as dependências:**
    *   Certifique-se de ter o `uv` instalado. Se não, instale-o com `pip install uv`.
    *   Instale as dependências do projeto e ative o ambiente virtual:
        ```bash
        uv sync
        uv shell # Para ativar o ambiente na sessão atual
        ```
    *   *Alternativa (se uv não for usado):* Crie e ative um ambiente virtual Python com `python3.14 -m venv .venv` e `source .venv/bin/activate`. Em seguida, instale as dependências de `pyproject.toml` com `pip install -e ".[s3]"`.
3.  **Configure as variáveis de ambiente locais:**
    *   Crie um arquivo `.env` na raiz do projeto (se não existir) com as seguintes informações:
        ```
        # Credenciais Minio/AWS S3 para MLflow artifacts e DVC
        AWS_ACCESS_KEY_ID=minioadmin
        AWS_SECRET_ACCESS_KEY=minioadmin

        # Configurações do PostgreSQL para MLflow backend
        POSTGRES_USER=mlflow_user
        POSTGRES_PASSWORD=mlflow_password
        POSTGRES_DATABASE=mlflow_db
        ```
    *   *Importante:* O `AWS_ACCESS_KEY_ID` e `AWS_SECRET_ACCESS_KEY` aqui são para o S3-Ninja/Minio. Em produção com S3 real, seriam suas credenciais AWS. Este arquivo `.env` é ignorado pelo Git para segurança.
4.  **Inicie os serviços MLOps locais (MLflow, PostgreSQL, Minio) via Docker Compose:**
    *   Certifique-se de ter o Docker e Docker Compose instalados e em execução.
    ```bash
    docker-compose up -d
    ```
    Este comando iniciará o servidor MLflow (disponível em `http://localhost:5000`), um banco de dados PostgreSQL e um serviço Minio (compatível com S3) para armazenar os artefatos do MLflow. O DVC também usará o Minio/S3-Ninja para puxar os dados.


### Exceções ou potenciais problemas:

*   **Se `git clone` falhar:**
    *   É porque: A URL do repositório está incorreta ou você não tem permissão de acesso.
    *   Então faça: Verifique a URL e suas credenciais de acesso ao Git.
*   **Se `uv sync` ou `uv shell` falharem:**
    *   É porque: O `uv` não está instalado, ou há problemas nas dependências em `pyproject.toml`/`uv.lock`.
    *   Então faça: Instale o `uv` se necessário (`pip install uv`) ou verifique a integridade dos arquivos de dependência.
*   **Se `docker-compose up -d` falhar ou os serviços não iniciarem:**
    *   É porque: O Docker não está em execução, o Docker Compose não está instalado, ou há conflitos de portas, ou problemas de dependência entre os contêineres.
    *   Então faça: Inicie o Docker. Verifique os logs dos contêineres com `docker-compose logs`. Resolva conflitos de porta (ex: se a porta 5000 do MLflow já estiver em uso). Certifique-se de que o arquivo `.env` está formatado corretamente.
*   **Se MLflow ou DVC não conseguirem se conectar ao Minio/S3-Ninja:**
    *   É porque: Pode haver um problema de rede entre os contêineres Docker e sua máquina host (onde o DVC está sendo executado). O `http://host.docker.internal` é específico para Docker Desktop em Windows/macOS.
    *   Então faça: No `.dvc/config` e `docker-compose.yaml` para Linux, você pode precisar substituir `host.docker.internal` pelo IP da sua máquina host ou usar a rede `bridge` do Docker e o nome do serviço (e.g., `s3-ninja:9000`).

## 2. Puxando e Preparando os Dados para Pesquisa

Esta tarefa guia o pesquisador a obter a versão mais recente ou específica dos dados anotados e prepará-los para o treinamento do modelo.

### Guia de Instruções:

Para **puxar a versão mais recente dos dados e prepará-los para uso** faça:

1.  **Navegue até o diretório raiz do projeto no terminal.**
2.  **Puxe a versão mais atual dos dados brutos com DVC:**
    ```bash
    dvc pull data/
    ```
    Isso baixará o dataset mais recente referenciado pelo `data.dvc` para a pasta `data/` do seu projeto.
    *   *Alternativa (para puxar uma versão específica):* Se você precisa de uma versão anterior dos dados, primeiro faça o `git checkout` para o commit que contém a versão desejada do `data.dvc`, e então execute `dvc pull data/`.
3.  **Abra o notebook `00-data_ingestion.ipynb` no Jupyter Lab/Notebook.**
4.  **Execute todas as células do notebook sequencialmente.**
    *   Este notebook irá processar os dados em `data/`, criar a estrutura de diretórios `dataset/` com os splits (`train`, `valid`, `test_freeze`, `test_accumulative`, `test_drift`), e gerar o `dataset_info.csv` e os arquivos de configuração YOLO (`data_freeze.yaml`, etc.).
    *   Você pode ajustar parâmetros como `train_ratio`, `valid_ratio` ou `n_drift` na célula de execução principal do notebook.
5.  **Versionamento dos Metadados do Dataset:**
    *   Após a execução do notebook, o arquivo `dataset/dataset_info.csv` (e potencialmente outros `.yaml`s) será atualizado.
    *   Rastreie essas mudanças com DVC e Git:
        ```bash
        dvc add dataset/dataset_info.csv
        git add dataset/dataset_info.csv.dvc
        git commit -m "Atualiza splits de dados e metadados apos ingestão"
        ```
    *   *Opcional:* Se você deseja que outros colaboradores tenham acesso a esses metadados de splits, empurre-os para o remoto DVC e Git:
        ```bash
        dvc push dataset/dataset_info.csv.dvc
        git push origin <seu_branch>
        ```


### Exceções ou potenciais problemas:

*   **Se `dvc pull data/` falhar:**
    *   É porque: O remoto S3 (Minio local ou S3 real) não está acessível ou o DVC não está configurado corretamente.
    *   Então faça: Verifique se o serviço Minio está em execução (`docker-compose ps`) e se o `.dvc/config` aponta para o `endpointurl` correto.
*   **Se o notebook `00-data_ingestion.ipynb` falhar:**
    *   É porque: Faltam bibliotecas Python (certifique-se de que o ambiente `uv` está ativado), os dados em `data/` estão corrompidos/incompletos, ou há erros na lógica do script (verifique o traceback).
    *   Então faça: Verifique se `uv shell` foi executado. Inspecione a pasta `data/` para garantir que as imagens e rótulos esperados estão lá. Depure as mensagens de erro no notebook.
*   **Se `dvc add dataset/dataset_info.csv` falhar:**
    *   É porque: Você pode ter esquecido de executar o notebook `00-data_ingestion.ipynb`, ou o arquivo `dataset_info.csv` não foi gerado/atualizado.
    *   Então faça: Execute o notebook e verifique a existência do arquivo.

## 3. Realizando Análise Exploratória de Dados (EDA)

Esta tarefa permite que os pesquisadores entendam a composição do dataset e identifiquem potenciais vieses ou problemas.

### Guia de Instruções:

Para **realizar a Análise Exploratória de Dados (EDA) dos splits de dados** faça:

1.  **Abra o notebook `01-data_analysis.ipynb` no Jupyter Lab/Notebook.**
2.  **Execute todas as células do notebook sequencialmente.**
    *   Este notebook carregará os dados da pasta `dataset/` (incluindo o `dataset_info.csv`).
    *   Ele gerará diversas visualizações (gráficos de distribuição de classes, análise de tamanho de bounding box, mapas de calor de distribuição espacial, etc.) e relatórios.
    *   Os resultados visuais serão salvos automaticamente na pasta `analysis/`.
3.  **Inspecione os resultados na pasta `analysis/` e no próprio notebook.**
    *   Os gráficos e tabelas ajudarão a identificar:
        *   Classes desbalanceadas.
        *   Tamanhos de objetos predominantes.
        *   Vieses espaciais (objetos sempre aparecendo na mesma região da imagem).
        *   Amostras negativas (imagens sem objetos).
4.  **Commite os resultados da análise (opcional, mas recomendado):**
    *   Embora a pasta `analysis/` seja grande, você pode querer versionar os relatórios chave ou gráficos gerados. Adicione-os ao Git se relevantes para a reprodutibilidade da análise.


### Exceções ou potenciais problemas:

*   **Se o notebook `01-data_analysis.ipynb` falhar ao carregar dados:**
    *   É porque: O notebook `00-data_ingestion.ipynb` não foi executado ou não gerou os arquivos de dados e metadados (`dataset_info.csv`) esperados na pasta `dataset/`.
    *   Então faça: Certifique-se de ter executado com sucesso o `00-data_ingestion.ipynb`.
*   **Se o notebook exibir erros relacionados a bibliotecas de visualização (e.g., Matplotlib, Seaborn):**
    *   É porque: As bibliotecas podem não ter sido instaladas corretamente no ambiente virtual.
    *   Então faça: Verifique as dependências em `pyproject.toml` e execute `uv sync` novamente para garantir que todas as bibliotecas estão instaladas.
*   **Se a pasta `analysis/` não for atualizada:**
    *   É porque: O notebook não foi executado completamente, ou há erros na lógica de salvamento dos arquivos.
    *   Então faça: Verifique os *logs* de execução do notebook e o código de salvamento de arquivos.

## 4. Treinando e Otimizando Modelos YOLO com Rastreamento de Experimentos

Esta tarefa é para pesquisadores que desejam treinar novos modelos, otimizar seus hiperparâmetros e registrar todo o processo usando WandB e MLflow.

### Guia de Instruções:

Para **treinar um modelo YOLO e rastrear seu experimento** faça:

1.  **Abra o notebook `02-model_training.ipynb` no Jupyter Lab/Notebook.**
2.  **Configurações Iniciais:**
    *   Na primeira célula, defina o nome do seu projeto WandB, o nome do experimento MLflow e configure as credenciais de API (se necessário, embora para WandB geralmente o *login* interativo seja suficiente).
    *   Especifique a arquitetura do modelo YOLO (e.g., `yolo12n.pt`), o caminho para os dados de treino/validação (apontando para os arquivos `data_freeze.yaml`, `data_drift.yaml`, `data_accumulative.yaml` em `dataset/`).
3.  **Treinamento Básico (sem otimização):**
    *   Ajuste os hiperparâmetros diretamente no notebook (e.g., `epochs`, `batch_size`, `lr`).
    *   Execute a célula de treinamento do modelo.
    *   O treinamento será automaticamente rastreado no WandB e no MLflow. Você pode acessar o painel do WandB (`wandb.ai`) para ver as métricas em tempo real e o UI do MLflow (`http://localhost:5000`) para ver o registro completo do experimento.
4.  **Otimização de Hiperparâmetros com Optuna:**
    *   Selecione a seção do notebook dedicada à otimização com Optuna.
    *   Defina o espaço de busca para os hiperparâmetros (e.g., `lr_min`, `lr_max`, `batch_sizes`) e o número de *trials*.
    *   Execute a célula de otimização. Cada *trial* do Optuna resultará em um treinamento YOLO e será registrado como uma *run* separada no MLflow e WandB, permitindo que você compare facilmente os resultados e encontre a melhor combinação de hiperparâmetros.
5.  **Após o treinamento/otimização, o modelo treinado (`.pt`) será registrado no MLflow.**
    *   Você pode acessá-lo na UI do MLflow em `http://localhost:5000`.


### Exceções ou potenciais problemas:

*   **Se o treinamento falhar ou não iniciar:**
    *   É porque: Há problemas na configuração do dataset (arquivos `.yaml` incorretos), CUDA não disponível (se estiver usando GPU), ou erros nos hiperparâmetros.
    *   Então faça: Verifique os caminhos dos arquivos `.yaml` em `dataset/`. Certifique-se de que sua instalação do PyTorch e CUDA está correta. Inspecione os logs de erro detalhados no output do notebook.
*   **Se os experimentos não aparecerem no WandB ou MLflow:**
    *   É porque: O servidor MLflow não está em execução, as credenciais WandB não estão configuradas, ou há um problema de conectividade.
    *   Então faça: Verifique se `docker-compose ps` mostra o `mlflow_server` como `Up`. Faça `wandb login` no terminal se o notebook solicitar. Verifique as configurações de `MLFLOW_TRACKING_URI` e `WANDB_API_KEY`.
*   **Se o Optuna não encontrar a melhor combinação de hiperparâmetros após muitos trials:**
    *   É porque: O espaço de busca definido é muito grande, o número de *trials* é insuficiente, ou a métrica objetivo não é sensível o suficiente.
    *   Então faça: Revise o espaço de busca, aumente o número de *trials*, ou refine a métrica objetivo.

## 5. Acessando a Interface de Usuário do MLflow Localmente

Esta tarefa permite aos pesquisadores visualizarem e gerenciarem seus experimentos, parâmetros e modelos registrados.

### Guia de Instruções:

Para **acessar a interface de usuário do MLflow** faça:

1.  **Certifique-se de que o servidor MLflow está em execução:**
    ```bash
    docker-compose ps
    ```
    Verifique se `mlflow_server` está com status `Up`. Se não estiver, inicie-o com `docker-compose up -d`.
2.  **Abra seu navegador web e acesse o endereço:**
    ```
    http://localhost:5000
    ```
3.  **Explore os experimentos:**
    *   Na interface, você poderá ver uma lista de todos os experimentos (e.g., "YOLO_Training_Experiment", "Hyperparameter_Optimization").
    *   Clique em um experimento para ver suas *runs* individuais, cada uma representando um treinamento ou *trial* do Optuna.
4.  **Inspecione os detalhes de uma *run*:**
    *   Dentro de cada *run*, você encontrará:
        *   **Parâmetros**: Hiperparâmetros usados no treinamento.
        *   **Métricas**: Valores de loss, mAP, etc. (se WandB estiver integrado, também pode haver links para o painel do WandB).
        *   **Artefatos**: Arquivos de modelo (`.pt`), gráficos, *logs* (salvos no Minio/S3).
        *   **Tags**: Informações adicionais sobre o experimento.
5.  **Navegue pelo registro de modelos:**
    *   Na barra lateral esquerda, clique em "Models" para ver os modelos versionados. Você pode registrar novas versões de modelos a partir de *runs* específicas.


### Exceções ou potenciais problemas:

*   **Se a página `http://localhost:5000` não carregar:**
    *   É porque: O contêiner `mlflow_server` não está em execução ou há um problema de rede.
    *   Então faça: Verifique o status do `docker-compose ps` e os logs do `mlflow_server` com `docker-compose logs mlflow_server`.
*   **Se o MLflow UI estiver vazio ou não mostrar os experimentos esperados:**
    *   É porque: Você não executou nenhum experimento ou os experimentos não foram registrados corretamente com o MLflow.
    *   Então faça: Execute o notebook `02-model_training.ipynb` e certifique-se de que o rastreamento do MLflow está ativo e configurado corretamente.

## 6. Recuperando Resultados de Experimentos e Modelos

Esta tarefa é para reutilizar modelos previamente treinados ou analisar resultados de experimentos antigos.

### Guia de Instruções:

Para **recuperar um modelo ou os detalhes de um experimento anterior** faça:

1.  **Acesse a interface de usuário do MLflow (`http://localhost:5000`)** (veja Tarefa 5).
2.  **Identifique o experimento e a *run* de interesse:**
    *   Navegue até o experimento e encontre a *run* que contém o modelo ou os resultados que você deseja recuperar.
    *   Anote o **Run ID** ou o **Model Name e Version** do modelo.
3.  **Recuperar parâmetros e métricas via código (em um notebook):**
    *   No notebook `03-model-redeploy.ipynb` (ou qualquer outro), use o MLflow API para carregar os detalhes de uma *run* específica:
        ```python
        import mlflow

        # Substitua pelo ID da run que você quer carregar
        run_id = "SEU_RUN_ID_AQUI" 
        client = mlflow.tracking.MlflowClient()
        run = client.get_run(run_id)

        print(f"Parâmetros: {run.data.params}")
        print(f"Métricas: {run.data.metrics}")
        ```
4.  **Carregar um modelo registrado via código:**
    *   Para carregar um modelo específico registrado no MLflow:
        ```python
        import mlflow.pyfunc

        # Opção 1: Carregar pela URI do artefato (diretamente da run)
        # model_uri = f"runs:/{run_id}/artifacts/yolo_model" # Ajuste o caminho do artefato
        # loaded_model = mlflow.pyfunc.load_model(model_uri)

        # Opção 2: Carregar por nome e versão do modelo registrado
        model_name = "YOLO_Detector_v1" # Substitua pelo nome do seu modelo
        model_version = 1               # Substitua pela versão
        loaded_model = mlflow.pyfunc.load_model(f"models:/{model_name}/{model_version}")

        # Agora 'loaded_model' é um wrapper que você pode usar para inferência
        # Ex: predictions = loaded_model.predict(input_data)
        ```
    *   *Nota:* Para modelos YOLO (`.pt`), você pode precisar carregar os pesos diretamente do caminho do artefato no MLflow e depois instanciar a classe YOLO apropriada. O `mlflow.pyfunc.load_model` é mais genérico e pode não ser ideal para modelos YOLO diretamente.

5.  **Acessar artefatos do modelo diretamente (e.g., arquivo `.pt`):**
    *   Na UI do MLflow, dentro de uma *run*, clique em "Artifacts" e depois nos arquivos do modelo. Você pode ver o caminho completo (URI) do artefato e baixá-lo ou referenciá-lo em seu código.


### Exceções ou potenciais problemas:

*   **Se `mlflow.tracking.MlflowClient()` ou `mlflow.pyfunc.load_model()` falharem:**
    *   É porque: O servidor MLflow não está em execução ou o `MLFLOW_TRACKING_URI` não está configurado corretamente no ambiente onde o código está sendo executado.
    *   Então faça: Verifique se `docker-compose ps` mostra o `mlflow_server` como `Up`. Certifique-se de que a variável de ambiente `MLFLOW_TRACKING_URI` está definida (e.g., `os.environ["MLFLOW_TRACKING_URI"] = "http://localhost:5000"` no seu notebook).
*   **Se o `run_id`, `model_name` ou `model_version` estiverem incorretos:**
    *   É porque: Você forneceu um identificador inválido para o MLflow.
    *   Então faça: Use a UI do MLflow para confirmar os IDs e nomes/versões exatos.
*   **Se o modelo carregado não funcionar como esperado:**
    *   É porque: Pode haver uma incompatibilidade de bibliotecas entre o ambiente em que o modelo foi salvo e o ambiente atual, ou o formato de entrada esperado pelo modelo não está correto.
    *   Então faça: Revise as dependências no `uv.lock` do commit onde o modelo foi salvo. Verifique a documentação do modelo sobre o formato de entrada.

---