---
title: Origens de dados disponíveis com a preparação de dados do Azure Machine Learning suportadas | Documentos da Microsoft
description: Este documento fornece uma lista completa das origens de dados suportadas disponíveis para a preparação de dados do Azure Machine Learning.
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
ms.openlocfilehash: 88ed4fa43e5724cfe1d6f1555db947d77045cd2e
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650452"
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Origens de dados suportadas para preparação de dados do Azure Machine Learning 
Este artigo descreve as origens de dados suportado atualmente para preparação de dados do Azure Machine Learning.

Seguem-se a origens de dados suportadas nesta versão.

## <a name="types"></a>Tipos 

### <a name="sql-server"></a>SQL Server
Ler a partir do servidor SQL no local ou a base de dados SQL do Azure.

#### <a name="options"></a>Opções
- Endereço do Servidor
- Confiar no servidor (mesmo quando o certificado no servidor não é válido. Utilize com cuidado)
- Tipo de autenticação (Windows, servidor)
- Nome de Utilizador
- Palavra-passe
- Para ligar à base de dados
- Consulta SQL

#### <a name="notes"></a>Notas
- Colunas de variante do SQL não são suportadas
- Coluna de hora é convertida para datetime, acrescentando a hora da base de dados até à data 1970/1/1
- Quando executada no cluster do Spark, todos os dados relacionados com colunas (data, datetime, datetime2, datetimeoffset) irão avaliar os valores incorretos para datas antes 1583
- Valores nas colunas decimais podem perder a precisão devido a conversão em decimal

### <a name="directory-vs-file"></a>Diretório vs. o ficheiro
Escolha um único arquivo e lê-lo na preparação de dados. O tipo de ficheiro é analisado para determinar os parâmetros de padrão para a ligação do arquivo mostrado no ecrã seguinte.

Escolha um diretório ou um conjunto de arquivos dentro de um diretório (o Seletor de ficheiros é seleção múltipla). Com qualquer uma das abordagens, os ficheiros são lidos na como um fluxo de dados individual e são acrescentados entre si, com cabeçalhos removidos se for necessário.

Os tipos de ficheiro suportados são:
- Delimitado por (. csv,. tsv, txt, etc.)
- Largura fixa
- Texto sem formatação
- Ficheiro JSON

### <a name="csv-file"></a>Ficheiro CSV
Ler um arquivo de separados por vírgulas de armazenamento.

#### <a name="options"></a>Opções
- Separador
- Comentário
- Cabeçalhos
- Símbolo decimal
- Codificação do ficheiro
- Linhas a ignorar

### <a name="tsv-file"></a>Ficheiro TSV
Ler um ficheiro de valores separador separados do armazenamento.

#### <a name="options"></a>Opções
- Comentário
- Cabeçalhos
- Codificação do ficheiro
- Linhas a ignorar

### <a name="excel-xlsxlsx"></a>Excel (.xls/.xlsx)
Leia uma folha de um ficheiro Excel ao mesmo tempo, especificando o nome da folha de cálculo ou número.

#### <a name="options"></a>Opções
- Nome da folha ou um número
- Cabeçalhos
- Linhas a ignorar

### <a name="json-file"></a>Ficheiro JSON
Leia um ficheiro JSON no armazenamento. O ficheiro é "aplanado" na leitura.

#### <a name="options"></a>Opções
- Nenhuma

### <a name="parquet"></a>Parquet
Leia um conjunto de dados no Parquet, optar por um único ficheiro ou pasta.

Parquet como um formato pode ter várias formas no armazenamento. Para conjuntos de dados mais pequenos, um ficheiro único .parquet, às vezes, é utilizado. Várias bibliotecas de Python suportam ler ou escrever para ficheiros de .parquet único. Por enquanto, preparação de dados do Azure Machine Learning baseia-se a biblioteca de PyArrow Python para ler o Parquet durante o uso interativo local. Ele oferece suporte a arquivos .parquet único (desde que foram escritas como tal e não como parte de um conjunto de dados maior), bem como o Parquet conjuntos de dados.

Um conjunto de dados Parquet é uma coleção de mais de um ficheiro de .parquet, cada um representando uma partição menor de um conjunto de dados maior. Conjuntos de dados geralmente estão contidos numa pasta e são o formato de saída padrão parquet para plataformas, como o Spark e do Hive.

>[!NOTE]
>Quando estiver lendo dados Parquet numa pasta com vários arquivos de .parquet, é mais seguro selecionar o diretório para leitura e o **conjunto de dados de Parquet** opção. Isso torna PyArrow ler a pasta inteira em vez dos ficheiros individuais. Isso garante o suporte para ler mais complicadas formas de armazenar o Parquet em disco, como a criação de partições de pasta.

A execução de escalamento horizontal baseia-se no Parquet do Spark capacidades de leitura e suporta ficheiros únicos, bem como as pastas, semelhantes ao uso interativo local.

#### <a name="options"></a>Opções
- Conjunto de dados de parquet. Esta opção determina se a preparação de dados do Azure Machine Learning expande-se um determinado diretório e tenta ler cada arquivo individualmente (o modo não selecionado), ou se ele trata o diretório como o conjunto de dados inteiro (o modo selecionado). Com o modo selecionado, PyArrow escolhe a melhor forma de interpretar os ficheiros.


## <a name="locations"></a>Localizações
### <a name="local"></a>Local
Um disco rígido local ou uma localização de armazenamento de rede mapeadas.

### <a name="sql-server"></a>SQL Server
Servidor SQL no local, ou a base de dados SQL do Azure.

### <a name="azure-blob-storage"></a>Armazenamento de Blobs do Azure
Blob storage do Azure, que requer uma subscrição do Azure.

