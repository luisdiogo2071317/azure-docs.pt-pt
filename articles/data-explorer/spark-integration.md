---
title: Conector do Explorador de dados do Azure para o Apache Spark
description: Este artigo mostra-lhe como utilizar o conector do Explorador de dados do Azure para o Apache Spark.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: 6f115df97d0b790c94d6dc07f3f40feeceb1a7cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824214"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Conector do Explorador de dados do Azure para o Apache Spark

Utilizar o Explorador de dados do Azure e do Apache Spark, pode criar aplicações rápidas e escaláveis, destinado a cenários, tais como a aprendizagem automática (ML), Extract-Transform-Load (ETL) e do Log Analytics de orientados a dados.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o conector, tem de ter a aplicação:

* 1.8 do Java SDK
* Maven 3.x
* Versão 2.3.2 Spark ou superior

## <a name="link-to-data-explorer"></a>Ligação para o Data Explorer

Definições do projeto para aplicativos Scala e Java com o Maven, ligue a sua aplicação com o artefacto seguinte:

```java
groupId = com.microsoft.azure
artifactId = spark-kusto-connector
version = 1.0.0-Beta-01 
```

Maven, ligar com o seguinte:

```Maven
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-kusto-connector</artifactId>
    <version>1.0.0-Beta-01</version>
  </dependency>
```

## <a name="build-commands"></a>Criar comandos de introdução

Para criar jar e executar todos os testes:

```
mvn clean package
```

Para criar jar, executar o teste de todas as e instalar o jar ao seu repositório local do Maven:

```
mvn clean install
```