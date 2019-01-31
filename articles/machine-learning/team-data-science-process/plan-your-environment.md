---
title: Identificar cenários e planear o processo de análise - Team Data Science Process
description: Identificar cenários e planear para processamento de dados de análise avançada por uma série de perguntas importantes a considerar.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 0cf9ffddffe7f97008e69c9c4fb7945faeb779da
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469605"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Como identificar cenários e planear o processamento de dados de análises avançadas
Quais recursos planeja incluir ao configurar um ambiente para fazer a análise avançada de processamento num conjunto de dados? Este artigo sugere uma série de perguntas para solicitar que o ajudam a identificar as tarefas e os recursos relevantes o seu cenário. A ordem dos passos de alto nível para Análise Preditiva é descrita na [o que é o Team Data Science Process (TDSP)?](overview.md). Cada uma dessas etapas exige recursos específicos para as tarefas relevantes para o seu cenário específico. Principais perguntas para identificar o seu cenário preocupação logística de dados, características, a qualidade dos conjuntos de dados e as ferramentas e linguagens de que preferir efetuar a análise.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Perguntas de logística: e movimentação de localizações de dados
As perguntas de logística se a localização do **origem de dados**, o **destino pretendido** no Azure e os requisitos para mover os dados, incluindo a agenda, quantidade e recursos envolvidos. Os dados poderão ter de ser movidos várias vezes durante o processo de análise. Um cenário comum é mover dados locais em alguma espécie de armazenamento no Azure e, em seguida, no Machine Learning Studio.

1. **O que é a sua origem de dados?** É local ou na cloud? Por exemplo:
   
   * Os dados estão publicamente disponíveis num endereço HTTP.
   * Os dados residem num local de arquivo/rede local.
   * Os dados estão num banco de dados do SQL Server.
   * Os dados são armazenados no contentor de armazenamento do Azure
2. **O que é o destino do Azure?** Em que precisa ser para processamento ou de modelagem? Por exemplo:
   
   * Armazenamento de Blobs do Azure
   * Bases de dados do SQL Azure
   * SQL Server numa VM do Azure
   * O HDInsight (Hadoop no Azure) ou tabelas do Hive
   * Azure Machine Learning
   * Montáveis Azure discos rígidos virtuais.
3. **Como fará para mover os dados?** Os procedimentos e os recursos disponíveis para a ingestão ou carregar dados para uma variedade de armazenamento diferente e ambientes de processamento são descritos nos seguintes artigos:
   
   * [Carregar dados para ambientes de armazenamento para análise](ingest-data.md)
   * [Importar os dados de treinamento para o Azure Machine Learning Studio a partir de várias origens de dados](../studio/import-data.md).
4. **Os dados precisam ser movidos com base numa agenda regular ou modificados durante a migração?** Considere a utilização do Azure Data Factory (ADF) quando os dados têm de ser migradas continuamente, especialmente se num cenário híbrido, que acessa os no local e recursos na cloud está envolvido, ou quando os dados são transacionados ou tem de ser modificado ou tem a lógica de negócios adicionada ao ele é no decorrer a ser migrados. Para obter mais informações, consulte [mover dados de um servidor SQL no local para o SQL Azure com o Azure Data Factory](move-sql-azure-adf.md)
5. **Quanto dos dados está a ser movidos para o Azure?** Conjuntos de dados muito grandes podem exceder a capacidade de armazenamento de determinados ambientes. Por exemplo, consulte a discussão de limites de tamanho para o Machine Learning Studio, na secção seguinte. Nesses casos, uma amostra dos dados pode ser utilizada durante a análise. Para obter detalhes de como dimensionar um conjunto de dados em vários ambientes do Azure, consulte [amostra dos dados no Team Data Science Process](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Perguntas de características de dados: tipo, o formato e o tamanho
Estas questões são essenciais para planejar seu armazenamento e processamento de ambientes, cada um dos quais são adequadas para vários tipos de dados e cada um deles tem determinadas restrições.

1. **Quais são os tipos de dados?** Por exemplo:
   
   * Numérica
   * Categórico
   * Cadeias
   * Binário
2. **Como são formatados os seus dados?** Por exemplo:
   
   * Separados por vírgulas (CSV) ou separados por tabulações (TSV) ficheiros simples
   * Comprimidos ou descomprimidos
   * Blobs do Azure
   * Tabelas do Hive do Hadoop
   * Tabelas do SQL Server
3. **Como grandes são os seus dados?**
   
   * Pequenos: Menos de 2 GB
   * Médio: Maior que 2 GB e inferior a 10 GB
   * Grande: Superior a 10 GB

Considere o ambiente do Azure Machine Learning Studio, por exemplo:

* Para obter uma lista dos formatos de dados e tipos suportados pelo Azure Machine Learning Studio, consulte [formatos de dados e tipos de dados suportados](../studio/import-data.md#data-formats-and-data-types-supported) secção.


Para obter informações sobre as limitações de outros serviços do Azure usados no processo de análise, consulte [subscrição do Azure e limites do serviço, Quotas e restrições](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Perguntas de qualidade de dados: exploração e processamento prévio de
1. **O que sabe sobre os seus dados?** Explore os dados para obter um compreender das suas características básicas. Os padrões ou tendências que ele exibe um, os quais valores atípicos tem ou quantos valores estão em falta. Este passo é importante para determinar o grau de pré-processamento necessário, para formular hipóteses que possam sugerir os recursos mais apropriados ou tipo de análise e para formular planos para a recolha de dados adicionais. Calcular estatísticas descritivas e visualizações de desenho são técnicas úteis para inspeção de dados. Para obter detalhes de como explorar um conjunto de dados em vários ambientes do Azure, consulte [explorar dados no Team Data Science Process](explore-data.md).
2. **Os dados precisam de pré-processamento ou limpeza?**
   Pré-processar e limpeza de dados são tarefas importantes que, normalmente, têm de ser conduzidas antes do conjunto de dados pode ser utilizado eficazmente para o machine learning. Dados não processados costuma desnecessárias e pouco fiáveis e podem estar em falta valores. Usar esses dados para a Modelagem pode produzir resultados enganosos. Para obter uma descrição, consulte [tarefas para preparar dados para avançada do machine learning](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Perguntas de ferramentas e linguagens
Existem muitas opções aqui dependendo de quais linguagens e ambientes de desenvolvimento ou ferramentas precisam ou estão a utilizar mais conformable.

1. **Quais idiomas preferir usar para análise?**  
   
   * R
   * Python
   * SQL
2. **Quais ferramentas deve usar para análise de dados?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -uma linguagem de script utilizada para administrar os recursos do Azure numa linguagem de script.
   * [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
   * [O Revolution Analytics](https://www.microsoft.com/sql-server/machinelearningserver)
   * [RStudio](http://www.rstudio.com)
   * [Ferramentas do Python para Visual Studio](https://aka.ms/ptvsdocs)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Blocos de notas do Jupyter](http://jupyter.org/)
   * [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identificar o seu cenário de análise avançada
Depois de ter a respondeu às questões na secção anterior, está pronto para determinar qual o cenário de melhor se adequa a seu caso. Os cenários de exemplo descritos na [cenários de análises avançadas no Azure Machine Learning](plan-sample-scenarios.md).

