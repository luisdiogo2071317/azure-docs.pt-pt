---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 002/08/2019
ms.openlocfilehash: ce7d8628c28a4a202a05aeea60e71655b4b7f1a2
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984924"
---
Contentores de serviços cognitivos não estão licenciados para executar sem a ser ligado ao Azure para medição. Os clientes têm de ativar os contentores comunicar informações de faturação com o serviço de medição em todos os momentos. Contentores de serviços cognitivos não enviar dados de cliente (a expressão) para a Microsoft. O contentor de relatórios de utilização sobre a cada 10 a 15 minutos.

O `docker run` utiliza os argumentos a seguir para fins de faturação:

| Opção | Descrição |
|--------|-------------|
| `ApiKey` | A chave de API do recurso de serviço cognitivo utilizada para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para uma chave de API para o recurso aprovisionado especificado no `Billing`. |
| `Billing` | O ponto final do recurso de serviço cognitivo utilizado para controlar informações de faturas.<br/>O valor desta opção tem de ser definido para o ponto final do URI de um recurso do LUIS Azure aprovisionado.|
| `Eula` | Indica que aceite a licença para o contentor.<br/>O valor desta opção tem de ser definido `accept`. |

> [!IMPORTANT]
> Todas as três opções tem de ser especificadas com valores válidos ou não inicia o contentor.
