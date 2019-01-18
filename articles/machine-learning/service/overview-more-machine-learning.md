---
title: Opções de produto de aprendizagem de comparação
titleSuffix: Microsoft
description: Compare a variedade de produtos da Microsoft para criar, implementar e gerir modelos de machine learning. Decida quais os produtos a escolher para a sua solução.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 1/17/2019
ms.openlocfilehash: d5660d1a4d5d2a26af57e2a2411552e3f6267379
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54389137"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Quais são os produtos de aprendizagem da Microsoft?

A Microsoft disponibiliza várias opções de produtos para criar, implementar e gerir os seus modelos de machine learning. Compare estes produtos e escolha aquilo de que precisa para desenvolver as suas soluções de machine learning com mais eficiência.


**Opções baseadas na nuvem**

As seguintes opções estão disponíveis para aprendizagem automática na cloud do Azure.

| Cloud&nbsp;opções | O que é | O que permite fazer |
|-|-|-|
| [Serviço Azure Machine Learning](#azure-machine-learning-services) | Serviço cloud gerido para ML  | Preparar, implementar e gerir modelos no Azure com Python e a CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Arraste&ndash;e&ndash;interface visual do menu de ML | Criar, testar e implementar modelos através de algoritmos pré-configurada (Python e R)|
| [Azure Databricks](#azure-databricks) | Plataforma de análise baseada no Spark | Criar e implementar modelos e fluxos de trabalho de dados |
| [Serviços Cognitivos do Azure](#azure-cognitive-services) | Serviços do Azure com IA e modelos de ML pré-criados | Adicionar facilmente funcionalidades inteligentes às suas aplicações |
| [Máquina Virtual de Ciência de Dados do Azure](#azure-data-science-virtual-machine) | Máquina virtual com ferramentas de ciência de dados pré-instaladas | Desenvolver soluções de ML num ambiente pré-configurado |

**Opções no local**

As seguintes opções estão disponíveis para aprendizagem automática no local. Servidores no local também podem executar numa máquina virtual na cloud.

| No local&nbsp;opções | O que é | O que permite fazer |
|-|-|-|
| [Serviços de Machine Learning do SQL Server](#sql-server-machine-learning-services) | Motor de análise incorporado no SQL | Criar e implementar modelos no SQL Server |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Servidor empresarial autónomo para análise preditiva | Criar e implementar modelos com R e Python |

**Ferramentas de desenvolvimento**

As seguintes ferramentas de desenvolvimento estão disponíveis para o machine learning.

| Desenvolvimento&nbsp;ferramentas | O que é | O que permite fazer |
|-|-|-|
| [ML.NET](#mlnet) | SDK de ML de open source para várias plataformas | Criar soluções de ML para aplicações .NET |
| [Windows ML](#windows-ml) | Plataforma de ML do Windows 10 | Avaliar os modelos preparados num dispositivo Windows 10 |







## <a name="azure-machine-learning-service"></a>Serviço Azure Machine Learning

[O serviço do Azure Machine Learning](overview-what-is-azure-ml.md) é um serviço totalmente gerido na cloud utilizado para preparar, implementar e gerir modelos de ML, à escala. Suporta totalmente tecnologias de open source, pelo que pode utilizar dezenas de milhares de pacotes Python de open source, como o TensorFlow, o PyTorch e o scikit-learn. Também estão disponíveis, como ferramentas de rich [blocos de notas do Azure](https://notebooks.azure.com/), [blocos de notas do Jupyter](http://jupyter.org), ou o [Azure Machine Learning para Visual Studio Code](https://aka.ms/vscodetoolsforai) extensão torna mais fácil explorar e transformar dados e, em seguida, preparar e implementar modelos. O serviço Azure Machine Learning inclui funcionalidades que automatizam a otimização e a geração de modelos com facilidade, eficiência e precisão.

Utilize o serviço Azure Machine Learning para preparar, implementar e gerir modelos de ML com Python e a CLI à escala da cloud.

Experimente o [uma versão gratuita ou paga do serviço Azure Machine Learning](http://aka.ms/AMLFree) hoje mesmo.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

O [Azure Machine Learning Studio](../studio/what-is-ml-studio.md) fornece-lhe uma área de trabalho interativa e visual que pode utilizar para criar, testar e implementar modelos rápida e facilmente com algoritmos de machine learning pré-criados. Machine Learning Studio publica modelos como serviços web que podem facilmente ser consumidos por aplicações personalizadas ou ferramentas de BI como o Excel.
Não é necessária programação. Pode construir o seu modelo de machine learning ao ligar conjuntos de dados e módulos de análise numa tela interativa e, em seguida, implementá-lo com alguns cliques.

Utilize o Machine Learning Studio para criar e implementar modelos sem necessidade de código.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

O [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) é uma plataforma de análise baseada no Apache Spark e otimizada para a plataforma de serviços cloud Microsoft Azure. O Databricks está integrado no Azure para prestar configurações com um clique, fluxos de trabalho fluídos e uma área de trabalho interativa que permite a colaboração entre cientistas de dados, engenheiros de dados e analistas empresariais.
Utilize código Python, R, Scala e SQL em blocos de notas baseados na Web para consultar, visualizar e modelar dados.

Utilize o Databricks quando quiser colaborar na criação de soluções de machine learning no Apache Spark.

## <a name="azure-cognitive-services"></a>Serviços Cognitivos do Azure

Os [Serviços Cognitivos do Azure](/azure/cognitive-services/welcome) são um conjunto de APIs com as quais pode criar aplicações que utilizam métodos naturais de comunicação. Estas APIs permitem que as suas aplicações vejam, escutem, falem, compreendam e interpretem as necessidades do utilizador com apenas algumas linhas de código. Adicione facilmente funcionalidades inteligentes às suas aplicações, tais como: 

- Deteção de emoções e sentimentos
- Reconhecimento de voz e visão
- Language understanding (LUIS)
- Conhecimentos e investigação

Utilize os Serviços Cognitivos para criar aplicações em vários dispositivos e plataformas. As APIs são melhoradas constantemente e são fáceis de configurar.

## <a name="azure-data-science-virtual-machine"></a>Máquina Virtual de Ciência de Dados do Azure

A [Máquina Virtual de Ciência de Dados do Azure](../data-science-virtual-machine/overview.md) é uma máquina virtual personalizada na cloud do Microsoft Azure, concebida especificamente para fazer ciência de dados. Tem muitas ferramentas populares de ciência de dados e outras pré-instaladas e pré-configuradas para permitir a rápida criação de aplicações inteligentes para análises avançadas.

A Máquina Virtual de Ciência de Dados é suportada como um destino do serviço Azure Machine Learning.
Está disponível nas versões para Windows e Linux Ubuntu (do Azure Machine Learning serviço não é suportado no Linux CentOS).
Para obter informações de versões específicas e uma lista do que está incluído, veja [Introdução à Máquina Virtual de Ciência de Dados do Azure](../data-science-virtual-machine/overview.md).

Utilize a VM de Ciência de Dados quando precisar de executar ou alojar as suas tarefas num único nó. Ou se tiver de aumentar verticalmente de forma remota o processamento numa máquina individual.

## <a name="sql-server-machine-learning-services"></a>Serviços de Machine Learning do SQL Server

O [Microsoft Machine Learning Services do SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) adiciona análise estatística, visualização de dados e análise preditiva em R e Python para dados relacionais em bases de dados do SQL Server. As bibliotecas de R e Python da Microsoft incluem modelação avançada e algoritmos de ML, que podem ser executados em paralelo e em escala no SQL Server.

Utilize o SQL Server Machine Learning Services quando precisar de IA e análise preditiva incorporadas em dados relacionais no SQL Server.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server

O [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) é um servidor empresarial para alojar e gerir cargas de trabalho paralelas e distribuídas de processos de R e Python. O Microsoft Machine Learning Server funciona em Linux, Windows, Hadoop e Apache Spark, e também está disponível no [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Disponibiliza um motor de execução para soluções criadas com os pacotes [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) e [MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), e expande as linguagens R e Python de open source com suporte para análise de elevado desempenho, análise estatística, machine learning e conjuntos de dados extremamente grandes. Esta funcionalidade é fornecida através dos pacotes proprietários que são instalados com o servidor. Para desenvolvimento, pode utilizar IDEs como o [R Tools for Visual Studio](https://www.visualstudio.com/vs/rtvs/) e o [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python/).

Utilize o Microsoft Machine Learning Server quando precisar de criar e operacionalizar modelos criados com R e Python num servidor ou distribuir preparações em R e Python em escala num cluster do Hadoop ou do Spark.

## <a name="mlnet"></a>ML.NET

O [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) é uma arquitetura de machine learning de open source para várias plataformas que permite criar soluções de machine learning personalizadas e integrá-las nas suas aplicações .NET.

Utilize o ML.NET quando quiser integrar soluções de machine learning nas suas aplicações .NET.

## <a name="windows-ml"></a>Windows ML

O [Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) permite utilizar modelos de machine learning preparados nas suas aplicações ao avaliar os modelos preparados localmente em dispositivos Windows 10.

Utilize o Windows ML quando quiser usar modelos de machine learning preparados nas suas aplicações do Windows.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre todos os produtos de programação com base em Inteligência Artificial (IA) disponíveis na Microsoft, veja [Microsoft AI platform](https://www.microsoft.com/ai) (Plataforma de IA da Microsoft)
- Para saber como criar soluções de IA, veja [Microsoft AI School](https://aischool.microsoft.com/learning-paths)
