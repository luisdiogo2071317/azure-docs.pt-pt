---
title: "Decisões de implementação ligado do Azure para a pilha do Azure integrado sistemas | Microsoft Docs"
description: "Determine as decisões de implementações de vários nós ligado do Azure de pilha do Azure de planeamento de implementação."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: c1a3b2107abdc3ef19a314616518c494687d81bf
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Decisões sobre a pilha do Azure de planeamento de implementação ligada do Azure integrado sistemas
Depois de tiver decidido [como irá integrar pilha do Azure para o seu ambiente de nuvem híbrida](azure-stack-deployment-decisions.md), em seguida, pode finalizar as suas decisões de implementação de pilha do Azure.

Implementar pilha do Azure ligado ao Azure significa que pode ter o Azure Active Directory (Azure AD) ou serviços de Federação do Active Directory (AD FS) para o arquivo de identidade. Também pode escolher entre o modelo de faturação: pay-como-utiliza ou com base na capacidade. Uma implementação ligada é a opção predefinida, porque permite aos clientes obter o maior valor fora do Azure pilha, especialmente para cenários de nuvem híbridos que envolvam Azure e Azure pilha. 

## <a name="choose-an-identity-store"></a>Selecionar um arquivo de identidade
Com uma implementação ligada, pode escolher entre o Azure AD ou AD FS para o arquivo de identidade. Uma implementação de desligado, sem conectividade internet, pode apenas utilizar o AD FS.

A opção de arquivo de identidade não tem efeito em máquinas virtuais de inquilino (VMs). VMs de inquilino podem escolher que pretendem ligar ao dependendo de como irá ser configurados do arquivo de identidade: do Azure AD, Windows Server Active Directory associados a um domínio, grupo de trabalho, etc. Este não está relacionado com a decisão de fornecedor de identidade de pilha do Azure. 

Por exemplo, se implementar o inquilino de IaaS VMs no início da pilha do Azure e pretende associar um domínio do Active Directory empresarial e utilizar contas a partir daí, pode fazer isto ainda. Não são necessários para utilizar o arquivo de identidade do Azure AD, que selecione aqui dessas contas.

### <a name="azure-ad-identity-store"></a>Arquivo de identidade do Azure AD
Quando utilizar o Azure AD para o arquivo de identidade requer duas contas do Azure AD: uma conta de administrador global e uma conta de faturação. Estas contas podem ser as mesmas contas ou contas diferentes. Durante a com a mesma conta de utilizador poderá ser útil se tiver um número limitado de contas do Azure e mais simples, necessidades comerciais podem sugerir utilizando duas contas:

1. **Conta de administrador global** (apenas necessário para implementações ligadas). Esta é uma conta do Azure que é utilizada para criar aplicações e principais de serviço para os serviços de infraestrutura de pilha do Azure no Azure Active Directory. Esta conta tem de ter permissões de administrador de diretório para o diretório que o sistema de pilha do Azure será implementado em. Ele irá tornar-se "operador de cloud" Administrador Global para o Azure AD de inquilino e será utilizado: 
    - Para aprovisionar e delegar principais de serviço para todos os serviços de pilha do Azure tem de interagir com o Azure Active Directory e do Graph API e aplicações. 
    - Como a conta de administrador de serviço. Este é o proprietário da subscrição de fornecedor predefinido (que é possível alterar mais tarde). Pode iniciar sessão no portal de administração do Azure pilha com esta conta e pode utilizá-la para criar ofertas e planos, defina quotas e efetuar outras administrativas funções na pilha do Azure.
2. **Conta de faturação** (necessário para ambos ligado e desligado implementações). Esta conta do Azure é utilizada para estabelecer a relação de faturação entre o sistema de pilha do Azure integrado e o back-end de comércio do Azure. Esta é a conta que será faturada por taxas de pilha do Azure. Esta conta também será utilizada para sindicação marketplace e outros cenários híbridos. 

### <a name="ad-fs-identity-store"></a>ARQUIVO DE IDENTIDADE DO AD FS
Escolha esta opção se pretender utilizar o seu próprio arquivo de identidade, tais como o Active Directory empresarial, para as contas de administrador de serviço.  

## <a name="choose-a-billing-model"></a>Escolha um modelo de faturação
Pode escolher uma **Pay-como-utiliza** ou **capacidade** modelo de faturação. Implementações de modelo de faturação pay-como-utiliza tem de ser capazes de utilização de relatórios através de uma ligação para o Azure, pelo menos, uma vez a cada 30 dias, por conseguinte, se a conectividade será não está disponível, o modelo de faturação capacidade é a única opção. 

### <a name="pay-as-you-use"></a>Pay-como-utiliza
Com o modelo de faturação Pay-como-utiliza, a utilização é cobrada a uma subscrição do Azure. Paga apenas quando utiliza os serviços de pilha do Azure. Se este é o modelo que opte por utilizar, precisará de uma subscrição do Azure e o ID de conta associados a essa subscrição (por exemplo, serviceadmin@contoso.onmicrosoft.com). EA, CSP e CSL subscrições são suportadas. Relatórios de utilização é configurado durante [registo Azure pilha](azure-stack-registration.md).

> [!NOTE]
> Na maioria dos casos, os clientes empresariais utilizará subscrições EA e fornecedores de serviços irá utilizar o CSP ou CSL subscrições.

Se pretender utilizar uma subscrição do CSP, reveja a tabela abaixo para identificar a subscrição do CSP a utilizar, como a abordagem correta depende do cenário exato do CSP:

|Cenário|Opções de domínio e de subscrição|
|-----|-----|
|É um direta ou Indireta parceiro de CSP e irá funcionar a pilha do Azure|Utilize uma subscrição de CSL (camada de serviço comum).|
|É um direta ou Indireta parceiro de CSP e irá funcionar a pilha do Azure|Centro de parceiros, criar um inquilino do Azure AD com um nome descritivo, por exemplo <your organization>CSPAdmin e uma subscrição do Azure CSP associados à mesma.|
|São um revendedor de CSP Indireta e irá funcionar a pilha do Azure|Peça ao seu fornecedor de CSP Indireta para criar, utilizando o Centro de parceiros, um inquilino do Azure AD para a sua organização e uma subscrição do Azure CSP associados à mesma.|

### <a name="capacity-based-billing"></a>Capacidade de faturação baseada em
Se optar por utilizar o modelo de faturação de capacidade, tem de comprar um Azure pilha capacidade planear SKU com base na capacidade do seu sistema. Terá de saber o número de núcleos físicos na pilha do Azure para comprar a quantidade correta. 

Faturação capacidade requer um Enterprise Agreement (EA) subscrição do Azure para o registo. O motivo é que o registo configura sindicação, que requer uma subscrição do Azure. A subscrição não está a ser utilizada para a utilização de pilha do Azure.

## <a name="learn-more"></a>Saiba mais
- Para obter informações sobre casos de utilização, aquisição, parceiros e fornecedores de hardware do OEM, consulte o [Azure pilha](https://azure.microsoft.com/overview/azure-stack/) página de produto.
- Para obter informações sobre o plano e a georreplicação disponibilidade pilha do Azure integrados sistemas, consulte o documento técnico: [pilha do Azure: uma extensão do Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Para saber mais sobre o Microsoft Azure pilha empacotamento e preços [transferir. o PDF](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 
