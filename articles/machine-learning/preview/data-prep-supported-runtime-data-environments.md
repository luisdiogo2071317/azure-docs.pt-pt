---
title: "Suportado combinações de ambientes de execução e os dados para preparativos de dados do Azure Machine Learning | Microsoft Docs"
description: "Este documento fornece uma lista completa das combinações suportadas de tempos de execução diferentes e origens de dados para o Azure Machine Learning dados preparativos"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 248cbcfe35db646a8bc71c6f825dcaa8a4661e91
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="supported-matrix-for-this-release"></a>Matriz suportada para esta versão 
Quando o código carrega dados através da utilização de origens de dados do Azure Machine Learning ou do Azure Machine Learning dados preparativos, obter ou um Pandas ou localizações de dados e ambientes de computação do Spark dataframe, seguintes combinações de experimentação são suportados:

|     |Ficheiros locais  |Armazenamento de Blobs do Azure  |Base de dados do SQL Server ***  |
|---------|---------|---------|---------|---------|
|Local Python    |     Suportadas    |Não suportado         | Não suportado        |         |
|Python docker (VM com Linux)     |Suportado apenas os ficheiros de projeto *         | Não suportado        |        Não suportado |         |
|PySpark docker (VM com Linux)     |Suportado apenas os ficheiros de projeto *     |Suportadas         | Suportado (2)        |         |
|Python de Máquina Virtual de ciência de dados do Azure     |Suportado apenas os ficheiros de projeto *         |Não suportado         |Não suportado         |         |
|PySPark de Máquina Virtual de ciência de dados do Azure     | Suportado apenas os ficheiros de projeto *        |Não suportado         |Não suportado         |         |
|PySpark do Azure HDInsight     | Não suportado        |Suportadas         |Suportado (2)         |         |
|Python do Azure HDInsight     | Não suportado        | Não suportado        | Não suportado        |         |

O Azure Data Lake Store não é atualmente suportado para qualquer destino de computação.

* Quando são utilizados caminhos de ficheiro local, os ficheiros no projeto são copiados para o ambiente de computação e, em seguida, existe a ler. Ficheiros fora do projecto não são copiados e os caminhos já não irão resolver no ambiente de computação. Considere a utilização de substituição de origem de dados para que o seu código pode utilizar um ficheiro local ao executar localmente. Em seguida, mude para um blob de armazenamento do Azure para uma configuração de execução diferente. Também pode utilizar o suporte de amostragem em origens de dados para gerir as execuções de dados de grandes dimensões apenas em determinadas configurações de execução.

* * Utiliza o controlador do Maven JDBC SQL Server 6.2.1. Tem de se certificar de que este pacote (ou um tipo compatível) está incluído no seu ficheiro spark_dependencies.yml para o ambiente de computação.

SQL Database do Azure suporta ou o SQL Server fornecidos que a base de dados pode ser contactado a partir do ambiente de computação. 
