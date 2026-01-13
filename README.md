# Sistema de Apoio à Decisão para Alocação de Docentes

Esse projeto é um trabalho de conclusão de curso para graduação em Estatística Bacharelado pela Universidade de Brasília. O relatório final está disponível em tcc_karl_peixoto.pdf.

O projeto é um sistema que aplica Meta-Heurísticas (Algoritmos Genéticos e Colônia de Formigas) e Métodos Exatos (Programação Linear Inteira - PLI) para otimizar a alocação de docentes em disciplinas, respeitando preferências dos professores e restrições de horários. Interface web simples em Flask para execução e análise dos resultados.

---

## Estrutura do Projeto

```text
projeto_alocador/
├─ ferramenta/                       # Aplicação web (Flask)
│  ├─ app.py                         # Rotas, carregamento de dados e orquestração dos otimizadores
│  ├─ modelos/                       # Algoritmos de otimização
│  │  ├─ otimizador_pli.py           # PLI (PuLP)
│  │  ├─ otimizador_aco.py           # ACO (Colônia de Formigas)
│  │  ├─ otimizador_ag.py            # AG (Algoritmo Genético)
│  │  └─ otimizador_base.py          # Base comum (preparação de dados, utilitários)
│  └─ templates/                     # Páginas HTML (Jinja2)
│     ├─ base.html
│     ├─ dados_iniciais.html
│     ├─ executar.html
│     └─ resultado.html
├─ dados/                            # Arquivos CSV padronizados consumidos pelo sistema
│  ├─ docentes.csv
│  ├─ disciplinas.csv
│  ├─ preferencias.csv
│  └─ matriz_conflitos.csv
├─ dados_iniciais/                   # Planilhas cruas da coordenação (entrada do processo)
├─ preparacao_dos_dados.ipynb        # Notebook para transformar dados crus em CSVs padronizados
├─ requirements.txt
└─ README.md
```

- "Site": pasta `ferramenta/` contém o app Flask e as views.
- "Algoritmos": pasta `ferramenta/modelos/` implementa AG, ACO e PLI.
- "Dados": pasta `dados/` guarda os CSVs que o app consome.
- "Dados Iniciais": pasta `dados_iniciais/` recebe suas planilhas originais (antes do processamento).

---

## ⚠️ Fluxo de Preparação de Dados (CRÍTICO)

> ⚠️ Atenção: o sistema **não roda direto** com seus arquivos brutos. É obrigatório preparar os dados antes de iniciar o servidor.
>
> 1. **Entrada:** Coloque suas planilhas originais na pasta `dados_iniciais/`.
> 2. **Formato:** Os arquivos devem seguir estritamente o formato dos exemplos (quando disponíveis) — caso não haja uma pasta de modelos de dados, use a estrutura de colunas esperada no notebook.
> 3. **Processamento:** Execute o notebook `preparacao_dos_dados.ipynb` (em VS Code/Jupyter) para transformar os dados.
> 4. **Saída:** O processamento gera os arquivos padronizados na pasta `dados/`: `disciplinas.csv`, `docentes.csv`, `preferencias.csv`, `matriz_conflitos.csv`.
>
> Sem esses quatro arquivos na pasta `dados/`, a aplicação não terá insumos para executar os algoritmos.

Dica: Se preferir script `.py`, exporte o notebook (`Arquivo → Exportar → Python`) e rode `python preparacao_dos_dados.py`.

---

## Instalação e Execução

### 1) Clonar o repositório

```bash
# Exemplo (ajuste conforme seu provedor)
git clone <URL-do-seu-repositorio>
cd projeto_alocador
```

### 2) Criar e ativar um ambiente virtual (Windows)

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Ou em CMD:

```cmd
python -m venv .venv
.\.venv\Scripts\activate.bat
```

### 3) Instalar dependências

```bash
pip install -r requirements.txt
```

### 4) Preparar os dados (obrigatório)

Abra e execute o notebook `preparacao_dos_dados.ipynb` até o fim para gerar os CSVs na pasta `dados/`.

### 5) Rodar a aplicação Web (Flask)

Opção A — Flask CLI (recomendado):

```powershell
$env:FLASK_APP = "ferramenta.app"
$env:FLASK_ENV = "development"
flask run
```

Ou em CMD:

```cmd
set FLASK_APP=ferramenta.app
set FLASK_ENV=development
flask run
```

- Acesse: http://127.0.0.1:5000
- Principais rotas: `/` (dados iniciais), `/executar` (rodar AG/ACO/PLI), `/resultado` (visualização), `/resultado/export` (Excel).

> Observação: o arquivo `ferramenta/app.py` expõe o objeto Flask `app`. Por isso o `FLASK_APP` deve ser `ferramenta.app`.

---

## Autor e Contato

- Nome: Karl Peixoto
- LinkedIn: https://www.linkedin.com/in/karl-peixoto-71787537/
- E-mail: karlpmm@gmail.com

---

### Notas Finais

- Mantenha os arquivos em `dados/` sempre atualizados com base no fluxo de preparação.