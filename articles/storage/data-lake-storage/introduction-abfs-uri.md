---
title: Utilize a pré-visualização do Azure Data Lake armazenamento Gen2 URI
description: Utilize a pré-visualização do Azure Data Lake armazenamento Gen2 URI
services: storage
keywords: ''
author: jamesbak
ms.topic: article
ms.author: jamesbak
manager: jahogg
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a6130d8440b16e5a72c939fc07f6bf32c0946418
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114297"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>Utilize o Gen2 Lake armazenamento de dados do Azure URI

O [Hadoop Filesystem](http://www.aosabook.org/en/hdfs.html) controlador compatível com a pré-visualização do Azure Data Lake armazenamento Gen2 for conhecido pelo respetivo identificador de esquema `abfs` (sistema de ficheiros de Blob do Azure). Consistente com outros controladores de sistema de ficheiros do Hadoop, o controlador ABFS utiliza um formato URI para endereçar ficheiros e diretórios dentro de uma conta com capacidade de Gen2 de armazenamento do Data Lake.

## <a name="uri-syntax"></a>Sintaxe URI

A sintaxe URI para Gen2 de armazenamento do Data Lake está dependente de ou não a conta de armazenamento é configurada para ter Gen2 de armazenamento do Data Lake como sistema de ficheiros predefinido.

Se a conta com capacidade de Gen2 de armazenamento do Data Lake pretende endereço **não é** definido como o sistema de ficheiros durante a criação de conta, em seguida, a expressão compacta sintaxe URI é:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.widows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Identificador de esquema**: O `abfs` protocolo é utilizado como o identificador de esquema. Tem a opção para ligar com ou sem uma ligação do secure socket layer (SSL). Utilize `abfss` para estabelecer ligação com uma ligação de camada de socket segura.

2. **Sistema de ficheiros**: A localização principal que contém os ficheiros e pastas. Este é o mesmo como contentores no serviço Blobs Storage do Azure.

3. **Nome da conta**: O nome atribuído à sua conta de armazenamento durante a criação.

4. **Caminhos**: uma barra delimitada (`/`) representação da estrutura de diretório.

5. **Nome de ficheiro**: O nome do ficheiro individual. Este parâmetro é opcional se envolvido um diretório.

No entanto, se a conta que pretende endereço está definida como o sistema de ficheiros durante a criação de conta, em seguida, a expressão compacta sintaxe URI é:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Caminho**: uma barra delimitada (`/`) representação da estrutura de diretório.

2. **Nome de ficheiro**: O nome do ficheiro individual.


## <a name="next-steps"></a>Passos Seguintes

- [Utilizar Gen2 de armazenamento do Azure Data Lake com clusters do HDInsight do Azure](use-hdi-cluster.md)