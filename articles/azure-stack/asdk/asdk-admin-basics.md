---
title: Noções básicas de Kit de desenvolvimento de pilha do Azure | Microsoft Docs
description: Descreve como efetuar tarefas de administração básica para o Azure pilha Development Kit (ASDK).
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
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: fa2ee4df9a0a28c08237f30fcf6a4bac50c21100
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849513"
---
# <a name="asdk-administration-basics"></a>Noções básicas da administração de ASDK 
Existem várias coisas que precisa de saber se estiver familiarizado com a administração de Kit de desenvolvimento de pilha do Azure (ASDK). Esta orientação fornece uma descrição geral da sua função como um operador de pilha do Azure no ambiente de avaliação e como garantir que os utilizadores de teste pode tornar-se produtivos rapidamente.

Em primeiro lugar, deve rever o [que é o Kit de desenvolvimento de pilha do Azure?](asdk-what-is.md) artigo para se certificar de que compreende o objetivo de ASDK e respetivos limitações. Deve utilizar o kit de desenvolvimento como uma "sandbox", onde pode avaliar pilha do Azure para desenvolver e testar as suas aplicações num ambiente de não produção. 

Como o Azure, Azure pilha innovates rapidamente, pelo que irá Lançamos regularmente novas compilações do ASDK. No entanto, não é possível atualizar o ASDK como o pode fazer as implementações de sistemas de pilha do Azure integrado. Por isso, se pretender mover para a compilação mais recente, tem de completamente [Reimplementar a ASDK](asdk-redeploy.md). Não é possível aplicar os pacotes de atualização. Este processo demora tempo, mas a vantagem é que pode experimentar funcionalidades mais recentes logo que ficarem disponíveis. 

## <a name="what-account-should-i-use"></a>Que conta devo utilizar?
Existem algumas considerações de conta que deve ter em mente quando gerir pilha do Azure. Especialmente nas implementações utilizando o Windows Server Active Directory os serviços de Federação (AD FS), como o fornecedor de identidade em vez do Azure Active Directory (Azure AD). Aplicam as seguintes considerações de conta para sistemas de pilha do Azure integrado e implementações de ASDK:

|Conta|Azure AD|AD FS|
|-----|-----|-----|
|Administrador local (. \Administrator)|Administrador de anfitrião ASDK|Administrador de anfitrião ASDK|
|AzureStack\AzureStackAdmin|Administrador de anfitrião ASDK<br><br>Pode ser utilizado para iniciar sessão no portal de administração a pilha do Azure<br><br>Acesso para ver e administrar anéis de Service Fabric|Administrador de anfitrião ASDK<br><br>Sem acesso ao portal de administração da pilha do Azure<br><br>Acesso para ver e administrar anéis de Service Fabric<br><br>Já não é proprietário da subscrição de fornecedor predefinido (DPS)|
|AzureStack\CloudAdmin|Pode aceder e executar comandos permitidos o ponto final com privilégios|Pode aceder e executar comandos permitidos o ponto final com privilégios<br><br>Não pode iniciar sessão para o anfitrião ASDK<br><br>Proprietário da subscrição do fornecedor predefinida (DPS)|
|Administrador Global do Azure AD|Utilizado durante a instalação<br><br>Proprietário da subscrição do fornecedor predefinida (DPS)|Não aplicável|
|

## <a name="what-tools-do-i-use-to-manage"></a>As ferramentas de utilizar para gerir?
Pode utilizar o [Portal de administrador de pilha do Azure](https://adminportal.local.azurestack.external) ou o PowerShell para gerir a pilha do Azure. Conheça os conceitos básicos a forma mais fácil é através do portal. Se pretender utilizar o PowerShell, tem de instalar [PowerShell para o Azure pilha](asdk-post-deploy.md#install-azure-stack-powershell) e [transferir as ferramentas de pilha do Azure a partir do GitHub](asdk-post-deploy.md#download-the-azure-stack-tools).

Pilha do Azure utiliza o Azure Resource Manager, como o mecanismo de implementação, gestão e organização subjacente. Se gerir a pilha do Azure e ajudar a suportar os utilizadores, deve saber mais sobre o Azure Resource Manager. Pode saber mais o lendo o [introdução ao Azure Resource Manager documento](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Suas responsabilidades típicas
Os seus utilizadores pretendem utilizar os serviços. Perspetiva, a sua função principal é disponibilizar estes serviços aos mesmos. Utilizando o ASDK, pode saber quais os serviços para oferecer e como se os serviços disponível por [criar planos, ofertas e quotas](asdk-offer-services.md). Também terá de adicionar os itens do Marketplace, tais como imagens da máquina virtual. A forma mais fácil é [transferir itens do marketplace](asdk-marketplace-item.md) do Azure para a pilha do Azure.

> [!NOTE]
> Se pretender testar os planos, ofertas e serviços, deve utilizar o [portal de utilizador](https://portal.local.azurestack.external); não o [portal de administrador](https://adminportal.local.azurestack.external).

Além de fornecer serviços, tem de efetuar todas as os deveres regulares de um operador de pilha do Azure para manter o ASDK em execução. Estas tarifas contam incluir coisas como o seguinte:
- Adicione contas de utilizador para o Azure Active Directory (Azure AD) ou para implementações de serviços de Federação do Active Directory (AD FS).
- Atribuir funções de controlo (RBAC) (isto não é restringido a apenas os administradores) de acesso baseado em funções
- Infraestrutura de monitor do Estado de funcionamento
- Gerir recursos de rede e armazenamento
- Substituir o hardware do computador de anfitrião de kit de desenvolvimento com falhas 

## <a name="where-to-get-support"></a>Onde obter suporte
Para o kit de desenvolvimento, a única opção de suporte é fazer perguntas relacionadas com suporte no [fórum do Microsoft Azure pilha](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Se clicar no ícone de ajuda e suporte (ponto de interrogação) no canto superior direito do portal de administrador e, em seguida, clique em **novo pedido de suporte**, esta ação abre o site fóruns diretamente. Estes fóruns regularmente são monitorizados. 

> [!IMPORTANT]
> Porque o ASDK é um ambiente de avaliação, não há nenhum oficial de suporte oferecida através da Microsoft ao cliente suportar (CSS).

## <a name="next-steps"></a>Passos Seguintes
[Implementar o ASDK](asdk-install.md)

