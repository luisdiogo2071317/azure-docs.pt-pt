---
title: Gerir atualizações na descrição geral do Azure Stack | Documentos da Microsoft
description: Saiba mais sobre a gestão de atualizações para os sistemas integrados do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/14/2019
ms.author: mabrigg
ms.openlocfilehash: ca09427de2f18612146ff03e4eef705006823ca8
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302617"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Gerir atualizações na descrição geral do Azure Stack

*Aplica-se a: Sistemas integrados do Azure Stack*

Pacotes de atualização da Microsoft para o Azure Stack, normalmente, os sistemas integrados versão em todo a quarta terça-feira de cada mês. Pedir ao OEM sobre seu processo de notificação específicos para garantir que as notificações de atualização atingir a sua organização. Também pode verificar nesta biblioteca de documentação sob **descrição geral** > **notas de versão** para obter informações sobre as versões que estão em suporte Active Directory. 

Cada versão das atualizações de software da Microsoft é fornecido como um pacote de atualização única. Como um operador do Azure Stack, pode importar, instalação e monitorizar o progresso da instalação destas atualizações de pacotes a partir do portal do administrador. 

O fornecedor do hardware de fabricante de equipamento original (OEM) também irá lançar atualizações, como atualizações de controladores e firmware. Embora estas atualizações são entregues como pacotes separados pelo seu fornecedor de hardware de OEM, que são importados, instalaram e geridos os pacotes de atualização de forma mesmo da Microsoft, pacotes de atualização são importados, instalados e geridos.

Para manter seu sistema em suporte, deve manter atualizado para um nível de versão específica do Azure Stack. Certifique-se de que revê os [política de manutenção do Azure Stack](azure-stack-servicing-policy.md).

> [!NOTE]
> Não pode aplicar os pacotes de atualização do Azure Stack Development Kit do Azure Stack. Os pacotes de atualização foram concebidos para os sistemas integrados. Para obter informações, consulte [Reimplementar o ASDK](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>O fornecedor de recursos de atualização

O Azure Stack inclui um provedor de recursos de atualização que orquestra a aplicação das atualizações de software da Microsoft. Este fornecedor de recursos assegura que as atualizações são aplicadas em todos os anfitriões físicos, aplicações do Service Fabric e tempos de execução e todas as máquinas de virtuais de infraestrutura e seus serviços associados.

Como instalar atualizações, pode ver o estado de alto nível como destinos de processo de atualização vários subsistemas no Azure Stack (por exemplo, anfitriões físicos e máquinas de virtuais de infraestrutura).

## <a name="plan-for-updates"></a>Plano para atualizações

Recomendamos vivamente que notifique os utilizadores de quaisquer operações de manutenção e que agende as janelas de manutenção normal durante o horário não comercial se possível. Operações de manutenção podem afetar a cargas de trabalho inquilinas e operações do portal.


- Antes de iniciar a instalação desta atualização, execute [AzureStack teste](azure-stack-diagnostic-test.md) com os parâmetros seguintes para validar o status do seu Azure Stack e resolver quaisquer problemas operacionais detectados, incluindo todos os avisos e falhas. Também rever alertas ativos e resolver qualquer um que requerem uma ação.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

## <a name="using-the-update-tile-to-manage-updates"></a>Mosaico utilização, o atualização para gerir atualizações
Gerir as atualizações a partir do portal do administrador. Como um operador do Azure Stack, pode utilizar o atualizar mosaico no dashboard para:

- ver informações importantes como a versão atual.
- instalar atualizações e monitorizar o progresso.
- rever o histórico de atualização para as atualizações instaladas anteriormente.
 
## <a name="determine-the-current-version"></a>Determinar a versão atual

O atualizar mosaico mostra a versão atual do Azure Stack. Pode obter o mosaico de atualização, utilizando qualquer um dos seguintes métodos no portal do administrador:

- No dashboard, ver a versão atual no **atualização** mosaico.
 
   ![Atualizações de mosaico no dashboard predefinido](./media/azure-stack-updates/image1.png)
 
- Sobre o **gestão da região** mosaico, clique no nome da região. Ver a versão atual no **atualização** mosaico.

## <a name="next-steps"></a>Passos Seguintes

- [O Azure Stack a política de manutenção](azure-stack-servicing-policy.md) 
- [Gestão da região no Azure Stack](azure-stack-region-management.md)     


