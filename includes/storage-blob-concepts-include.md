---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/17/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7889fbc9373cbdfdfab891bf8b1cd610523c7032
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088096"
---
O Armazenamento de Blobs do Azure é a solução de armazenamento de objetos da Microsoft para a cloud. O Armazenamento de blobs está otimizado para armazenar grandes quantidades de dados não estruturados, como dados de texto ou binários.

O armazenamento de blobs é ideal para:

* Entrega de imagens ou documentos diretamente a um browser.
* Armazenamento de ficheiros para acesso distribuído.
* Transmissão de áudio e vídeo.
* Escrever nos ficheiros de registo.
* Armazenamento de dados de cópia de segurança e restauro, recuperação após desastre e arquivo.
* Armazenamento de dados para análise por um serviço no local ou alojado no Azure.

Os utilizadores ou aplicações de cliente podem aceder aos objetos de armazenamento de Blobs através de HTTP ou HTTPS&mdash;a partir de qualquer parte do mundo&mdash;através de URLs, da [API REST do Armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [CLI do Azure](https://docs.microsoft.com/cli/azure/storage) ou uma biblioteca de cliente do Armazenamento do Azure. As bibliotecas de clientes de armazenamento estão disponíveis para várias linguagens, incluindo [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) e [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Conceitos do serviço Blob

O armazenamento do blob expõe três recursos: a sua conta de armazenamento, os contentores na conta e os blobs num contentor. O diagrama seguinte mostra a relação entre estes recursos.

![Diagrama da arquitetura de armazenamento de Blob (objeto)](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Conta de armazenamento

Todo o acesso a objetos de dados no Armazenamento do Azure ocorre através de uma conta de armazenamento. Para mais informações, veja [Visão geral de conta de armazenamento do Azure](../articles/storage/common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Contentor

Um contentor organiza um conjunto de blobs, de forma semelhante a uma pasta num sistema de ficheiros. Todos os blobs residem num contentor. Uma conta de armazenamento pode incluir um número ilimitado de contentores, e um contentor pode armazenar um número ilimitado de blobs. 

  > [!NOTE]
  > O nome do contentor tem de ser em minúsculas.

### <a name="blob"></a>Blobs
 
O Armazenamento do Azure oferece três tipos de blobs: &mdash;blobs de blocos, blobs de acréscimo e [blobs de páginas](../articles/storage/blobs/storage-blob-pageblob-overview.md) (servem para ficheiros VHD).

* Os blobs de blocos armazenam texto e dados binários, até cerca de 4,7 TB. Os blobs de blocos são constituídos por blocos de dados que podem ser geridos individualmente.
* Os blobs de acréscimo são compostos por blocos, de forma semelhante aos blobs de blocos, mas estão otimizados para operações de acréscimo. Os blobs de acréscimo são ideais para cenários como os dados de registo a partir de máquinas virtuais.
* Os blobs de páginas armazenam ficheiros de acesso aleatório com até 8 TB de tamanho. Os blobs de páginas armazenam os ficheiros VHD que suportam as VMs.

Todos os blobs residem num contentor. Um contentor é semelhante a uma pasta num sistema de ficheiros. Ainda pode organizar os blobs em diretórios virtuais e navegá-los, como faria com um sistema de ficheiros. 

Pode haver ocasiões em que grandes conjuntos de dados e restrições de rede tornam o carregamento de dados para o armazenamento de Blobs por cabo irrealista. Pode utilizar o [Azure Data Box Disk](../articles/databox/data-box-disk-overview.md) para pedir discos de estado sólido (SSD) à Microsoft. Pode copiar os seus dados para esses discos e enviá-los de volta para a Microsoft para serem carregados no armazenamento de Blobs.

Se precisar de exportar grandes quantidades de dados a partir da sua conta de armazenamento, veja [Utilizar o serviço de Importação/Exportação do Microsoft Azure para transferir dados para o armazenamento de Blobs](../articles/storage/common/storage-import-export-service.md).
  
Para obter detalhes sobre os nomes dos contentores e dos blobs, veja [Naming and referencing containers, blobs, and metadata (Nomenclatura e referência de contentores, blobs e metadados)](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
