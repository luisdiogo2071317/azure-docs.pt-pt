---
title: Carregar dados para ambientes de armazenamento do Azure para análise | Documentos da Microsoft
description: Mover dados de e para o Armazenamento de Blobs do Azure
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: f73488947b85c97d00257faf8907055227570cb6
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441729"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Carregar dados para ambientes de armazenamento para análise

O processo de ciência de dados de equipa exige que o dados ser ingeridos ou carregados para uma variedade de ambientes de armazenamento diferentes a serem processados ou analisados da forma mais adequada em cada fase do processo. Destinos de dados utilizados frequentemente para processamento incluem armazenamento de Blobs do Azure, bases de dados do SQL Azure, SQL Server em VM do Azure, o HDInsight (Hadoop) e o Azure Machine Learning. 

Os seguintes artigos descrevem como a ingestão de dados em vários ambientes de destino onde os dados são armazenados e processados.

* De/para [armazenamento de Blobs do Azure](move-azure-blob.md)
* Para [SQL Server numa VM do Azure](move-sql-server-virtual-machine.md)
* Para [base de dados SQL do Azure](move-sql-azure.md)
* Para [tabelas do Hive](move-hive-tables.md)
* Para [tabelas particionadas do SQL](parallel-load-sql-partitioned-tables.md)
* De [no local do SQL Server](move-sql-azure-adf.md)

Técnica e necessidades de negócio, bem como a localização inicial, formate e tamanho de seus dados irá determinar os ambientes de destino no qual os dados têm de ser ingeridos para atingir as metas da sua análise. Não é incomum para um cenário de transferência de dados para ser movido entre vários ambientes para alcançar a variedade de tarefas necessárias para construir um modelo preditivo. Esta sequência de tarefas pode incluir, por exemplo, exploração de dados, processamento prévio de, limpeza, amostragem de baixo e preparação de modelos.
