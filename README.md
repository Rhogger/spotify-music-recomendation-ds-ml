# Spotify Music Recommendation System 🎵

Sistema inteligente de recomendação de músicas baseado em características de áudio e metadados do Spotify.

## 📝 Sobre o Projeto

Este projeto utiliza técnicas de Machine Learning para analisar características de faixas de áudio (como acousticness, danceability, energy e valence) e criar um motor de recomendação que encontra músicas similares.

**Desenvolvido como parte do curso de pós-graduação da UNIRV.**

### O que o projeto faz

1. Carrega dados brutos de 160k músicas do Spotify
2. Limpa e padroniza os dados
3. Realiza análise exploratória para entender padrões e tomadas de decisão
4. Normaliza features de áudio usando StandardScaler
5. Aplica one-hot encoding para características temporais (décadas)
6. Treina modelo KNN para encontrar vizinhos mais próximos
7. Exporta modelos e pipelines para uso no front-end, outro projeto encontrado no seguinte [repositório no github](https://github.com/Rhogger/spotify-music-recomendation-frontend)

## 📋 Pré-requisitos

- Python 3.12 ou superior
- Git
- Visual Studio Code
- Pipenv ou venv (para gerenciamento de dependências)

## 🔧 Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/Rhogger/spotify-music-recomendation-ds-ml.git
cd spotify-music-recomendation-ds-ml
```

### 2. Configure o ambiente virtual

Escolha uma das opções abaixo:

#### Opção A: Usando Pipenv (Recomendado)

```bash
# Instale o Pipenv (se não tiver instalado)
pip install pipenv

# Instale as dependências
pipenv install

# Ative o ambiente virtual
pipenv shell
```

#### Opção B: Usando venv

```bash
# Crie o ambiente virtual
python -m venv venv

# Ative o ambiente virtual
# Linux/Mac:
source venv/bin/activate
# Windows:
# venv\Scripts\activate

# Instale as dependências
pip install -r requirements.txt
```

## 📁 Estrutura do Projeto

```text
spotify-music-recomendation/
├── src/                     # Código fonte principal
│   ├── datasets/           # Datasets processados
│   ├── models/             # Modelos treinados e pipelines
│   └── notebooks/          # Jupyter notebooks de análise
├── .vscode/                # Configurações do VS Code
├── .gitattributes          # Configuração para diffs do Git
├── .gitignore              # Arquivos ignorados pelo Git
├── Pipfile                 # Dependências do projeto
├── Pipfile.lock            # Lock das dependências
├── requirements.txt        # Dependências (alternativa ao Pipfile)
├── ruff.toml              # Configuração do Ruff
├── DEV.md                 # Guia de desenvolvimento
└── README.md              # Este arquivo
```

## 👨‍💻 Desenvolvimento

Para mais detalhes sobre o setup de desenvolvimento, configuração do workspace e extensões recomendadas, consulte o documento [DEV.md](./DEV.md).

Lá você encontrará:

- **Configurações do Workspace** - Setup recomendado para VS Code com extensões essenciais para Python, Jupyter notebooks e análise de dados
- **Extensões Recomendadas** - Lista curada de extensões para melhorar sua produtividade (Pylance, Jupyter, Python, etc.)
- **Padrões de Código** - Guia de estilo e formatação usando Ruff

## 🔀 Fluxo do Projeto

O projeto segue um pipeline de análise em etapas:

1. **Data Cleaning** (`data_clean.ipynb`) - Limpeza e padronização dos dados brutos do Spotify
2. **EDA** (`eda.ipynb`) - Análise exploratória para entender as características das músicas
3. **Pre-processing** (`pre_processing.ipynb`) - Normalização, binarização e feature engineering
4. **Modeling** (`model.ipynb`) - Treinamento do modelo KNN e avaliação

Todos os datasets processados ficam em `src/datasets/` e os modelos treinados em `src/models/`.

## ✨ Tecnologias Utilizadas

- **Python 3.12** - Linguagem principal
- **Pandas & NumPy** - Manipulação e computação científica
- **Scikit-learn** - Machine Learning (StandardScaler, NearestNeighbors, train_test_split)
- **Matplotlib, Seaborn & Gradio** - Visualização de dados
- **Jupyter Notebooks** - Análise interativa e exploratória
- **Joblib** - Serialização de modelos e pipelines
- **Kaggle Hub** - Download automatizado de datasets
- **Ruff** - Linting e formatação de código

## 📊 Dataset

**Fonte:** [160k Spotify Songs Sorted](https://www.kaggle.com/datasets/fcpercival/160k-spotify-songs-sorted)

**Features utilizadas no modelo:**

- `acousticness` - Confiança (0-1) de que a faixa é acústica
- `danceability` - Quão dançante a faixa é (0-1)
- `energy` - Intensidade e atividade da faixa (0-1)
- `valence` - Positividade musical transmitida (0-1)
- `popularity` - Score de popularidade (0-100) → binarizado em `is_popular`
- `explicit` - Binário que diz se a música possui palavrões
- `year` - Ano de lançamento → convertido em features de décadas (1920s-2020s)

**Tamanho final:** ~170k músicas após limpeza

## 🤖 Modelo de Recomendação

### Arquitetura

O sistema utiliza **K-Nearest Neighbors (KNN)** como algoritmo base para encontrar músicas similares.

### Entrada (Input)

O modelo recebe como entrada um vetor de **11 features** para cada música:

| Feature | Tipo | Escala | Descrição |
|---------|------|--------|-----------|
| `acousticness` | Normalizado | [0, 1] | StandardScaler aplicado |
| `danceability` | Normalizado | [0, 1] | StandardScaler aplicado |
| `energy` | Normalizado | [0, 1] | StandardScaler aplicado |
| `valence` | Normalizado | [0, 1] | StandardScaler aplicado |
| `is_popular` | Binária | {0, 1} | 0: ≤33, 1: >33 de popularity |
| `explicit` | Binária | {0, 1} | Origianl do dataset |
| `1920s` - `2020s` | One-Hot | {0, 1} | 6 features de décadas |

**Total de features de entrada:** 11

### Processamento

1. **Normalização** - As 4 features de áudio são normalizadas usando `StandardScaler` (média=0, std=1)
2. **Binarização** - Popularity é convertida em classe binária (popular/não popular) e explicit é original do dataset
3. **One-Hot Encoding** - Décadas são codificadas em 6 features binárias (1920s, 1930s, ..., 2020s)
4. **Split** - Dataset dividido em 70% treino e 30% teste (random_state=42)

### Parâmetros do Modelo KNN

```python
NearestNeighbors(
    n_neighbors=20,           # Encontra 20 vizinhos mais próximos
    metric='euclidean',       # Distância euclidiana
    algorithm='kd_tree'       # Estrutura de dados otimizada
)
```

### Saída (Output)

O modelo retorna, para cada música de entrada:

- **20 índices** das músicas mais similares (ordenadas por proximidade)
- **20 distâncias euclidianas** correspondentes (quanto menor, mais similar)

### Métricas de Desempenho

O modelo é avaliado com base em:

- **Distância euclidiana média** - Proximidade geral dos vizinhos
- **Distribuição de distâncias** - Min, max, desvio padrão
- **Qualidade de recomendação** - Vizinhos com distâncias baixas indicam boas recomendações

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---
