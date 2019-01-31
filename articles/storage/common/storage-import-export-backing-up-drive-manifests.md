---
title: Fazendo backup manifestos da unidade de importar/exportar do Azure | Documentos da Microsoft
description: Saiba como ter seus manifestos da unidade para o serviço de importação/exportação do Microsoft Azure backup automaticamente.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 61881508e18a2c7dbe1bc3be72d34423f862437a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473396"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>Fazendo backup unidade manifestos para tarefas de importação/exportação do Azure

Manifestos da unidade podem ser automaticamente copiados para blobs, definindo a `BackupDriveManifest` propriedade `true` no [colocar tarefa](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) ou [propriedades da tarefa de atualização](/rest/api/storageimportexport/jobs#Jobs_Update) operações de REST API. Por predefinição, os manifestos da unidade não são uma cópia de segurança. As cópias de segurança de manifesto de unidade são armazenadas como blobs de blocos num contentor na conta de armazenamento associado à tarefa. Por predefinição, é o nome do contentor `waimportexport`, mas pode especificar um nome diferente na `DiagnosticsPath` propriedade ao chamar os `Put Job` ou `Update Job Properties` operações. O blob de manifesto de cópia de segurança são denominados no seguinte formato: `waies/jobname_driveid_timestamp_manifest.xml`.

 Pode obter o URI dos manifestos da unidade de cópia de segurança para uma tarefa ao chamar o [Get Job de](/rest/api/storageimportexport/jobs#Jobs_Get) operação. O blob é devolvido o URI no `ManifestUri` propriedade para cada unidade.

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a API de REST do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
