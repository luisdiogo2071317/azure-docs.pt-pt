---
title: Utilizar a extensibilidade do Python com preparações de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece uma descrição geral e alguns exemplos detalhados de como utilizar o código de Python para estender a funcionalidade de preparação de dados
services: machine-learning
author: euangMS
ms.author: euang
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.openlocfilehash: a713f5fcde31e0e25de080a65b71209011ef551d
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649600"
---
# <a name="data-preparations-python-extensions"></a>Extensões de Python de preparações de dados
Como uma forma de preencher as lacunas de funcionalidade entre as funcionalidades incorporadas, preparações de dados do Azure Machine Learning inclui extensibilidade em vários níveis. Neste documento, descrevemos a extensibilidade por meio de script de Python. 

## <a name="custom-code-steps"></a>Passos de código personalizado 
Preparações de dados tem os seguintes passos personalizados em que os usuários possam gravar código:

* Adicionar coluna
* Filtro Avançado
* Transformar fluxos de dados
* Transformar a partição

## <a name="code-block-types"></a>Tipos de bloco de código 
Para cada uma dessas etapas, damos suporte a dois tipos de bloco de código. Em primeiro lugar, suportamos uma expressão de Python bare, que é executado como está. Em segundo lugar, damos suporte a um módulo de Python onde chamamos uma função específica com uma assinatura conhecida no código que fornece.

Por exemplo, pode adicionar uma nova coluna que calcula o log de outra coluna das seguintes duas formas:

Expressão 

```python    
    math.log(row["Score"])
```

Módulo 
    
```python
def newvalue(row): 
        return math.log(row["Score"])
```


A transformação de Adicionar coluna no modo de módulo espera encontrar uma função chamada `newvalue` que aceita uma variável de linha e devolve o valor da coluna. Este módulo pode incluir qualquer quantidade de código de Python com outras funções, imports, etc.

Os detalhes de cada ponto de extensão são abordados nas seções a seguir. 

## <a name="imports"></a>Importações 
Se utilizar o tipo de bloco de expressão, pode ainda adicionar **importar** instruções para o seu código. Todos eles têm de estar agrupados nas linhas superior do seu código.

Corrigir 

```python
import math 
import numpy 
math.log(row["Score"])
```
 

Erro  

```python
import math  
math.log(row["Score"])  
import numpy
```
 
 
Se utilizar o tipo de bloco do módulo, pode seguir todas as regras de Python normais para utilizar o **importar** instrução. 

## <a name="default-imports"></a>Importações padrão
As seguintes importações são sempre incluído e é utilizável no seu código. Não precisa de reimportá-los. 

```python
import math  
import numbers  
import datetime  
import re  
import pandas as pd  
import numpy as np  
import scipy as sp
```
  

## <a name="install-new-packages"></a>Instalar novos pacotes
Para utilizar um pacote que não está instalado por predefinição, tem primeiro de instalá-lo para os ambientes que utiliza preparações de dados. Esta instalação precisa ser feito no seu computador local e quaisquer destinos de computação que pretende executar.

Para instalar os pacotes num destino de computação, terá de modificar o ficheiro de conda_dependencies.yml localizado na pasta aml_config na raiz do seu projeto.

### <a name="windows"></a>Windows 
Para encontrar a localização no Windows, localize a instalação de aplicações específicas de Python e o seu diretório de scripts. A localização predefinida é:  

`C:\Users\<user>\AppData\Local\AmlWorkbench\Python\Scripts` 

Em seguida, execute um dos seguintes comandos: 

`conda install <libraryname>` 

ou 

`pip install <libraryname> `

### <a name="mac"></a>Mac 
Para encontrar a localização num Mac, encontre a instalação de aplicações específicas de Python e o seu diretório de scripts. A localização predefinida é: 

`/Users/<user>/Library/Caches/AmlWorkbench/Python/bin` 

Em seguida, execute um dos seguintes comandos: 

`./conda install <libraryname>`

ou 

`./pip install <libraryname>`

## <a name="use-custom-modules"></a>Utilizar módulos personalizados
Em transformar fluxo de dados (Script), escrever o seguinte código de Python

```python
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction1
df = ExtensionFunction1(df)
```

Adicionar coluna (Script), defina o tipo de bloco de código = módulo e escreva o seguinte código de Python

```python 
import sys
sys.path.append(*<absolute path to the directory containing UserModule.py>*)

from UserModule import ExtensionFunction2

def newvalue(row):
    return ExtensionFunction2(row)
```
Para contextos de execução diferente (local, Docker, Spark), do ponto de caminho absoluto para o lugar certo. Pode querer utilizar "os.getcwd() + relativePath" para localizá-la.


## <a name="column-data"></a>Dados da coluna 
Dados de coluna podem ser acedidos a partir de uma linha utilizando a notação de ponto ou a notação de chave-valor. Nomes de colunas que contêm espaços ou carateres especiais não podem ser acedidos utilizando a notação de ponto. O `row` variável deve ser sempre definida em ambos os modos de extensões de Python (módulo e expressão). 

Exemplos 

```python
    row.ColumnA + row.ColumnB  
    row["ColumnA"] + row["ColumnB"]
```

## <a name="add-column"></a>Adicionar coluna 
### <a name="purpose"></a>Objetivo
O ponto de extensão de Adicionar coluna permite que escreva Python para calcular uma nova coluna. O código de escrita tem acesso para a linha total. Ele precisa retornar um novo valor de coluna para cada linha. 

