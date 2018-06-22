---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: craigshoemaker
ms.service: azure-policy
ms.topic: include
ms.date: 05/18/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: e6a0ded137162328fd446b65ddb4a15fa6f1db88
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313885"
---
## <a name="deleting-personal-information"></a>A eliminar informações pessoais

[!INCLUDE [gdpr-intro-sentence.md](gdpr-intro-sentence.md)]

As informações pessoais são relevantes para a importação/exportação de serviço (através do portal e a API) durante a importação e exportação operações. Os dados utilizados durante estes processos incluem:

- Nome do contacto
- Número de telefone
- Email
- Morada
- Cidade
- Código zip/postal
- Estado
- Distrito/país/região
- ID de unidade
- Número de conta operadora
- Número de controlo de envio

Quando é criada uma tarefa de importação/exportação, os utilizadores forneçam informações de contacto e um endereço de envio. As informações pessoais são armazenadas em até duas localizações diferentes: a tarefa e, opcionalmente, nas definições do portais. Informações pessoais apenas são armazenadas nas definições do portal, se selecionar a caixa de verificação com etiqueta, **guardar operadora e devolver o endereço como a predefinição** durante o *devolver informações de envio* secção do processo de exportação.

Informações de contacto pessoais podem ser eliminadas das seguintes formas:

- Dados guardados com a tarefa foi eliminados com a tarefa. Os utilizadores podem eliminar manualmente as tarefas e as tarefas concluídas são eliminadas automaticamente após 90 dias. Pode eliminar manualmente as tarefas através da API REST ou o portal do Azure. Para eliminar a tarefa no portal do Azure, vá para a tarefa importar/exportar e clique em *eliminar* na barra de comando. Para obter detalhes sobre como eliminar uma tarefa de importação/exportação através da REST API, consulte [eliminar uma tarefa de importação/exportação](../articles/storage/common/storage-import-export-cancelling-and-deleting-jobs.md).

- As informações de contacto guardadas nas definições de portais podem ser removidas, eliminando as definições do portal. Pode eliminar as definições do portal, seguindo estes passos:
  - Inicie sessão no [portal do Azure](https://portal.azure.com).
  - Clique em de *definições* ícone ![ícone de definições do Azure](media/storage-import-export-delete-personal-info/azure-settings-icon.png)
  - Clique em *exportar todas as definições* (para guardar as definições atuais para um `.json` ficheiro).
  - Clique em *eliminar todas as definições e dashboards privadas* para eliminar todas as definições, incluindo informações de contacto guardadas.

Para obter mais informações, consulte a política do Microsoft Privacy ao [Centro de confiança](https://www.microsoft.com/trustcenter)