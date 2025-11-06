## 🚀 Configuração da Workspace no VS Code

### 1. Extensões Recomendadas

Para uma melhor experiência de desenvolvimento, instale as seguintes extensões no VS Code:

#### Essenciais para Data Science

- **Jupyter** (`ms-toolsai.jupyter`) - Suporte para notebooks Jupyter
- **Python** (`ms-python.python`) - Suporte completo para Python
- **Data Wrangler** (`ms-toolsai.datawrangler`) - Ferramenta visual para limpeza de dados

#### Qualidade de Código

- **Ruff** (`charliermarsh.ruff`) - Linter e formatter Python ultrarrápido

#### Produtividade

- **Excel Viewer** (`GrapeCity.gc-excelviewer`) - Visualizador de arquivos CSV/Excel
- **Indent Rainbow** (`oderwat.indent-rainbow`) - Colorização de indentação
- **Path Intellisense** (`christian-kohler.path-intellisense`) - Autocomplete para caminhos
- **Preview** (`searKing.preview-vscode`) - Preview de arquivos markdown e outros

### 2. Instalação das Extensões

#### Opção 1: Via Command Palette (Recomendado)

1. Abra o VS Code
2. Pressione `Ctrl+Shift+P` (Linux/Windows) ou `Cmd+Shift+P` (Mac)
3. Digite `Extensions: Install Extension`
4. Pesquise e instale cada extensão pelos IDs listados acima

#### Opção 2: Via Terminal

Execute o seguinte comando no terminal:

```bash
# Instalar todas as extensões de uma vez
code --install-extension ms-toolsai.jupyter \
     --install-extension charliermarsh.ruff \
     --install-extension ms-toolsai.datawrangler \
     --install-extension GrapeCity.gc-excelviewer \
     --install-extension oderwat.indent-rainbow \
     --install-extension christian-kohler.path-intellisense \
     --install-extension searKing.preview-vscode \
     --install-extension ms-python.python
```

### 3. Abrindo a Workspace

#### Opção 1: Via Terminal (Recomendado)

```bash
# No diretório do projeto
code .
```

#### Opção 2: Via VS Code

1. Abra o VS Code
2. Vá em `File > Open Folder...`
3. Navegue até a pasta do projeto e selecione-a
4. Clique em "Select Folder"

#### Opção 3: Via Workspace File

```bash
# Se existir um arquivo .code-workspace
code spotify-recommendation.code-workspace
```

### 4. Configuração do Interpretador Python

Após abrir o projeto:

1. Pressione `Ctrl+Shift+P`
2. Digite `Python: Select Interpreter`
3. Selecione o interpretador do ambiente virtual:

**Se usando Pipenv:**

- Geralmente localizado em: `~/.local/share/virtualenvs/spotify-*/bin/python`

**Se usando venv:**

- Localizado em: `./venv/bin/python` (Linux/Mac) ou `.\venv\Scripts\python.exe` (Windows)

## 🛠️ Desenvolvimento

### Executar Linting

**Com Pipenv:**

```bash
pipenv run ruff check .
```

**Com venv:**

```bash
# Certifique-se de que o ambiente está ativado
ruff check .
```

### Formatar Código

**Com Pipenv:**

```bash
pipenv run ruff format .
```

**Com venv:**

```bash
# Certifique-se de que o ambiente está ativado
ruff format .
```