### <a name="how-to-use"></a>Como utilizar
Pode adicionar este ponto de extensão ao utilizar o bloco de Adicionar coluna (Script). Está disponível no nível superior **transformações** menu, bem como no **coluna** menu de contexto. 

### <a name="syntax"></a>Sintaxe
Expressão

```python
    math.log(row["Score"])
```

Módulo 

```python
def newvalue(row):  
     return math.log(row["Score"])
```
 

## <a name="advanced-filter"></a>Filtro Avançado
### <a name="purpose"></a>Objetivo 
O ponto de extensão de filtro avançado permite que escreva um filtro personalizado. Tem acesso para a linha inteira, e seu código deve retornar True (incluir a linha) ou FALSO (excluir a linha). 

### <a name="how-to-use"></a>Como utilizar
Pode adicionar este ponto de extensão ao utilizar o bloco de filtro avançado (Script). Está disponível no nível superior **transformações** menu. 

### <a name="syntax"></a>Sintaxe

Expressão

```python
    row["Score"] > 95
```

Módulo  

```python
def includerow(row):  
    return row["Score"] > 95
```
 

## <a name="transform-dataflow"></a>Transformar fluxos de dados
### <a name="purpose"></a>Objetivo 
O ponto de extensão de transformar fluxo de dados permite-lhe completamente transformar o fluxo de dados. Tem acesso a um Pandas dataframe que contém todas as colunas e linhas que está a processar. O código tem de devolver um dataframe Pandas com os novos dados. 

>[!NOTE]
>Em Python, todos os dados para ser carregado na memória é num Pandas dataframe se esta extensão é utilizada. 
>
>No Spark, todos os dados são recolhidos num nó único do worker. Se os dados são muito grandes, uma função de trabalho poderá ficar sem memória. Utilize-o com cuidado.

### <a name="how-to-use"></a>Como utilizar 
Pode adicionar este ponto de extensão ao utilizar o bloco de transformar fluxo de dados (Script). Está disponível no nível superior **transformações** menu. 
### <a name="syntax"></a>Sintaxe 

Expressão

```python
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Módulo 

```python
def transform(df):  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()  
    return df
```
  

## <a name="transform-partition"></a>Transformar a partição  
### <a name="purpose"></a>Objetivo 
O ponto de extensão de partição de transformar permite-lhe transformar uma partição de fluxo de dados. Tem acesso a um Pandas dataframe que contém todas as colunas e linhas para essa partição. O código tem de devolver um dataframe Pandas com os novos dados. 

>[!NOTE]
>Em Python, poderá ficar com uma partição única ou várias partições, dependendo do tamanho dos seus dados. No Spark, está trabalhando com um pacote de dados que contém os dados para uma partição num nó de trabalho especificado. Em ambos os casos, não pode assumir que tem acesso a todo o conjunto de dados. 


### <a name="how-to-use"></a>Como utilizar
Pode adicionar este ponto de extensão ao utilizar o bloco de transformar partição (Script). Está disponível no nível superior **transformações** menu. 

### <a name="syntax"></a>Sintaxe 

Expressão 

```python
    df['partition-id'] = index  
    df['index-column'] = range(1, len(df) + 1)  
    df = df.reset_index()
```
 

Módulo 

```python
def transform(df, index):
    df['partition-id'] = index
    df['index-column'] = range(1, len(df) + 1)
    df = df.reset_index()
    return df
```


## <a name="datapreperror"></a>DataPrepError  
### <a name="error-values"></a>Valores de erro  
No preparações de dados, existe o conceito de valores de erro. 

É possível que ocorram os valores de erro no código de Python personalizado. Eles são instâncias de uma classe de Python chamado `DataPrepError`. Essa classe encapsula uma exceção de Python e tem algumas propriedades. As propriedades contêm informações sobre o erro ocorrido quando o valor original foi processado, bem como o valor original. 


### <a name="datapreperror-class-definition"></a>Definição de classe DataPrepError
```python 
class DataPrepError(Exception): 
    def __bool__(self): 
        return False 
``` 
A criação de um DataPrepError na estrutura Python de preparações de dados geralmente fica assim: 
```python 
DataPrepError({ 
   'message':'Cannot convert to numeric value', 
   'originalValue': value, 
   'exceptionMessage': e.args[0], 
   '__errorCode__':'Microsoft.DPrep.ErrorValues.InvalidNumericType' 
}) 
``` 
#### <a name="how-to-use"></a>Como utilizar 
É possível quando o Python é executado num ponto de extensão para gerar DataPrepErrors como valores de retorno ao utilizar o método de criação anterior. É muito mais provável que DataPrepErrors são encontrados quando dados são processados num ponto de extensão. Neste momento, o código de Python personalizado tem de lidar com um DataPrepError como um tipo de dados válido.

#### <a name="syntax"></a>Sintaxe 
Expressão 
```python 
    if (isinstance(row["Score"], DataPrepError)): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
```python 
    if (hasattr(row["Score"], "originalValue")): 
        row["Score"].originalValue 
    else: 
        row["Score"] 
``` 
Módulo 
```python 
def newvalue(row): 
    if (isinstance(row["Score"], DataPrepError)): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
``` 
```python 
def newvalue(row): 
    if (hasattr(row["Score"], "originalValue")): 
        return row["Score"].originalValue 
    else: 
        return row["Score"] 
```  
