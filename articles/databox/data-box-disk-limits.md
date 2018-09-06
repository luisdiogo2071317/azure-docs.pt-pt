---
title: Limites de disco do Azure Data Box | Documentos da Microsoft
description: Descreve os limites de sistema e os tamanhos recomendados para o disco do Microsoft Azure Data Box.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/04/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 3b26c5f9a6d75725fb102d89192e77988b295dea
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782030"
---
# <a name="azure-data-box-disk-limits-preview"></a>Limites de disco de caixa de dados do Azure (pré-visualização)


Considere estes limites, como implantar e operar a sua solução de disco do Microsoft Azure Data Box. 

> [!IMPORTANT] 
> Disco do Azure Data Box está em pré-visualização. Reveja os [termos de utilização para a pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução. 


## <a name="data-box-service-limits"></a>Limites do serviço de caixa de dados

 - Serviço do Data Box está disponível apenas nos EUA, UE, Canadá e Austrália em todas as regiões do Azure para a cloud pública do Azure.
 - Uma conta de armazenamento única é suportada com disco Data Box.

## <a name="data-box-disk-performance"></a>Desempenho de disco de caixa de dados

Nos testes com discos ligados por USB 3.0, o desempenho dos discos foi de 430 MB/s. A velocidade real pode variar consoante o tamanho do ficheiro utilizado. Para ficheiros mais pequenos, o desempenho poderá ser mais lento.

## <a name="azure-storage-limits"></a>Limites de armazenamento do Azure

Esta secção descreve os limites para o serviço de armazenamento do Azure e as convenções de nomenclatura necessárias para ficheiros do Azure, blobs de blocos do Azure e blobs de páginas do Azure, conforme aplicável para o serviço do Data Box. Reveja os limites de armazenamento com atenção e siga as recomendações apresentadas.

Para obter as informações mais recentes sobre os limites de serviço de armazenamento do Azure e práticas recomendadas para a nomenclatura de partilhas, contentores e ficheiros de mensagens em fila, aceda a:

- [Nomenclatura e referência para contentores](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Nomenclatura e referência de partilhas](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blobs de blocos e as convenções de blob de página](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Se existirem quaisquer ficheiros ou diretórios que excedem os limites de serviço de armazenamento do Azure ou não está em conformidade com as convenções de nomenclatura de ficheiros/BLOBs do Azure, em seguida, estes ficheiros ou diretórios não são ingeridos no armazenamento do Azure através do serviço do Data Box.

## <a name="data-upload-caveats"></a>Limitações de carregamento de dados

- Não copie dados diretamente para os discos. Copiar dados para pré-criada *BlockBlob* e *PageBlob* pastas.
- Uma pasta sob o *BlockBlob* e *PageBlob* é um contentor. Por exemplo, os contentores são criados como *BlockBlob/contentores* e *PageBlob/contentor*.
- Se tiver um objeto existente do Azure (por exemplo, um blob) na cloud com o mesmo nome que o objeto que está a ser copiado, disco Data Box irá substituir o ficheiro na cloud.
- Todos os ficheiros escritos na *BlockBlob* e *PageBlob* partilhas é carregado como um blob de blocos e BLOBs de páginas, respectivamente.
- Esvaziar qualquer hierarquia de diretório (sem quaisquer ficheiros) criada *BlockBlob* e *PageBlob* pastas não é carregado.
- Se houver algum erro ao carregar dados para o Azure, é criado um registo de erros na conta de armazenamento de destino. O caminho para este registo de erros está disponível no portal do quando o carregamento estiver concluído e pode rever o registo para tomar medidas corretivas. Não elimine dados da origem sem verificar os dados carregados.

## <a name="azure-storage-account-size-limits"></a>Limites de tamanho de conta de armazenamento do Azure

Aqui estão os limites no tamanho dos dados que são copiados para a conta de armazenamento. Certifique-se de que os dados carregados estão de acordo com estes limites. Para obter as informações mais atualizadas sobre estes limites, aceda a [metas de dimensionamento de armazenamento de Blobs do Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) e [alvos de dimensionamento de ficheiros do Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamanho dos dados copiados para a conta de armazenamento do Azure                      | Limite Predefinido          |
|---------------------------------------------------------------------|------------------------|
| Blob de página e BLOBs de blocos                                            | 500 TB por conta de armazenamento. <br> Isto inclui dados de todas as origens, incluindo disco Data Box.|


## <a name="azure-object-size-limits"></a>Limites de tamanho de objeto do Azure

Aqui estão os tamanhos dos objetos do Azure que podem ser gravados. Certifique-se de que todos os ficheiros que são carregados estão em conformidade com estes limites.

| Tipo de objeto do Azure | Limite Predefinido                                             |
|-------------------|-----------------------------------------------------------|
| Blob de Blocos        | ~ 8 TB                                                 |
| Blob de Página         | 1 TB <br> (Todos os ficheiros carregados no formato de Blob de páginas têm de ter 512 bytes alinhados (um múltiplo de integral), caso contrário o carregamento falha. <br> O VHD e VHDX são alinhadas de 512 bytes.) |


## <a name="azure-block-blob-and-page-blob-naming-conventions"></a>Blob de blocos do Azure e convenções de nomenclatura de BLOBs de páginas

| Entidade                                       | Convenções                                                                                                                                                                                                                                                                                                               |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Os nomes dos contentores de blob de blocos e BLOBs de páginas | Tem de ser um nome DNS válido é entre 3 e 63 carateres de comprimento. <br>  Tem de começar com uma letra ou um número. <br> Pode conter apenas letras minúsculas, números e hífen (-). <br> Cada hífen (-) tem de ser imediatamente precedido e seguido por uma letra ou um número. <br> Não são permitidos hífenes consecutivos em nomes. |
| Nomes de blobs para blob de blocos e blob de páginas      | Os nomes de blobs são sensíveis a maiúsculas e minúsculas e podem conter qualquer combinação de carateres. <br> Um nome de blob tem de ter entre 1 e 1024 carateres de comprimento. <br> Os carateres de URL reservados devem ser escritos corretamente. <br>O número de segmentos de linha que inclui o nome do blob não pode exceder 254. Um segmento de linha é a cadeia de carateres entre os carateres delimitadores consecutivos (por exemplo, uma barra "/") que corresponde ao nome de um diretório virtual. |
