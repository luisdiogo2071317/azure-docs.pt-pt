---
title: Arquitetura de Kit de desenvolvimento de pilha do Azure | Microsoft Docs
description: Descreve a arquitetura do Kit de desenvolvimento de pilha do Azure (ASDK).
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
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 68da3ac0eb135f5956dfea76e186d9c57beea79c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975868"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Arquitetura do Kit de desenvolvimento de pilha do Microsoft Azure
O Kit de desenvolvimento de pilha do Azure (ASDK) é uma implementação de nó único da pilha do Azure. Todos os componentes são instalados em máquinas virtuais em execução numa máquina de anfitrião único. 

## <a name="logical-architecture-diagram"></a>Diagrama de arquitetura lógica
O diagrama seguinte ilustra a arquitetura lógica do ASDK e os respetivos componentes.

![Arquitetura ASDK](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Funções de máquina virtual
O ASDK oferece serviços, utilizando as seguintes VMs alojadas no computador de anfitrião do kit de desenvolvimento:

| Nome | Descrição |
| ----- | ----- |
| **AzS-ACS01** | Serviços de armazenamento de pilha do Azure.|
| **AzS-ADFS01** | Serviços de Federação do Active Directory (ADFS).  |
| **AzS-BGPNAT01** | Contorno router e fornece capacidades NAT e VPN para a pilha do Azure. |
| **AzS-CA01** | Serviços de autoridade de certificados para serviços de função de pilha do Azure.|
| **AzS-DC01** | Active Directory, DNS e DHCP dos serviços de pilha do Microsoft Azure.|
| **AzS-ERCS01** | Consola da recuperação de emergência VM. |
| **AzS-GWY01** | Gateway de serviços, tais como ligações do VPN site a site para redes de inquilino.|
| **AzS-NC01** | Controlador de rede, que gere os serviços de rede de pilha do Azure.  |
| **AzS-SLB01** | O balanceamento de carga serviços multiplexer na pilha do Azure para os inquilinos e os serviços de infraestrutura de pilha do Azure.  |
| **AzS-SQL01** | Arquivo de dados internos para funções de infraestrutura de pilha do Azure.  |
| **AzS-WAS01** | Portal de administração de pilha do Azure e os serviços do Azure Resource Manager.|
| **AzS-WASP01**| Portal de utilizador (inquilino) de pilha do Azure e os serviços do Azure Resource Manager.|
| **AzS-XRP01** | Controlador de gestão de infraestrutura do Microsoft Azure pilha, incluindo os fornecedores de recursos de computação, rede e armazenamento.|


## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre as tarefas de administração de ASDK básicas](asdk-admin-basics.md)
