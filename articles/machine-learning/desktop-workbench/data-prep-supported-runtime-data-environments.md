---
title: Combinações suportadas do ambientes de execução e os dados de preparações de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece uma lista completa das combinações suportadas de origens de dados e tempos de execução diferentes para preparações de dados do Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 07b9ac8389a2bf06e0356f65d816d75d49cc0cf5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650393"
---
# <a name="supported-matrix-for-this-release"></a>Matriz suportada nesta versão 
Quando seu código carrega os dados ao utilizar origens de dados do Azure Machine Learning ou do Azure Machine Learning preparações de dados, obter qualquer um de um Pandas ou localizações de dados e ambientes de computação Spark dataframe, seguintes combinações de experimentação são suportados:

|     |Ficheiros locais  |Armazenamento de Blobs do Azure  |Base de dados do SQL Server ***  |
|---------|---------|---------|---------|---------|
|Local Python    |     Suportadas    |Não suportado         | Não suportado        |         |
|Python do docker (VM do Linux)     |Suportado em apenas os arquivos de projeto *         | Não suportado        |        Não suportado |         |
|PySpark do docker (VM do Linux)     |Suportado em apenas os arquivos de projeto *     |Suportadas         | Suportado (2)        |         |
|Python de Máquina Virtual de ciência de dados do Azure     |Suportado em apenas os arquivos de projeto *         |Não suportado         |Não suportado         |         |
|PySPark de Máquina Virtual de ciência de dados do Azure     | Suportado em apenas os arquivos de projeto *        |Não suportado         |Não suportado         |         |
|O Azure HDInsight PySpark     | Não suportado        |Suportadas         |Suportado (2)         |         |
|Python do Azure HDInsight     | Não suportado        | Não suportado        | Não suportado        |         |

Azure Data Lake Store não é atualmente suportada para qualquer destino de computação.

* Quando são utilizados caminhos de ficheiro local, os ficheiros dentro do projeto são copiados para o ambiente de computação e, em seguida, leia aqui. Não são copiados ficheiros fora do projeto e os caminhos já não serão resolvido no ambiente de computação. Considere utilizar a substituição de origem de dados para que seu código possa utilizar um ficheiro local, ao executar localmente. Em seguida, mude para um blob de armazenamento do Azure para uma configuração de execução diferente. Também pode utilizar o suporte de amostragem em origens de dados para gerir execuções em dados de grande dimensão apenas em determinadas configurações de execução.

* * Utiliza o controlador do Maven JDBC SQL Server 6.2.1. Tem de se certificar de que este pacote (ou um compatível) está incluído no seu ficheiro spark_dependencies.yml para o ambiente de computação.

Suporta a base de dados SQL ou SQL Server fornecida que a base de dados pode ser contactado a partir do ambiente de computação. 
