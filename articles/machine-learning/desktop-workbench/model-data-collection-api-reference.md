---
title: Referência da API de recolha de dados do Machine Learning modelo do Azure | Documentos da Microsoft
description: Referência da API de recolha de dados do Machine Learning modelo do Azure.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ROBOTS: NOINDEX
ms.openlocfilehash: c047555c30481f34b607197f71483197bc64620c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961472"
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Referência da API de recolha de dados do Machine Learning modelo do Azure

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Recolha de dados de modelo permite-lhe arquivar as entradas de modelo e as previsões de um serviço web machine learning. Consulte a [guia de procedimentos de recolha de dados de modelo](how-to-use-model-data-collection.md) para saber como pode instalar `azureml.datacollector` no seu computador Windows e Linux.

Neste guia de referência de API, usamos uma abordagem passo a passo sobre como recolher entradas de modelo e as previsões de um serviço web machine learning.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Ativar a recolha de dados de modelo no ambiente do Azure ML Workbench

 Procure conda\_dependencies.yml de ficheiros no seu projeto na pasta aml_config e tem seu conda\_dependencies.yml ficheiro incluir o módulo de azureml.datacollector na secção pip, conforme mostrado abaixo. Tenha em atenção que se trata de uma subseção de um total conda\_dependencies.yml ficheiro:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Atualmente, pode utilizar o módulo de recoletor de dados no Azure ML Workbench ao executar no modo de docker. Modo de local poderá não funcionar para todos os ambientes.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Ativar a recolha de dados de modelo no ficheiro de classificação

O ficheiro de classificação que está a ser utilizado para operacionalização, importe o módulo de recoletor de dados e a classe de ModelDataCollector:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Instanciação de recoletor de dados de modelo
Criar uma nova instância de um ModelDataCollector:

    dc = ModelDataCollector(model_name, identifier='default', feature_names=None, model_management_account_id='unknown', webservice_name='unknown', model_id='unknown', model_version='unknown')

Ver detalhes de classe e o parâmetro:

### <a name="class"></a>Classe
| Nome | Descrição |
|--------------------|--------------------|
| ModelDataCollector | Uma classe no espaço de nomes de azureml.datacollector. Uma instância dessa classe será utilizada para recolher dados de modelo. Um único ficheiro de classificação pode conter vários ModelDataCollectors. Cada instância deve ser utilizada para a recolha de dados num único local discreto no ficheiro de classificação, para que o esquema de dados recolhidos permanece consistente (ou seja, entradas e predição)|


### <a name="parameters"></a>Parâmetros

| Nome | Tipo | Descrição |
|-------------|------------|-------------------------|
| nome_modelo | cadeia | o nome do modelo de quais dados são recolhidos para |
| Identificador | cadeia | a localização no código que identifica estes dados, ou seja 'RawInput' ou "Predição" |
| feature_names | lista de cadeias de caracteres | uma lista de nomes de recursos que tornam-se o cabeçalho de csv quando fornecido |
| model_management_account_id | cadeia | o identificador para a conta de gestão de modelo em que este modelo é armazenado. Isso é preenchido automaticamente quando modelos são operacionalizados por meio de AML |
| webservice_name | cadeia | o nome do webservice em que esse modelo é atualmente implementado. Isso é preenchido automaticamente quando modelos são operacionalizados por meio de AML |
| model_id | cadeia | O identificador exclusivo para este modelo no contexto de uma conta de gestão de modelos. Isso é preenchido automaticamente quando modelos são operacionalizados por meio de AML |
| model_version | cadeia | o número de versão desse modelo no contexto de uma conta de gestão de modelos. Isso é preenchido automaticamente quando modelos são operacionalizados por meio de AML |



 

## <a name="collecting-the-model-data"></a>Recolher os dados do modelo

É possível recolher os dados do modelo a utilizar uma instância do ModelDataCollector criada acima.

    dc.collect(input_data, user_correlation_id="")

Ver detalhes de método e o parâmetro:

### <a name="method"></a>Método
| Nome | Descrição |
|--------------------|--------------------|
| recolher | Utilizada para recolher os dados para uma entrada de modelo ou de predição|


### <a name="parameters"></a>Parâmetros

| Nome | Tipo | Descrição |
|-------------|------------|-------------------------|
| input_data | vários tipos | os dados a serem recolhidos (atualmente aceita a lista de tipos, numpy.array, pandas. Pacote de dados, pyspark.sql.DataFrame). Para tipos de pacote de dados, se não existe um cabeçalho com nomes de funcionalidade, estas informações estão incluídas no destino de dados (sem a necessidade de passar explicitamente os nomes das funcionalidades no construtor ModelDataCollector) |
| user_correlation_id | cadeia | um id de correlação opcional, que pode ser fornecido pelo utilizador para correlacionar esta predição |

