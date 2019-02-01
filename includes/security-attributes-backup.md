---
author: msmbaldwin
ms.service: backup
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 5a4cc3ea822c5613fc7a50b8e370d6846b683721
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513513"
---
## <a name="preventative"></a>Preventivas

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Encriptação inativa:<ul><li>Encriptação do lado do servidor</li><li>Encriptação do lado do servidor com chaves geridas pelo cliente</li><li>Outros recursos de criptografia (por exemplo, o lado do cliente, são sempre encriptados, etc.)</ul>| Sim | A utilizar a encriptação do serviço de armazenamento para as contas de armazenamento. |
| Encriptação em trânsito:<ul><li>Express route encriptação</li><li>Na encriptação de Vnet</li><li>Encriptação de VNet a VNet</ul>| Não | Através de HTTPS. |
| A manipulação de chave de encriptação (CMK, BYOK, etc.)| Não |  |
| Encriptação de nível de coluna (Serviços de dados do Azure)| Não |  |
| Chamadas de API encriptadas| Sim |  |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Suporte de ponto final de serviço| Não |  |
| suporte de Injeção de vNET| Não |  |
| Isolamento de rede / suporte de firewall| Sim | O túnel forçado é suportado para cópia de segurança VM. O túnel forçado não é suportado para cargas de trabalho em execução em VMs. |
| Suporte para protocolo de túnel forçado | Não |  |

## <a name="detection"></a>Deteção

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Monitorização de suporte (do Log analytics, o App insights etc) do Azure| Sim | O log Analytics é suportado através de registos de diagnóstico. Consulte a cópia de segurança do Monitor do Azure protegidos cargas de trabalho utilizar o Log Analytics (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) para obter mais informações. |

## <a name="iam-support"></a>Suporte IAM

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Gestão de acesso - autenticação| Sim | A autenticação é através do Azure Active Directory. |
| Gestão de acesso - autorização| Sim | Cliente criada e funções RBAC incorporadas são usadas. Consulte Use Role-Based o controlo de acesso para gerir pontos de recuperação de cópia de segurança do Azure (https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault) para obter mais informações. |


## <a name="audit-trail"></a>Registo de Auditoria

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Planear a gestão/controlo e auditoria do registo| Sim | Todas as ações de cliente acionada no portal do Azure são registadas nos registos de atividade. |
| Registo e auditoria do plano de dados| Não | Não é possível aceder diretamente ao plano de dados de cópia de segurança do Azure.  |

## <a name="configuration-management"></a>Gestão da Configuração

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Suporte de gestão de configuração (controle de versão da configuração, etc)| Sim|  |