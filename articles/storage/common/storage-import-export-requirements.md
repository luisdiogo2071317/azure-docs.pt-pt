---
title: Requisitos para o serviço importar/exportar do Azure | Microsoft Docs
description: Compreenda os requisitos de hardware e software para o serviço importar/exportar do Azure.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/14/2018
ms.author: alkohli
ms.openlocfilehash: eb50846bd838597a6f1e9a0b6550595866e05edc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661015"
---
# <a name="azure-importexport-system-requirements"></a>Requisitos de sistema de importação/exportação do Azure

Este artigo descreve os requisitos importantes para o seu serviço de importação/exportação do Azure. Recomendamos que reveja as informações cuidadosamente antes de utilizar o serviço importar/exportar e, em seguida, se referem novamente à mesma conforme necessário durante a operação.

## <a name="supported-operating-systems"></a>Sistemas operativos suportados

Para preparar os discos rígidos com a ferramenta de WAImportExport, o seguinte **sistema operativo de 64 bits que suporta encriptação de unidade BitLocker** são suportados.


|Plataforma |Versão |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |



## <a name="supported-storage-accounts"></a>Contas de armazenamento suportadas

Serviço de importação/exportação do Azure suporta as seguintes contas de armazenamento do Azure.
- Clássica
- Contas de Armazenamento de blobs
- Contas de armazenamento do v1 de objetivo gerais. 

Cada tarefa pode ser utilizada para transferir dados de ou para apenas uma conta de armazenamento. Por outras palavras, uma tarefa de importação/exportação único não pode abranger várias várias contas de armazenamento. Para obter informações sobre como criar uma nova conta de armazenamento, consulte [como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> O serviço de exportação de importação do Azure não suporta contas do storage onde os [pontos finais de serviço de rede Virtual](../../virtual-network/virtual-network-service-endpoints-overview.md) funcionalidade foi ativada. 

## <a name="supported-storage-types"></a>Tipos de armazenamento suportadas

A seguinte lista de tipos de armazenamento é suportada com o serviço importar/exportar do Azure.


|Tarefa  |Armazenamento  |Suportadas  |Não suportado  |
|---------|---------|---------|---------|
|Importar     |  Armazenamento de Blobs do Azure. <br>Blobs de blocos, blobs de páginas suportados. <br> Ficheiros do Azure suportados.       |         |
|Exportar     |   Armazenamento de Blobs do Azure. <br>Os blobs de blocos, blobs de páginas e blobs de acréscimo suportados.       | Ficheiros do Azure não suportados.        |


## <a name="supported-hardware"></a>Hardware suportado 

Para o serviço importar/exportar do Azure, precisa de discos suportados e SATA conectores para copiar dados suportados.

### <a name="supported-disks"></a>Discos suportados

A lista seguinte de discos é suportada para utilização com o serviço de importação/exportação.


|Tipo de disco  |Tamanho  |Suportadas |Não suportado  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3.5"       |SATA II, SATA III         |HDD externo com o adaptador USB incorporada <br> Disco dentro as maiúsculas e minúsculas de um HDD externo         |


Pode ter uma tarefa de importação/exportação único:
- Um máximo de 10 HDD/SSD.
- Uma combinação de HDD/SSD de qualquer dimensão.

Grande número de unidades de serem distribuído por várias tarefas e não existe nenhum limites no número de tarefas que podem ser criadas. 

Para tarefas de importação, apenas o primeiro volume de dados na unidade foi processado. O volume de dados deve ser formatado com NTFS.

### <a name="supported-external-usb-adaptors"></a>Suportado adaptors USB externos

Segue-se uma lista de adaptors USB externos utilizados para copiar dados em HDDs internos. 
- 68UPSATAA de Anker - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Orico 6628SUS3-C-BK (6628 série)
- Thermaltake BlacX frequente troca SATA externo rígido unidade ancoragem estação (2.0 de USB & eSATA)


## <a name="next-steps"></a>Passos Seguintes

* [Configurar a ferramenta de WAImportExport](storage-import-export-tool-how-to.md)
* [Transferir dados com o utilitário de linha de comandos AzCopy](storage-use-azcopy.md)
* [Exemplo de API de REST de exportação de importação do Azure](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

