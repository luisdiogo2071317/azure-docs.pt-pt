---
title: Analisar dados com o bloco de notas do Jupyter e Kqlmagic
description: Este tópico mostra-lhe como analisar dados com o bloco de notas do Jupyter e KQLmagic
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 7152b1d09a11d5860d52b5f73ae601422bd0f722
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654499"
---
# <a name="analyze-data-using-jupyter-notebook-and-kqlmagic"></a>Analisar dados com o bloco de notas do Jupyter e Kqlmagic
O bloco de notas do Jupyter é uma aplicação web de código-fonte aberto que permite-lhe criar e partilhar documentos contendo código, equações, visualizações e texto narrativo. Utilização inclui a limpeza de dados e transformação, simulação numérica, modelação estatística, visualização de dados e aprendizagem automática.
[O bloco de notas do Jupyter](https://jupyter.org/) suporta funções de mágicas que expandem as capacidades do kernel ao suportar os comandos adicionais. Kqlmagic é uma função que estende os recursos do kernel do Python no bloco de notas do Jupyter para que possa executar consultas de linguagem de consulta de Kusto nativamente. Pode facilmente combinar Python e Kusto linguagem de consulta para consultar e visualizar dados utilizando rich Plot.ly biblioteca integrada com `render` comandos. Origens de dados para a execução de consultas são suportadas. Estas origens de dados incluem o Explorador de dados do Azure, um serviço de exploração de dados rápida e altamente escalável para dados de telemetria e de registo, bem como o Log Analytics e Application Insights.

## <a name="prerequisites"></a>Pré-requisitos
- Conta de e-mail organizacional que seja membro do Azure Active Directory (AAD).
- O bloco de notas do Jupyter instalado no seu computador local ou utilizar blocos de notas do Azure e clonar o exemplo [bloco de notas do Azure](https://kustomagicsamples-manojraheja.notebooks.azure.com/j/notebooks/Getting%20Started%20with%20kqlmagic%20on%20Azure%20Data%20Explorer.ipynb)


## <a name="install-kqlmagic-library"></a>Instalar a biblioteca de Kqlmagic

1. Instale Kqlmagic:

    ```python
    !pip install Kqlmagic --no-cache-dir  --upgrade
    ```

2. Carregar Kqlmagic:

    ```python
    reload_ext Kqlmagic
    ```

## <a name="connect-to-the-azure-data-explorer-help-cluster"></a>Ligar ao cluster ajuda de Explorador de dados do Azure

Utilize o seguinte comando para ligar à *exemplos* base de dados alojada na *ajudar* cluster. Para os utilizadores do AAD não Microsoft, substitua o nome do inquilino `Microsoft.com` com o seu inquilino do AAD.


```python
%kql AzureDataExplorer://tenant="Microsoft.com";code;cluster='help';database='Samples'
```

## <a name="query-and-visualize"></a>Consultar e visualizar

Consulta de dados com o [renderizar operador](/azure/kusto/query/renderoperator) e visualizar dados utilizando a biblioteca de ploy.ly. Esta consulta e visualização fornece uma experiência integrada que utiliza KQL nativo. Kqlmagic suporta a maioria dos gráficos, exceto `timepivot`, `pivotchart`, e `ladderchart`. Composição é suportada com todos os atributos, exceto `kind`, `ysplit`, e `accumulate`. 

### <a name="query-and-render-piechart"></a>Consultar e processar piechart

```python
%%kql 
StormEvents 
| summarize statecount=count() by State
| sort by statecount 
| limit 10
| render piechart title="My Pie Chart by State"
```

### <a name="query-and-render-timechart"></a>Pego de consulta e processamento

```python
%%kql
StormEvents
| summarize count() by bin(StartTime,7d)
| render timechart
```

> [!NOTE]
> Estes gráficos são interativos. Selecione um intervalo de tempo para aplicar zoom numa hora específica.

### <a name="customize-the-chart-colors"></a>Personalizar as cores do gráfico
Se não gostar a paleta de cores padrão, personalize os gráficos utilizando as opções de paleta. As paletas disponíveis podem ser encontradas aqui: [Escolha a paleta de cores para o resultado de gráfico de consulta Kqlmagic](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)

1. Para obter uma lista de paletas:

    ```python
    %kql --palettes -popup_window
    ```

1. Selecione o `cool` paleta de cores e compor a consulta novamente:

    ```python
    %%kql -palette_name "cool"
    StormEvents 
    | summarize statecount=count() by State
    | sort by statecount 
    | limit 10
    | render piechart title="My Pie Chart by State"
    ```

## <a name="parametrize-a-query-with-python"></a>Parametrize uma consulta com Python

Kqlmagic permite simple intercâmbio entre a linguagem de consulta de Kusto e Python. Para obter mais informações: [Parametrize sua consulta Kqlmagic com Python](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 

### <a name="use-a-python-variable-in-your-kql-query"></a>Utilizar uma variável de Python na sua consulta KQL

Pode usar o valor de uma variável de Python na sua consulta para filtrar os dados:

```python
statefilter = ["TEXAS", "KANSAS"]
```

```python
%%kql
let _state = statefilter;
StormEvents 
| where State in (_state) 
| summarize statecount=count() by bin(StartTime,1d), State
| render timechart title = "Trend"
```

### <a name="convert-query-results-to-pandas-dataframe"></a>Converter os resultados da consulta Pandas DataFrame 

Pode acessar os resultados de uma consulta KQL no Pandas DataFrame. Acessar os últimos resultados da consulta foi executada pela variável `_kql_raw_result_` e converter facilmente os resultados para Pandas DataFrame da seguinte forma:

```python
df = _kql_raw_result_.to_dataframe()
df.head(10)
```

### <a name="example"></a>Exemplo 

Em muitos cenários de análise, pode querer criar blocos de notas reutilizáveis que contêm muitas consultas e alimentam os resultados de uma consulta em consultas subsequentes. O exemplo abaixo utiliza a variável de Python `statefilter` para filtrar os dados.

1. Executar uma consulta para visualizar os Estados de 10 principais com máximo `DamageProperty`:

    ```python
    %%kql
    StormEvents 
    | summarize max(DamageProperty) by State
    | order by max_DamageProperty desc
    | limit 10
    ```

1. Execute uma consulta para extrair o estado superior e defini-lo numa variável de Python:

    ```python
    df = _kql_raw_result_.to_dataframe()
    statefilter =df.loc[0].State
    statefilter
    ```

1. Executar uma consulta utilizando o `let` instrução e a variável de Python:

    ```python
    %%kql
    let _state = statefilter;
    StormEvents 
    | where State in (_state) 
    | summarize statecount=count() by bin(StartTime,1d), State
    | render timechart title = "Trend"
    ```

1. Execute o comando de ajuda: 

    ```python
    %kql --help "help"
    ```

## <a name="next-steps"></a>Passos Seguintes
    
Execute o comando de ajuda para explorar os blocos de notas de exemplo seguintes que contêm todas as funcionalidades suportadas:
- [Introdução ao Kqlmagic para Explorador de dados do Azure](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStart.ipynb) 
- [Introdução ao Kqlmagic para o Application Insights](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartAI.ipynb) 
- [Introdução ao Kqlmagic para o Log Analytics](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartLA.ipynb) 
- [Parametrize sua consulta Kqlmagic com Python](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 
- [Escolha a paleta de cores para o resultado de gráfico de consulta Kqlmagic](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)



