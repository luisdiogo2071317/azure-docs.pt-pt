---
title: Arquitetura de Kit de desenvolvimento do Azure Stack | Documentos da Microsoft
description: Descreve a arquitetura do Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 21c54e2e996bb987f7a27ac3e6333df6f74d6f4b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338629"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Arquitetura do Development Kit do Microsoft Azure Stack
O Azure Stack Development Kit (ASDK) é uma implementação de nó único do Azure Stack. Todos os componentes são instalados em máquinas virtuais em execução num único computador anfitrião. 

## <a name="logical-architecture-diagram"></a>Diagrama de arquitetura lógica
O diagrama seguinte ilustra a arquitetura lógica do ASDK e seus componentes.

![Arquitetura ASDK](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Funções de máquina virtual
O ASDK oferece serviços usando as seguintes VMs alojadas no computador de anfitrião do kit de desenvolvimento:

| Nome | Descrição |
| ----- | ----- |
| **AzS-ACS01** | Serviços de armazenamento do Azure Stack.|
| **AzS-ADFS01** | Serviços de Federação do Active Directory (ADFS).  |
| **AzS-BGPNAT01** | Router de borda e fornece capacidades VPN e de NAT para o Azure Stack. |
| **AzS-CA01** | Serviços de autoridade de certificados para serviços de função do Azure Stack.|
| **AzS-DC01** | Active Directory, DNS e DHCP dos serviços para o Microsoft Azure Stack.|
| **AzS-ERCS01** | Consola de recuperação de emergência VM. |
| **AzS-GWY01** | Serviços do Edge gateway tais como ligações de site-site VPN para redes de inquilino.|
| **AzS-NC01** | Controlador de rede, que gere os serviços de rede do Azure Stack.  |
| **AzS-SLB01** | O balanceamento de carga Multiplexador serviços no Azure Stack para os inquilinos e serviços de infraestrutura do Azure Stack.  |
| **AzS-SQL01** | Arquivo de dados internos para funções de infraestrutura do Azure Stack.  |
| **AzS-WAS01** | Portal de administrativo de pilha do Azure e serviços do Azure Resource Manager.|
| **AzS-WASP01**| Portal de utilizador (inquilino) do Azure Stack e serviços do Azure Resource Manager.|
| **AzS-XRP01** | Controlador de gestão de infraestrutura para o Microsoft Azure Stack, incluindo os fornecedores de recursos de computação, rede e armazenamento.|


## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre tarefas de administração de ASDK básicas](asdk-admin-basics.md)
