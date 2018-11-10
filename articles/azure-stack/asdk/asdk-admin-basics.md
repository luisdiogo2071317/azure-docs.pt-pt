---
title: Noções básicas do Kit de desenvolvimento do Azure Stack | Documentos da Microsoft
description: Descreve como realizar tarefas administrativas básicas para o Azure Stack Development Kit (ASDK).
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
ms.openlocfilehash: 66a2871e0c4b36959ccd8f08df5b6b7edd09f624
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227829"
---
# <a name="asdk-administration-basics"></a>Noções básicas de administração de ASDK 
Há várias coisas que precisa saber se estiver familiarizado com a administração do Azure Stack Development Kit (ASDK). Esta orientação fornece uma descrição geral da sua função como um operador do Azure Stack no ambiente de avaliação e como garantir que os utilizadores de teste pode tornar-se produtivos rapidamente.

Em primeiro lugar, deve rever o [o que é o Kit de desenvolvimento do Azure Stack?](asdk-what-is.md) artigo para se certificar de que compreende o objetivo do ASDK e suas limitações. Deve usar o kit de desenvolvimento como uma "área de segurança," onde pode avaliar o Azure Stack para desenvolver e testar as suas aplicações num ambiente de não produção. 

Como o Azure, Azure Stack innovates rapidamente, portanto, vamos regularmente lançar novas compilações do ASDK. No entanto, não é possível atualizar o ASDK como pode fazer as implementações de sistemas integrados do Azure Stack. Então, se pretender mover para a compilação mais recente, tem completamente [Reimplementar o ASDK](asdk-redeploy.md). Não é possível aplicar os pacotes de atualização. Este processo demora tempo, mas a vantagem é que pode experimentar as funcionalidades mais recentes tão logo que ficarem disponíveis. 

## <a name="what-account-should-i-use"></a>Que conta devo utilizar?
Existem algumas considerações de conta que deve estar atento ao gerir o Azure Stack. Especialmente em Implantações usando o Windows Server Active Directory Federation Services (AD FS), como o fornecedor de identidade em vez do Azure Active Directory (Azure AD). As seguintes considerações de conta aplicam-se a sistemas integrados do Azure Stack e implementações de ASDK:

|Conta|Azure AD|AD FS|
|-----|-----|-----|
|Administrador local (. \Administrator)|Administrador de anfitrião ASDK|Administrador de anfitrião ASDK|
|AzureStack\AzureStackAdmin|Administrador de anfitrião ASDK<br><br>Pode ser utilizado para iniciar sessão no portal de administração do Azure Stack<br><br>Acesso para ver e administrar anéis do Service Fabric|Administrador de anfitrião ASDK<br><br>Sem acesso ao portal de administração do Azure Stack<br><br>Acesso para ver e administrar anéis do Service Fabric<br><br>Já não é proprietário da subscrição de fornecedor predefinido (DPS)|
|AzureStack\CloudAdmin|Pode aceder e executar comandos permitidos no ponto de extremidade com privilégios|Pode aceder e executar comandos permitidos no ponto de extremidade com privilégios<br><br>Não pode iniciar sessão no anfitrião ASDK<br><br>Proprietário da subscrição do fornecedor padrão (DPS)|
|Administrador Global do Azure AD|Usados durante a instalação<br><br>Proprietário da subscrição do fornecedor padrão (DPS)|Não aplicável|
|

## <a name="what-tools-do-i-use-to-manage"></a>Que ferramentas posso utilizar para gerir?
Pode utilizar o [Portal do Azure Stack administrador](https://adminportal.local.azurestack.external) ou o PowerShell para gerir o Azure Stack. A forma mais fácil de aprender os conceitos básicos é através do portal. Se pretender utilizar o PowerShell, tem de instalar [PowerShell para o Azure Stack](asdk-post-deploy.md#install-azure-stack-powershell) e [transferir as ferramentas do Azure Stack a partir do GitHub](asdk-post-deploy.md#download-the-azure-stack-tools).

O Azure Stack utiliza o Azure Resource Manager como seu mecanismo de implantação, gerenciamento e organização subjacente. Se for para gerir o Azure Stack e ajudar a dar suporte aos usuários, deve aprender sobre o Azure Resource Manager. Pode saber mais, leia os [introdução ao documento técnico do Azure Resource Manager](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Suas responsabilidades típicas
Os seus utilizadores pretendem utilizar os serviços. Da sua perspectiva, sua função principal é disponibilizar esses serviços a eles. Usando o ASDK, pode saber quais os serviços para oferecer e como tornar os serviços disponíveis por [criação de planos, ofertas e quotas](asdk-offer-services.md). Também terá de adicionar os itens no Marketplace, como imagens de máquina virtual. A maneira mais fácil é [transferir itens do marketplace](asdk-marketplace-item.md) do Azure para o Azure Stack.

> [!NOTE]
> Se pretender testar seus planos, ofertas e serviços, deve utilizar o [portal de utilizador](https://portal.local.azurestack.external); não o [portal de administrador](https://adminportal.local.azurestack.external).

Além de fornecer serviços, tem de efetuar todas as tarefas regulares de um operador de pilha do Azure para manter o ASDK em execução. Estas tarefas incluem coisas como o seguinte:
- Adicione contas de utilizador para o Azure Active Directory (Azure AD) ou implementações de serviços de Federação do Active Directory (AD FS).
- Atribuir acesso baseado em funções funções (RBAC) do controle (isso não é restrito aos administradores apenas)
- Infraestrutura de monitor do Estado de funcionamento
- Gerir recursos de rede e armazenamento
- Substituir o hardware de computador de anfitrião de kit de desenvolvimento com falhas 

## <a name="where-to-get-support"></a>Onde obter suporte
Para o kit de desenvolvimento, é a única opção de suporte fazer perguntas relacionadas ao suporte no [fórum do Microsoft Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Se clicar no ícone de ajuda e suporte (ponto de interrogação) no canto superior direito do portal do administrador e, em seguida, clique em **novo pedido de suporte**, esta ação abre o site dos fóruns diretamente. Esses fóruns são monitorados regularmente. 

> [!IMPORTANT]
> Como o ASDK é um ambiente de avaliação, não existe nenhum oficial de suporte oferecido por meio da Microsoft dos clientes suportar (CSS).

## <a name="next-steps"></a>Passos Seguintes
[Implementar o ASDK](asdk-install.md)

