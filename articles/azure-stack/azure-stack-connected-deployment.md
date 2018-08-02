---
title: Sistemas integrados de decisões de implementação ligada do Azure para o Azure Stack | Documentos da Microsoft
description: Determine decisões para implementações de ligada ao Azure Stack Azure com vários nós de planejamento da implantação.
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
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: d64b834f1c6794976461c93d4ad1d05f8647e986
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414594"
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>A implementação do Azure ligada decisões para o Azure Stack de planeamento de sistemas integrados
Depois de decidir [como irá integrar o Azure Stack para seu ambiente de cloud híbrida](azure-stack-connection-models.md), em seguida, pode finalizar suas decisões de implementação do Azure Stack.

Implementação do Azure Stack, ligado ao Azure significa que pode ter o Azure Active Directory (Azure AD) ou os serviços de Federação do Active Directory (AD FS) para seu armazenamento de identidade. Também pode escolher entre o modelo de faturação: pay-as que use ou baseada em capacidade. Uma implementação ligada é a opção predefinida, uma vez que permite aos clientes obter o máximo valor fora do Azure Stack, particularmente para cenários de cloud híbrida que envolvem o Azure e o Azure Stack. 

## <a name="choose-an-identity-store"></a>Escolher um arquivo de identidade
Com uma implementação de ligado, pode escolher entre o Azure AD ou AD FS para seu armazenamento de identidade. Uma implementação de desligado, sem conectividade de internet, pode apenas utilizar o AD FS.

Sua escolha de arquivo de identidade não tem nenhuma relevância em máquinas de virtuais de inquilino (VMs). VMs do inquilino podem escolher qual querem ligar ao dependendo de como serão configurados de armazenamento de identidade: Azure AD, Windows Server Active Directory associados a um domínio, grupo de trabalho, etc. Isso está relacionada com a decisão de fornecedor de identidade do Azure Stack. 

Por exemplo, se implementar o inquilino de IaaS VMs com base no Azure Stack e desejá-los para associar um domínio do Active Directory empresarial e utilizar contas a partir daí, ainda pode fazer isso. Não tem de utilizar o armazenamento de identidade do Azure AD, que selecione aqui para essas contas.

### <a name="azure-ad-identity-store"></a>Armazenamento de identidade do Azure AD
Quando utilizar o Azure AD para o armazenamento de identidade requer duas contas do Azure AD: uma conta de administrador global e uma conta de cobrança. Estas contas podem ser as mesmas contas ou contas diferentes. Embora possa ser com a mesma conta de utilizador mais simples e útil se tiver um número limitado de contas do Azure, necessidades da sua empresa podem sugerir usando duas contas:

1. **Conta de administrador global** (apenas necessário para implementações ligadas). Esta é uma conta do Azure que é utilizada para criar principais de serviço para serviços de infraestrutura do Azure Stack e aplicações no Azure Active Directory. Esta conta tem de ter permissões de administrador do diretório para o diretório que será implementado em seu sistema do Azure Stack. Ela se tornará o operador"nuvem" Administrador Global do Azure AD de inquilino e será utilizado: 
    - Para aprovisionar e delegar principais de serviço para todos os serviços do Azure Stack que precisam interagir com o Azure Active Directory e a Graph API e aplicações. 
    - Como a conta de administrador de serviços. Este é o proprietário da subscrição do fornecedor do padrão (que é possível alterar mais tarde). Pode iniciar sessão no portal de administração do Azure Stack com esta conta e pode utilizá-lo para criar ofertas e planos, definir as quotas e realizar funções administrativas no Azure Stack.
2. **Conta de faturação** (obrigatório para ambos ligado e desligado implementações). Esta conta do Azure é utilizada para estabelecer a relação de faturação entre seu sistema integrado do Azure Stack e o back-end do Azure commerce. Esta é a conta que será cobrada as taxas do Azure Stack. Esta conta também vai ser utilizada pela sua oferta de itens do marketplace e outros cenários híbridos. 

### <a name="ad-fs-identity-store"></a>Armazenamento de identidade do AD FS
Escolha esta opção se pretender utilizar o seu próprio repositório de identidades, como o seu Active Directory empresarial, para as suas contas de administrador de serviços.  

## <a name="choose-a-billing-model"></a>Escolha um modelo de faturação
Pode escolher entre **Pay-as que use** ou o **capacidade** modelo de faturação. Implementações de modelos de faturação pay-as que use tem de ser capazes de utilização de relatório através de uma ligação para o Azure, pelo menos, uma vez a cada 30 dias. Por conseguinte, o modelo de faturação de pagamento-como-utiliza só está disponível para implementações ligadas.  

### <a name="pay-as-you-use"></a>Pay-as que use
Com o modelo de faturação de pagamento-como-utiliza, a utilização é cobrada a uma subscrição do Azure. Paga apenas quando usar os serviços do Azure Stack. Se esse é o modelo que opte por utilizar, terá de uma subscrição do Azure e o ID da conta associada a essa subscrição (por exemplo, serviceadmin@contoso.onmicrosoft.com). As subscrições do EA, o CSP e CSL são suportadas. Relatórios de utilização é configurada durante [registo do Azure Stack](azure-stack-registration.md).

> [!NOTE]
> Na maioria dos casos, os clientes empresariais utilizarão as subscrições do EA e fornecedores de serviços irá utilizar as subscrições de CSP ou CSL.

Se pretender utilizar uma subscrição do CSP, reveja a tabela a seguir para identificar a subscrição do CSP para utilizar, como a abordagem correta depende do cenário exato do CSP:

|Cenário|Opções de domínio e de subscrição|
|-----|-----|
|É um **direto de parceiro CSP** ou uma **fornecedor de CSP Indiretos**, e irá utilizar o Azure Stack|Utilize uma subscrição de CSL (camada de serviço comum).<br>     ou<br>Crie um inquilino do Azure AD com um nome descritivo no Centro de parceiros. Por exemplo &lt;sua organização > CSPAdmin com uma subscrição do Azure CSP associada a ele.|
|É um **revendedor de CSP Indiretos**, e irá utilizar o Azure Stack|Peça ao seu fornecedor de CSP Indiretos para criar um inquilino do Azure AD para a sua organização com uma subscrição do Azure CSP associada a ele usando o Centro de parceiros.|

### <a name="capacity-based-billing"></a>Capacidade de faturação com base
Se optar por utilizar o modelo de faturação de capacidade, tem de comprar um SKU do Azure Stack capacidade plano com base na capacidade do seu sistema. Terá de saber o número de núcleos físicos no seu Azure Stack para comprar a quantidade correta. 

Faturação capacidade requer um Enterprise Agreement (EA) uma subscrição do Azure para o registo. O motivo é que os registo configura a disponibilidade de itens no Marketplace, que requer uma subscrição do Azure. A subscrição não é utilizada para a utilização do Azure Stack.

## <a name="learn-more"></a>Saiba mais
- Para obter informações sobre casos de utilização, de compra, parceiros e fornecedores de hardware de OEM, consulte a [do Azure Stack](https://azure.microsoft.com/overview/azure-stack/) página do produto.
- Para obter informações sobre o plano e a disponibilidade geográfica para o Azure Stack, sistemas integrados, consulte o white paper: [do Azure Stack: uma extensão do Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Para saber mais sobre pacotes e preços do Microsoft Azure Stack [transferir o PDF](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Passos Seguintes
[Integração de rede do Centro de dados](azure-stack-network.md)