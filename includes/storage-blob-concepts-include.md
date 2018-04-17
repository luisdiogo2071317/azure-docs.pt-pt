---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 203f5a766c4c8a8f1e577f6be1e18d0f9ac95403
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
Armazenamento de Blobs do Azure é a solução de armazenamento de objeto da Microsoft para a nuvem. Armazenamento de Blobs está otimizado para armazenar as quantidades enormes de dados não estruturados, como texto ou dados binários.

O blob storage é ideal para:

* Que serve imagens ou documentos diretamente a um browser.
* Armazenamento de ficheiros para acesso distribuído.
* Transmissão em fluxo de áudio e vídeo.
* Escrever nos ficheiros de registo.
* Armazenamento de dados para cópia de segurança e restauro, recuperação após desastre e arquivo.
* Armazenamento de dados para análise no local ou serviço alojado no Azure.

Os objetos no Blob storage podem ser acedidos de qualquer local no mundo através de HTTP ou HTTPS. Os utilizadores ou aplicações de cliente podem aceder a blobs através de URLs, o [API de REST do Storage do Azure](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [CLI do Azure](https://docs.microsoft.com/cli/azure/storage), ou uma biblioteca de clientes do Storage do Azure. As bibliotecas de cliente de armazenamento estão disponíveis para vários idiomas, incluindo [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/en/latest/index.html), [PHP](http://azure.github.io/azure-storage-php/), e [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Conceitos do serviço Blob

O blob storage expõe três recursos: a conta de armazenamento, os contentores na conta e os blobs num contentor. O diagrama seguinte mostra a relação entre estes recursos.

![Diagrama da arquitetura de armazenamento de Blob (objeto)](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Conta de Armazenamento

Todo o acesso a objetos de dados no armazenamento do Azure ocorre através de uma conta de armazenamento. Para obter mais informações, consulte [contas do storage do Azure sobre](../articles/storage/common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Contentor

Um contentor organiza um conjunto de blobs, semelhantes a uma pasta no sistema de ficheiros. Todos os blobs residem num contentor. Uma conta do storage pode conter um número ilimitado de contentores e um contentor pode armazenar um número ilimitado de blobs. Tenha em atenção que o nome do contentor tem de ser em minúsculas.

### <a name="blob"></a>Blobs
 
Storage do Azure oferece três tipos de blobs – blobs de blocos, blobs, de acréscimo e [blobs de páginas](../articles/storage/blobs/storage-blob-pageblob-overview.md) (utilizado para ficheiros VHD).

* Armazenam blobs de blocos texto e os dados binários, até cerca de 4.7 TB. Os blobs de blocos são constituídos por blocos de dados que podem ser geridos individualmente.
* Acrescentar blobs são constituídos por blocos, como os blobs de blocos, mas estão otimizados para operações de acréscimo. Acrescentar blobs são ideais para cenários tais como dados de registo a partir de máquinas virtuais.
* Página arquivo de blobs acesso aleatório ficheiros até 8 TB de tamanho. Os blobs de páginas armazenam os ficheiros VHD que apoiam a VMs.

Todos os blobs residem num contentor. Um contentor é semelhante a uma pasta no sistema de ficheiros. Ainda pode organizar os blobs em diretórios virtuais e atravessá-los tal como faria com um sistema de ficheiros. 

Para conjuntos de dados muito grandes e em que as limitações de rede fazem com que carregar ou transferir dados para o Armazenamento de blobs de forma automática seja irrealista, pode enviar um conjunto de discos rígidos à Microsoft para importar ou exportar dados diretamente do datacenter. Para obter mais informações, consulte [utilizar o serviço de importação/exportação do Microsoft Azure para transferir dados para o Blob Storage](../articles/storage/common/storage-import-export-service.md).
  
Para obter detalhes sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata (Nomenclatura e Referência de Contentores, Blobs e Metadados)](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
