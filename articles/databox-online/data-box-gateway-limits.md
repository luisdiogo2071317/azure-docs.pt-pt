---
title: Limita o Gateway de caixa de dados do Azure | Documentos da Microsoft
description: Descreve os limites de sistema e os tamanhos recomendados para o Gateway de caixa de dados do Microsoft Azure.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/03/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 946c045407b150a923d0067776bf80cbfab54c67
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268043"
---
# <a name="azure-data-box-gateway-limits-preview"></a>Limites do Gateway de caixa de dados do Azure (pré-visualização)


Considere estes limites, como implantar e operar a sua solução de Gateway de caixa de dados do Microsoft Azure. 

> [!IMPORTANT] 
> O Data Box Gateway está em Pré-visualização. Reveja os [termos de utilização para a pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução. 


## <a name="data-box-gateway-service-limits"></a>Limites do serviço de Gateway de caixa de dados

- Nesta versão, o serviço está disponível apenas em determinadas regiões nos EUA, UE e Ásia-Pacífico. Para obter mais informações, aceda a [Disponibilidade de região](#data-box-gateway-overview#region-availability). A conta de armazenamento deve ser fisicamente mais próxima da região em que o dispositivo implementado (pode ser diferente da geo de serviço).
- Mover um recurso de Gateway de caixa de dados para um grupo diferente de subscrição ou recurso não é suportada. Para obter mais detalhes, aceda a [mover recursos para um novo grupo de recursos ou subscrição](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="data-box-gateway-device-limits"></a>Limites do dispositivo de Gateway de caixa de dados

A tabela seguinte descreve os limites para o dispositivo de Gateway de caixa de dados.

| Descrição | Valor |
|---|---|
|Não. de arquivos por dispositivo |100 milhões <br> Limite é de aproximadamente 25 milhões de ficheiros para todas as 2 TB de espaço em disco com o limite máximo em 100 milhões |
|Não. de partilhas por dispositivo |24 |
|Tamanho máximo do ficheiro escrito para uma partilha|Para um dispositivo virtual de 2 TB, o tamanho máximo é 500 GB. <br> O tamanho máximo do ficheiro aumenta com o tamanho de disco de dados a proporção anterior até atingir um máximo de 5 TB. |

## <a name="azure-storage-limits"></a>Limites de armazenamento do Azure

Esta secção descreve os limites para o serviço de armazenamento do Azure e as convenções de nomenclatura necessárias para ficheiros do Azure, blobs de blocos do Azure e blobs de páginas do Azure, conforme aplicável para o serviço de borda de caixa de Gateway/dados de caixa de dados. Reveja os limites de armazenamento com atenção e siga as recomendações apresentadas.

Para obter as informações mais recentes sobre os limites de serviço de armazenamento do Azure e práticas recomendadas para a nomenclatura de partilhas, contentores e ficheiros de mensagens em fila, aceda a:

- [Nomenclatura e referência para contentores](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Naming and referencing shares](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata) (Nomenclatura e referência a partilhas)
- [Blobs de blocos e as convenções de blob de página](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Se existirem quaisquer ficheiros ou diretórios que excedem os limites de serviço de armazenamento do Azure ou não está em conformidade com as convenções de nomenclatura de ficheiros/BLOBs do Azure, em seguida, estes ficheiros ou diretórios não são ingeridos no armazenamento do Azure através do serviço de borda de caixa de Gateway/dados de caixa de dados.

## <a name="data-upload-caveats"></a>Limitações de carregamento de dados

As limitações seguintes aplicam-se aos dados quando são transmitidos para o Azure.

- Sugerimos que mais de um dispositivo não deve escrever o mesmo contentor.
- Se tiver um objeto do Azure existente (como um blob ou um arquivo) na cloud com o mesmo nome que o objeto que está a ser copiado, o dispositivo irá substituir o ficheiro na cloud. 
- Uma hierarquia de diretório vazio (sem quaisquer ficheiros), criada em pastas de partilha não é carregada para os contentores de Blobs.


## <a name="azure-storage-account-size-and-object-size-limits"></a>Limites de tamanho de tamanho de conta de armazenamento do Azure e o objeto

Aqui estão os limites no tamanho dos dados que são copiados para a conta de armazenamento. Certifique-se de que os dados carregados estão de acordo com estes limites. Para obter as informações mais atualizadas sobre estes limites, aceda a [metas de dimensionamento de armazenamento de Blobs do Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) e [alvos de dimensionamento de ficheiros do Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamanho dos dados copiados para a conta de armazenamento do Azure                      | Limite Predefinido          |
|---------------------------------------------------------------------|------------------------|
| Blob de página e BLOBs de blocos                                            | 500 TB por conta de armazenamento|


## <a name="azure-object-size-limits"></a>Limites de tamanho de objeto do Azure

Aqui estão os tamanhos dos objetos do Azure que podem ser gravados. Certifique-se de que todos os ficheiros que são carregados estão em conformidade com estes limites.

| Tipo de objeto do Azure | Limite predefinido                                             |
|-------------------|-----------------------------------------------------------|
| Blob de Blocos        | ~ 8 TB                                                 |
| Blob de Página         | 1 TB <br> Todos os ficheiros carregados no formato de Blob de páginas têm de ter 512 bytes alinhados (um múltiplo de integral), caso contrário o carregamento falha. <br> O VHD e VHDX são alinhadas de 512 bytes. |


## <a name="next-steps"></a>Passos Seguintes

- [Preparar para implementar o Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
