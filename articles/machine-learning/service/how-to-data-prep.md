---
title: Preparar dados com o SDK de preparação de dados do Machine Learning para Python - Azure
description: Saiba como utilizar o Azure Machine Learning dados Prep SDK para Python para carregar dados de vários formatos, transformá-lo para ser mais utilizável e gravá-los para uma localização para os seus modelos aceder.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/27/2018
ms.openlocfilehash: 91d0f3565db484504a67a3b6ae0989b9291cd24f
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446437"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Preparar dados para modelagem com o Azure Machine Learning

Neste artigo, ficará a conhecer os recursos exclusivos do SDK de Prep de dados do Azure Machine Learning e casos de utilização. Preparação de dados é a parte mais importante de um fluxo de trabalho de aprendizagem automática. Dados do mundo real, muitas vezes, são apresentados, inconsistentes ou não é possível ser utilizado como dados de treinamento sem significativo limpeza e transformação. Correção de erros e anomalias nos dados não processados e a criação de novos recursos que são relevantes para o problema que está a tentar resolver, irão aumentar a precisão do modelo. O SDK foi concebido para ser familiar para os utilizadores de outras bibliotecas de preparação de dados comuns, enquanto oferecem vantagens para cenários-chave e a manutenção de interoperabilidade com essas outras bibliotecas.

Pode se preparar seus dados em Python com o [SDK do Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk).

## <a name="azure-machine-learning-data-prep-sdk"></a>Do Azure Machine Learning de preparação de dados SDK

O [SDK do Azure Machine Learning Data Prep](https://aka.ms/data-prep-sdk) é uma biblioteca de Python que oferece:

* Transformações de economizar tempo de inteligentes, como o processamento de esquema Fuzzy Grouping, coluna derivada por exemplo, divisão automática, inteligentes de leitura de arquivo e desalinhadas direito.
* Uma única API que funciona em pequenas dados localmente ou dados de grande dimensão na cloud, com **alguns para sem alterações de código**.
* A capacidade de dimensionar com mais eficiência num único computador com o uso de uma abordagem de transmissão em fluxo para processar os dados, em vez de carregamento na memória.

### <a name="install-the-sdk"></a>Instalar o SDK

Instale o SDK no seu ambiente de Python com o seguinte comando.

```shell
pip install azureml-dataprep
```

Utilize o seguinte código para importar o pacote.

```python
import azureml.dataprep as dprep
```

### <a name="examples-and-reference"></a>Referência e exemplos

Para saber mais sobre os módulos e funções deste SDK, consulte a [documentos de referência do SDK de preparação de dados](https://aka.ms/data-prep-sdk).

Os exemplos seguintes realçam algumas das funcionalidades exclusivas do SDK, incluindo:

* Deteção do tipo automática de ficheiros
* Transformações inteligentes
* Estatísticas de resumo
* Funcionalidade de ambiente de várias


#### <a name="automatic-file-type-detection"></a>Deteção do tipo automática de ficheiros

Utilize o `smart_read_file()` função para carregar os seus dados sem ter de especificar o tipo de ficheiro. Esta função automaticamente reconhece e analisa o tipo de ficheiro.

```python
dataflow = dprep.smart_read_file(path="<your-file-path>")
```

#### <a name="intelligent-transforms"></a>Transformações inteligentes

Utilize o SDK para dividir e derivar colunas por exemplo e inferência de tipos para automatizar a engenharia de funcionalidades. Suponha que tem um campo no seu objeto de fluxo de dados chamado `datetime` com um valor de `2018-09-15 14:30:00`.

Dividir automaticamente o `datetime` campo, chamar a função a seguir.

```python
new_dataflow = dataflow.split_column_by_example(source_column="datetime")
```

Ao não definir o parâmetro de exemplo, a função dividirá automaticamente a `datetime` campo em dois campos novos `datetime_1` e `datetime_2`. Os valores resultantes são `2018-09-15` e `14:30:00`, respectivamente. Também é possível fornecer um padrão de exemplo e o SDK irá prever e executar a transformação pretendida. Com o mesmo `datetime` objeto, o código a seguir, irá criar uma nova coluna `datetime_weekday` para o dia da semana com base no exemplo fornecido.

```python
new_dataflow = dataflow.derive_column_by_example(
        source_columns="datetime", 
        new_column_name="datetime_weekday", 
        example_data=[("2009-01-04 10:12:00", "Sunday"), ("2013-08-22 17:00:00", "Thursday")]
    )
```

#### <a name="summary-statistics"></a>Estatísticas de resumo

Pode gerar estatísticas de resumo rápidas para um fluxo de dados com uma linha de código. Este método oferece uma forma conveniente para compreender os seus dados e a forma como é distribuída.

```python
dataflow.get_profile()
```

Chamar essa função num objeto de fluxo de dados resultará na saída, como a tabela seguinte.

![Saída de estatísticas de resumo](./media/concept-data-preparation/output-example.png)

## <a name="multiple-environment-compatibilities"></a>Vários às compatibilidades de ambiente

O SDK também permite que os objetos de fluxo de dados a ser serializado e aberto num *qualquer* ambiente do Python. O ambiente em que for aberto pode ser diferente que o ambiente onde é guardada. Esta funcionalidade permite a transferência fácil entre ambientes do Python e integração rápida com modelos do Azure Machine Learning.

Utilize o seguinte código para guardar os objetos de fluxo de dados.

```python
package = dprep.Package([dataflow_1, dataflow_2])
package.save("<your-local-path>")
```

Utilize o seguinte código para reabrir o seu pacote em qualquer ambiente e obter uma lista de objetos de fluxo de dados.

```python
package = dprep.Package.open("<your-local-path>")
dataflow_list = package.dataflows
```

## <a name="data-preparation-pipeline"></a>Pipeline de preparação de dados

Para ver exemplos detalhados e o código para cada passo de preparação, utilize os seguintes guias de procedimentos:

1. [Carregar dados](how-to-load-data.md), que pode ser em vários formatos
2. [Transformar](how-to-transform-data.md) -lo numa estrutura mais utilizável
3. [Escrever](how-to-write-data.md) esses dados para uma localização acessível aos seus modelos

![Processo de preparação de dados](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Passos Seguintes

Revisão de um [bloco de notas do exemplo](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/tutorials/getting-started/getting-started.ipynb) de preparação de dados com o SDK do Azure Machine Learning Data Prep.

Azure SDK do Machine Learning Data Prep [documentação de referência](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py).
