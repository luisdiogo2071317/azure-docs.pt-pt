---
title: Requisitos para o serviço importar/exportar do Azure | Documentos da Microsoft
description: Compreenda os requisitos de software e hardware para o serviço importar/exportar do Azure.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 10e8fb6ac5bcce278de3924ebd3a0d9f90392217
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528059"
---
# <a name="azure-importexport-system-requirements"></a>Requisitos de sistema de importação/exportação do Azure

Este artigo descreve os requisitos importantes para o seu serviço importar/exportar do Azure. Recomendamos que reveja as informações cuidadosamente antes de utilizar o serviço importar/exportar e, em seguida, regressar à mesma conforme necessário durante a operação.

## <a name="supported-operating-systems"></a>Sistemas operativos suportados

Para preparar as unidades de disco rígido usando a ferramenta de WAImportExport, o seguinte procedimento **SO de 64 bits que suportam encriptação de unidade BitLocker** são suportados.


|Plataforma |Versão |
|---------|---------|
|Windows     | O Windows 7 Enterprise, Windows 7 Ultimate <br> O Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |


## <a name="supported-storage-accounts"></a>Contas de armazenamento suportadas

O serviço importar/exportar do Azure suporta as seguintes [contas de armazenamento do Azure](storage-account-options.md).
- Contas de armazenamento gerais v1 de fins (implementações clássicas ou do Azure Resource Manager)
- Contas de Armazenamento de blobs
- Contas de armazenamento da v2 de fins gerais

Cada tarefa pode ser utilizada para transferir dados de ou para apenas uma conta de armazenamento. Em outras palavras, uma tarefa de importação/exportação única não pode abranger várias várias contas de armazenamento. Para obter informações sobre como criar uma nova conta de armazenamento, consulte [como criar uma conta de armazenamento](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> O serviço de exportação de importação do Azure não suporta contas de armazenamento onde o [pontos finais de serviço de rede Virtual](../../virtual-network/virtual-network-service-endpoints-overview.md) funcionalidade foi ativada. 

## <a name="supported-storage-types"></a>Tipos de armazenamento suportadas

A seguinte lista de tipos de armazenamento é suportada com o serviço importar/exportar do Azure.


|Tarefa  |Serviço de armazenamento |Suportadas  |Não suportado  |
|---------|---------|---------|---------|
|Importar     |  Armazenamento de Blobs do Azure <br><br> Armazenamento de ficheiros do Azure       | Blobs de página e Blobs de blocos suportados <br><br> Ficheiros suportados          |
|Exportar     |   Armazenamento de Blobs do Azure       | Os blobs de blocos, blobs de páginas e blobs de acréscimo suportados         | Ficheiros do Azure não suportados


## <a name="supported-hardware"></a>Hardware suportado 

Para o serviço importar/exportar do Azure, terá de discos suportados para copiar dados.

### <a name="supported-disks"></a>Discos suportados

A seguinte lista de discos é suportada para utilização com o serviço de importação/exportação.


|Tipo de disco  |Tamanho  |Suportadas |Não suportado  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3.5"       |SATA II, SATA III         |HDD externo com o adaptador interno de USB <br> Disco dentro as maiúsculas e minúsculas de um HDD externo         |


Uma tarefa de importação/exportação única pode ter:
- Um máximo de 10 HDD/SSDs.
- Uma combinação de HDD/SSD de qualquer tamanho.

Grande número de unidades de serem distribuído por várias tarefas e não sem limites no número de tarefas que podem ser criadas. Para tarefas de importação, apenas o primeiro volume de dados na unidade é processado. O volume de dados deve ser formatado com NTFS.

Quando preparar unidades de disco rígido e copiar os dados usando a ferramenta de WAImportExport, pode utilizar adaptadores USB externas. Mais prontas para utilização USB 3.0 ou posteriores adaptadores devem funcionar. 


## <a name="next-steps"></a>Passos Seguintes

* [Configurar a ferramenta de WAImportExport](storage-import-export-tool-how-to.md)
* [Transferir dados com o utilitário de linha de comandos AzCopy](storage-use-azcopy.md)
* [Exemplo de importar exportar a API de REST do Azure](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

