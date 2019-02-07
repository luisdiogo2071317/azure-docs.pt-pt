---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: aba09012bf2e9d2741f598280add8b599a6f6d1a
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/06/2019
ms.locfileid: "55807097"
---
## <a name="preventative"></a>Preventivas

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Encriptação inativa:<ul><li>Encriptação do lado do servidor</li><li>Encriptação do lado do servidor com chaves geridas pelo cliente</li><li>Outros recursos de criptografia (por exemplo, o lado do cliente, são sempre encriptados, etc.)</ul>| Sim | Todos os objetos são encriptados. |
| Encriptação em trânsito:<ul><li>Express route encriptação</li><li>Na encriptação de Vnet</li><li>Encriptação de VNet a VNet</ul>| Sim | Toda a comunicação é por meio de chamadas de API encriptadas |
| A manipulação de chave de encriptação (CMK, BYOK, etc.)| Sim | Cliente controla todas as chaves no seu Cofre de chaves. Quando as chaves de módulo (HSM) cópia de segurança de hardware são specifiecd, um HSM de 2 de nível de FIPS protege a chave, o certificado ou o segredo. |
| Encriptação de nível de coluna (Serviços de dados do Azure)| N/A |  |
| Chamadas de API encriptadas| Sim | Através de HTTPS. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Suporte de ponto final de serviço| Sim | Através de pontos finais de serviço de rede Virtual (Vnet). |
| suporte de Injeção de vNET| Não |  |
| Isolamento de rede / suporte de firewall| Sim | Usando as regras de firewall da Vnet. |
| Suporte para protocolo de túnel forçado | Não |  |

## <a name="detection"></a>Deteção

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Monitorização de suporte (do Log analytics, o App insights etc) do Azure| Sim | Utilizar o Log Analytics. |

## <a name="iam-support"></a>Suporte IAM

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Gestão de acesso - autenticação| Sim | A autenticação é através do Azure Active Directory. |
| Gestão de acesso - autorização| Sim | A utilizar a política de acesso do Cofre de chaves. |


## <a name="audit-trail"></a>Registo de Auditoria

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Gestão/controlo do plano de auditoria e Registro em log| Sim | Utilizar o Log Analytics. |
| Registo e auditoria do plano de dados| Sim | Utilizar o Log Analytics. |

## <a name="access-controls"></a>Controlos de acesso

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Controlos de acesso do plano de gestão/controlo | Sim | Controlo de Acesso Baseado em Funções (RBAC) do Azure Resource Manager |
| Controlos de acesso do plano de dados (em todos os níveis de serviço) | Sim | Política de acesso do Cofre de chaves |
