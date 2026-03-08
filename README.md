# 📊 Python Insights — Análise de Cancelamento de Clientes

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-5.x-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Concluído-brightgreen?style=for-the-badge)

---

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como parte de um case real de análise de dados para uma empresa com mais de **800 mil clientes**. O objetivo principal foi identificar os **principais motivos de cancelamento** do serviço e propor **ações estratégicas** para reduzir o churn (taxa de cancelamento).

A empresa identificou que a maioria da sua base de clientes era composta por **clientes inativos** — aqueles que já haviam cancelado o serviço — e precisava urgentemente entender as causas desse cenário para melhorar seus resultados.

---

## 🎯 Objetivos

- ✅ Analisar a base de dados de clientes para entender o perfil dos cancelamentos
- ✅ Identificar os principais fatores que levam ao cancelamento
- ✅ Propor ações concretas e mensuráveis para reduzir a taxa de churn
- ✅ Visualizar os dados de forma intuitiva para facilitar a tomada de decisão

---

## 🗂️ Estrutura do Projeto

```
📦 python-insights-cancelamentos/
├── 📄 cancelamentos.csv          # Base de dados principal
├── 📓 analise_cancelamentos.ipynb # Notebook Jupyter com toda a análise
└── 📄 README.md                  # Documentação do projeto
```

---

## 🛠️ Tecnologias e Ferramentas

| Ferramenta           | Finalidade                             |
| -------------------- | -------------------------------------- |
| **Python 3.10+**     | Linguagem principal do projeto         |
| **Pandas**           | Manipulação e análise de dados         |
| **Plotly Express**   | Criação de gráficos interativos        |
| **Jupyter Notebook** | Ambiente de desenvolvimento interativo |
| **OpenPyXL**         | Suporte a arquivos Excel (dependência) |

---

## ⚙️ Instalação e Configuração

### Pré-requisitos

- Python 3.10 ou superior instalado
- Extensão Jupyter instalada no VS Code (recomendado) ou Jupyter Lab

### Instalando as dependências

```bash
pip install pandas openpyxl nbformat ipykernel plotly
```

### Clonando o repositório

```bash
git clone https://github.com/Mraimundo/data-analysis-python.git
cd data-analysis
```

### Executando o projeto

Abra o arquivo `analise_cancelamentos.ipynb` no Jupyter Notebook ou VS Code e execute as células sequencialmente.

---

## 🔍 Metodologia — Passo a Passo

### Passo 1 — Importação da Base de Dados

Carregamento do dataset no formato `.csv` utilizando a biblioteca **Pandas**:

```python
import pandas as pd

tabela = pd.read_csv("cancelamentos.csv")
```

---

### Passo 2 — Visualização e Limpeza Inicial

Antes de qualquer análise, foi necessário explorar os dados disponíveis e remover informações irrelevantes. A coluna `CustomerID` foi eliminada por não agregar valor analítico:

```python
tabela = tabela.drop(columns="CustomerID")
display(tabela)
```

---

### Passo 3 — Tratamento de Dados

Verificação de tipos de dados incorretos e remoção de registros com **valores nulos (NaN)**:

```python
display(tabela.info())

tabela = tabela.dropna()

display(tabela.info())
```

> **Por que remover nulos?** Valores ausentes podem distorcer análises estatísticas e gerar conclusões incorretas sobre os padrões de cancelamento.

---

### Passo 4 — Análise Inicial: Panorama dos Cancelamentos

Contagem absoluta e percentual de clientes que cancelaram o serviço:

```python
# Contagem absoluta
display(tabela["cancelou"].value_counts())

# Distribuição percentual
display(tabela["cancelou"].value_counts(normalize=True))
```

> 💡 **Descoberta:** A maioria da base era composta por clientes que já haviam cancelado — representando **56% do total** de clientes.

---

### Passo 5 — Análise Detalhada: Identificando as Causas

Geração de **histogramas interativos** com Plotly para cada variável da base, segmentados por status de cancelamento (`cancelou = 0` ou `cancelou = 1`):

```python
import plotly.express as px

for coluna in tabela.columns:
    grafico = px.histogram(tabela, x=coluna, color="cancelou", text_auto=True)
    grafico.show()
```

---

## 📈 Principais Insights Encontrados

A análise gráfica revelou três padrões críticos que explicam a maioria dos cancelamentos:

### 🔴 Insight 1 — Tipo de Contrato

> **100% dos clientes com contrato mensal cancelaram o serviço.**

Clientes com contratos de curta duração (mensais) têm muito menos comprometimento com a plataforma e cancelam com muito mais facilidade.

**Ação recomendada:** Oferecer descontos e incentivos para migração para contratos **trimestrais ou anuais**.

---

### 🔴 Insight 2 — Ligações ao Call Center

> **Todos os clientes que ligaram mais de 4 vezes ao call center cancelaram.**

Um alto volume de contatos indica que problemas não estão sendo resolvidos adequadamente, gerando frustração e consequente cancelamento.

**Ação recomendada:** Implementar um **alerta interno** ao atingir a 3ª ligação do mesmo cliente, priorizando atendimento imediato e resolução definitiva.

---

### 🔴 Insight 3 — Atraso no Pagamento

> **Clientes com mais de 20 dias de atraso no pagamento invariavelmente cancelaram.**

O atraso prolongado é tanto um sintoma de desengajamento quanto um gatilho para cancelamento.

**Ação recomendada:** Acionar régua de cobrança e equipe de relacionamento quando o atraso atingir **15 dias**, antes que se torne crítico.

---

## ✅ Resultado das Ações Propostas

Após aplicar os filtros correspondentes às três ações corretivas na base de dados:

```python
# Excluir clientes com contrato mensal
condicao = tabela["duracao_contrato"] != "Monthly"
tabela = tabela[condicao]

# Manter clientes com até 4 ligações ao call center
condicao = tabela["ligacoes_callcenter"] <= 4
tabela = tabela[condicao]

# Manter clientes com até 20 dias de atraso
condicao = tabela["dias_atraso"] <= 20
tabela = tabela[condicao]

display(tabela["cancelou"].value_counts(normalize=True))
```

### Comparativo de Resultados

| Cenário                     | Taxa de Cancelamento      |
| --------------------------- | ------------------------- |
| **Antes das ações**         | 56%                       |
| **Após as ações propostas** | 18%                       |
| **Redução absoluta**        | **38 pontos percentuais** |

> 🎯 Implementando as três ações estratégicas identificadas, a empresa seria capaz de **reduzir o cancelamento de 56% para apenas 18%** — uma melhoria de **~68% na taxa de churn**.

---

## 💡 Conclusões e Recomendações Estratégicas

| Problema Identificado      | Gatilho de Alerta     | Ação Recomendada                   |
| -------------------------- | --------------------- | ---------------------------------- |
| Contrato mensal            | Novo cadastro mensal  | Oferecer upgrade com desconto      |
| Muitas ligações ao suporte | 3ª ligação do cliente | Escalonamento prioritário          |
| Atraso no pagamento        | 15 dias de atraso     | Contato proativo de relacionamento |

---

## 👤 Autor

Desenvolvido como parte do programa **Python Insights** — análise de dados aplicada a problemas reais de negócio.

---

## 📄 Licença

Este projeto está sob a licença MIT. Consulte o arquivo `LICENSE` para mais detalhes.
