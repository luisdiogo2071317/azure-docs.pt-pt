---
title: Os tipos de ficheiro no Azure Stack | Documentos da Microsoft
description: Reveja os tipos de quota diferentes disponíveis para serviços e recursos no Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: 2e884164347239838d08fbbc1616ed54ffc4ff24
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358740"
---
# <a name="quota-types-in-azure-stack"></a>Tipos de ficheiro no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

[Quotas](azure-stack-plan-offer-quota-overview.md#plans) definem os limites de recursos que uma subscrição de utilizador pode aprovisionar ou consumir. Por exemplo, uma quota pode permitir que um usuário criar até cinco VMs. Cada recurso pode ter seus próprios tipos de quotas.

## <a name="compute-quota-types"></a>Tipos de quota de computação 
| **Tipo** | **Default value** (Valor predefinido) | **Descrição** |
| --- | --- | --- |
| Número máx. de máquinas virtuais | 20 | O número máximo de máquinas virtuais que uma subscrição pode criar nesta localização. |
| Número máximo de núcleos de máquina virtual | 50 | O número máximo de núcleos que uma subscrição pode criar nesta localização (por exemplo, uma VM A3 tem quatro núcleos). |
| Número máximo de número de conjuntos de disponibilidade | 10 | O número máximo de conjuntos de disponibilidade que podem ser criadas nesta localização. |
| Define o número máximo de dimensionamento de máquinas virtuais | 20 | O número máximo de conjuntos de dimensionamento de máquinas virtuais que podem ser criadas nesta localização. |

## <a name="storage-quota-types"></a>Tipos de quota de armazenamento 
| **Item** | **Default value** (Valor predefinido) | **Descrição** |
| --- | --- | --- |
| Capacidade máxima (GB) |500 |Capacidade de armazenamento total que pode ser utilizada por uma subscrição nesta localização. |
| Número total de contas de armazenamento |20 |O número máximo de contas de armazenamento que uma subscrição pode criar nesta localização. |

> [!NOTE]  
> Pode demorar até duas horas antes de uma quota de armazenamento é imposta.


## <a name="network-quota-types"></a>Tipos de quota de rede
| **Item** | **Default value** (Valor predefinido) | **Descrição** |
| --- | --- | --- |
| Máx. IPs públicos |50 |O número máximo de IPs públicos que uma subscrição pode criar nesta localização. |
| Redes virtuais do máx. |50 |O número máximo de redes virtuais que uma subscrição pode criar nesta localização. |
| Gateways de rede virtual máx. |1 |O número máximo de gateways de rede virtual (Gateways de VPN) que uma subscrição pode criar nesta localização. |
| Máximo de ligações de rede |2 |O número máximo de ligações de rede (ponto a ponto ou site a site) que uma subscrição pode criar em todos os gateways de rede virtual nesta localização. |
| Balanceadores de carga máx. |50 |O número máximo de balanceadores de carga que uma subscrição pode criar nesta localização. |
| NICs máximos |100 |O número máximo de interfaces de rede que uma subscrição pode criar nesta localização. |
| Grupos de segurança de rede máx. |50 |O número máximo de grupos de segurança de rede que uma subscrição pode criar nesta localização. |

## <a name="view-an-existing-quota"></a>Ver uma quota existente
1. Encontrar no dashboard do portal de administração do padrão, o **fornecedores de recursos** mosaico.
2. Selecione o serviço com a quota de que pretende ver, assim **computação** ou **armazenamento**.
3. Selecione **Quotas**e, em seguida, selecione a quota de que pretende visualizar.


## <a name="edit-a-quota"></a>Editar uma quota  
Pode optar por editar a configuração original de uma quota em vez de [utilizar um plano de suplemento](create-add-on-plan.md). Ao editar uma quota, a nova configuração automaticamente aplica-se globalmente a todos os planos que utilizam esse quota e todas as assinaturas existentes que utilizam esses planos. A edição de uma quota é diferente de quando utiliza um plano de suplemento para fornecer uma quota modificada, o que um utilizador optar por subscrever. 

### <a name="to-edit-a-quota"></a>Para editar uma quota  
1. Encontrar no dashboard do portal de administração do padrão, o **fornecedores de recursos** mosaico.
2. Selecione o serviço com a quota de que pretende modificar, como **computação**, **rede**, ou **armazenamento**.
3. Em seguida, selecione **Quotas**e, em seguida, selecione a quota de que pretende alterar.
4. Sobre o **definir as quotas** painel, edite os valores e, em seguida, selecione **guardar**. 

Os novos valores para a quota aplicam-se globalmente a todos os planos que utilizam a quota modificada e a todas as subscrições existentes que utilizam esses planos. 



## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais sobre os planos, ofertas e quotas.](azure-stack-plan-offer-quota-overview.md)
- [Crie quotas durante a criação de um plano.](azure-stack-create-plan.md)
