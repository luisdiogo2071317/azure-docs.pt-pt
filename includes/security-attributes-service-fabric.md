---
author: msmbaldwin
ms.service: service-fabric
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 179d87a0c1af587148f1b5ffa2cad8085ef0886f
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306855"
---
## <a name="preventative"></a>Preventivas

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Encriptação inativa:<ul><li>Encriptação do lado do servidor</li><li>Encriptação do lado do servidor com chaves geridas pelo cliente</li><li>Outros recursos de criptografia (por exemplo, o lado do cliente, são sempre encriptados, etc.)</ul>| Sim | O cliente possui o cluster e o conjunto de dimensionamento de máquina virtual (VM) do cluster é criado no. Encriptação de disco do Azure pode ser ativada no conjunto de dimensionamento VM. |
| Encriptação em trânsito:<ul><li>Express route encriptação</li><li>Na encriptação de Vnet</li><li>Encriptação de VNet a VNet</ul>| Sim |  |
| A manipulação de chave de encriptação (CMK, BYOK, etc.)| Sim | O cliente possui o cluster e o conjunto de dimensionamento de máquina virtual (VM) do cluster é criado no. Encriptação de disco do Azure pode ser ativada no conjunto de dimensionamento VM. |
| Encriptação de nível de coluna (Serviços de dados do Azure)| N/A |  |
| Chamadas de API encriptadas| Sim | Chamadas de API do Service Fabric são feitas através do Gestor de recursos do Azure. Um válido JSON web tokens (JWT) é necessário. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Notas |
|---|---|--|
| Suporte de ponto final de serviço| Sim |  |
| suporte de Injeção de vNET| Sim |  |
| Isolamento de rede / suporte de firewall| Sim | Utilizar grupos de segurança de rede (NSG). |
| Suporte para protocolo de túnel forçado | Sim | Redes do Azure fornecem um túnel forçado. |

## <a name="detection"></a>Deteção

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Monitorização de suporte (do Log analytics, o App insights etc) do Azure| Sim | Com suporte e suporte de terceiros de monitorização do Azure. |

## <a name="iam-support"></a>Suporte IAM

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Gestão de acesso - autenticação| Sim | A autenticação é através do Azure Active Directory. |
| Gestão de acesso - autorização| Sim | Gestão identidades e acessos (IAM) para chamadas via SFRP. Chamadas diretamente para o ponto de final de cluster suporta duas funções: Utilizador e administrador. O cliente pode mapear as APIs para cada função. |


## <a name="audit-trail"></a>Registo de Auditoria

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Planear a gestão/controlo e auditoria do registo| Sim | Todas as operações do painel de controle executadas através de processos de auditoria e aprovações. |
| Registo e auditoria do plano de dados| N/A | Cliente é o proprietário do cluster.  |

## <a name="configuration-management"></a>Gestão da Configuração

| Atributo de segurança | Sim/Não | Notas|
|---|---|--|
| Suporte de gestão de configuração (controle de versão da configuração, etc)| Sim | A configuração do serviço é uma versão e implementados com o Azure implementar. O código (aplicação ou tempo de execução) tem a mesma versão de através da compilação do Azure.
 |
