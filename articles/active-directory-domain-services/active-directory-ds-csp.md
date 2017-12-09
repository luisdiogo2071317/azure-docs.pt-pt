---
title: "Fornecedores de soluções de nuvem de serviços de domínio do Azure Active Directory do Azure | Microsoft Docs"
description: "Serviços de domínio do Azure Active Directory para fornecedores de soluções em nuvem do Azure."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: 313c4853b0f585921739779bb764c50036c9ac8a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Serviços de domínio do Azure Active Directory (AD) para fornecedores de soluções em nuvem do Azure (CSP)
Este artigo explica como pode utilizar os serviços de domínio do Azure AD numa subscrição do Azure CSP.

## <a name="overview-of-azure-csp"></a>Descrição geral do CSP do Azure
CSP do Azure é um programa para a Microsoft Partners e fornece um canal de licença para vários serviços de nuvem da Microsoft. Azure CSP permite que parceiros gerir vendas, possui a relação de faturação, fornecer suporte técnico e faturação e ponto único do cliente de contacto. Além disso, o CSP do Azure fornece um conjunto completo de ferramentas, incluindo um portal self-service e que acompanha APIs. Estas ferramentas ativar parceiros CSP facilmente aprovisionar e gerir recursos do Azure e fornecem a faturação de clientes e as suas subscrições.

O [portal de centro de parceiros](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) age como um ponto de entrada para todos os parceiros de CSP do Azure. Fornece capacidades de gestão de cliente avançado, processamento automatizado e muito mais. Parceiros CSP do Azure podem utilizar as capacidades de centro de parceiros, utilizando uma IU baseada na web ou utilizando o PowerShell e várias chamadas de API.

O diagrama seguinte ilustra como o modelo CSP funciona num nível elevado. Contoso tem um diretório do Azure AD Active Directory. Têm uma parceria com um CSP, que implementa e gere os recursos na respetiva subscrição do Azure CSP. Contoso pode também ter regulares (diretas) subscrições do Azure, que são cobradas diretamente para Contoso.

![Descrição geral do modelo de CSP](./media/csp/csp_model_overview.png)

Inquilino do parceiro CSP tem três grupos de agente especial - Admin agentes, agentes de suporte técnico e os agentes de vendas. O grupo de agentes de Admin é atribuído à função de administrador inquilino no diretório do Azure AD da Contoso. Como resultado, os utilizadores que pertençam ao grupo de agentes do administrador do parceiro CSP tem privilégios de administrador inquilino no diretório do Azure AD da Contoso. Quando aprovisiona de parceiro CSP uma subscrição do Azure CSP para Contoso, o respetivo grupo de agentes de admin é atribuída à função de proprietário dessa subscrição. Como resultado, os agentes de administração do parceiro CSP tem os privilégios necessários para aprovisionar os recursos do Azure como máquinas virtuais, redes virtuais e serviços de domínio do Azure AD em nome Contoso.

Para obter mais informações, consulte o [descrição geral do Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Vantagens da utilização dos serviços de domínio do Azure AD numa subscrição do Azure CSP
Serviços de domínio do AD do Azure fornece serviços compatíveis do Windows Server AD no Azure, tais como LDAP, a autenticação de Kerberos/NTLM, associação a um domínio, política de grupo e DNS. Ao longo do decades, muitas aplicações incorporadas para funcionar com o AD utilizando estas capacidades. Vários fornecedores de software independentes (ISVs) tem criado e implementar aplicações no local dos seus clientes. Estas aplicações são onerous para suportar uma vez que o que requer, muitas vezes, o acesso aos diferentes ambientes em que estas aplicações são implementadas. Com as subscrições do Azure CSP, que tem uma alternativa mais simples com a escala e a flexibilidade do Azure.

Serviços de domínio do Azure AD suporta agora a subscrições do Azure CSP. Agora pode implementar a aplicação numa subscrição do Azure CSP associada ao diretório do seu cliente do Azure AD. Como resultado, os seus empregados (pessoal de suporte) podem gerir, administrar e as máquinas virtuais em que a aplicação é implementada utilizando credenciais empresariais da sua organização de serviço. Além disso, pode aprovisionar um domínio gerido dos serviços de domínio do Azure AD para o diretório do seu cliente do Azure AD. A aplicação está ligada ao domínio gerido do seu cliente. Por conseguinte, as capacidades na sua aplicação que se baseiam em Kerberos/NTLM, LDAP, ou o [System.DirectoryServices API](https://msdn.microsoft.com/library/system.directoryservices) funcionam de forma totalmente integrada no diretório do seu cliente. Os seus clientes finais bastante beneficiam de consumir a sua aplicação como um serviço, sem necessidade de se preocupar com a manutenção da infraestrutura que a aplicação é implementada no.

Todos os faturação para consumir nessa subscrição, incluindo os serviços de domínio do Azure AD, é-lhe cobrada novamente para os recursos do Azure. Manter o controlo total sobre a relação com o cliente quando se trata de suporte de venda, de faturação, técnico etc. Com a flexibilidade da plataforma Azure CSP, uma equipa pequena de suporte agentes pode servir muitos esses clientes que tenham as instâncias da aplicação implementada.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>Modelos de implementação de CSP para serviços de domínio do Azure AD
Existem duas formas em que pode utilizar os serviços de domínio do Azure AD com uma subscrição do Azure CSP. Escolha um correto com base em considerações de segurança e na simplicidade que tem os seus clientes.

### <a name="direct-deployment-model"></a>Modelo de implementação direta
Este modelo de implementação, os serviços de domínio do Azure AD está ativada dentro de uma rede virtual que pertence à subscrição do Azure CSP. Os agentes de administração do parceiro CSP tem os seguintes privilégios:
* Privilégios de administrador global no diretório do Azure AD do cliente.
* Privilégios de proprietário da subscrição na subscrição do Azure CSP.

![Modelo de implementação direta](./media/csp/csp_direct_deployment_model.png)

Neste modelo de implementação, os agentes de administração do fornecedor CSP podem administrar as identidades do cliente. Estes agentes de administrador tem a capacidade de aprovisionar novos utilizadores, grupos, adicionar aplicações dentro do diretório do Azure AD do cliente etc. Este modelo de implementação poderá ser adequado para organizações mais pequenas que não têm um administrador de identidade dedicado ou preferir para o parceiro CSP para administrar as identidades em nome daqueles.


### <a name="peered-deployment-model"></a>Modelo de implementação em modo de peering
Este modelo de implementação, os serviços de domínio do Azure AD está ativada dentro de uma rede virtual que pertencem ao cliente - ou seja, uma subscrição do Azure direta paga pelo cliente. O parceiro CSP, em seguida, pode implementar aplicações dentro de uma rede virtual que pertencem à subscrição do cliente CSP. Em seguida, podem ser ligadas a redes virtuais utilizando o peering de rede virtual do Azure. Como resultado, as cargas de trabalho/aplicações implementadas pelo parceiro de CSP na subscrição do Azure CSP pode ligar ao domínio gerido do cliente aprovisionado na subscrição do Azure de direta do cliente.

![Modelo de implementação em modo de peering](./media/csp/csp_peered_deployment_model.png)

Este modelo de implementação fornece uma separação de privilégios e permite que os agentes de suporte técnico do parceiro CSP administrar a subscrição do Azure e implementar e gerir recursos dentro da mesma. No entanto, os agentes de suporte técnico do parceiro CSP não precisa de ter privilégios de administrador global no diretório do Azure AD do cliente. Os administradores de identidade do cliente podem continuar a gerir identidades para a sua organização.

Este modelo de implementação poderá ser adequado para cenários em que um ISV e estiver (fabricante independente de software) fornece um alojada versão da respetiva aplicação no local, que também tem de ligar para o cliente do AD.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>Administração dos serviços de domínio do Azure AD domínios em subscrições de CSP geridos
Aplicam as seguintes considerações importantes ao administrar um domínio gerido numa subscrição do Azure CSP:

* **Os agentes de administração do CSP podem aprovisionar um domínio gerido utilizando as respetivas credenciais:** dos serviços de domínio do Azure AD suporta subscrições do Azure CSP. Por conseguinte, os utilizadores que pertencem ao grupo de agentes do administrador de um parceiro CSP podem aprovisionar um novo domínio gerido dos serviços de domínio do Azure AD.

* **Os CSPs podem implementar scripts de criação de novos domínios geridos para os seus clientes com o PowerShell:** consulte [como ativar o domínio do Azure AD para serviços com o PowerShell](active-directory-ds-enable-using-powershell.md) para obter mais detalhes.

* **Agentes de administração do CSP não é possível executar tarefas de gestão em curso no domínio gerido utilizando as suas credenciais:** utilizadores de administração do CSP não é possível executar tarefas de rotina de gestão no domínio gerido utilizando as suas credenciais. Estes utilizadores são externos ao diretório do Azure AD do cliente e as respetivas credenciais não estão disponíveis no diretório do Azure AD do cliente. Por conseguinte, os serviços de domínio do Azure AD não tem acesso para os hashes de palavra-passe do Kerberos e NTLM para estes utilizadores. Como resultado, esses utilizadores não podem ser autenticados nos domínios geridos de serviços de domínio do Azure AD.

  > [!WARNING]
  > **Tem de criar uma conta de utilizador no diretório do cliente para efetuar tarefas de administração em curso no domínio gerido.**
  > Não é possível iniciar sessão no domínio gerido utilizando as credenciais de um utilizador de admin CSP. Utilize as credenciais de uma conta de utilizador que pertencem ao diretório do Azure AD do cliente para o fazer. Precisa destas credenciais para tarefas como associar máquinas virtuais para o domínio gerido, administrar o DNS, administrar etc de política de grupo.
  >

* **A conta de utilizador criada para administração em curso tem de ser adicionada ao grupo de 'AAD DC administradores':** grupo 'AAD DC administradores' tem privilégios para efetuar determinadas tarefas de administração delegada no domínio gerido. Estas tarefas incluem a configuração de DNS, criação de unidades organizacionais, administrar a política de grupo etc. Para um parceiro CSP efetuar essas tarefas um domínio gerido, uma conta de utilizador tem de ser criado dentro do diretório do Azure AD do cliente. As credenciais para esta conta tem de ser partilhadas com agentes de administração do parceiro CSP. Além disso, esta conta de utilizador tem de ser adicionada ao grupo de 'Administradores de DC do AAD' para permitir que as tarefas de configuração no domínio gerido para ser efetuada utilizando esta conta de utilizador.


## <a name="next-steps"></a>Passos seguintes
* [Inscrever no programa Azure CSP](https://partnercenter.microsoft.com/partner/programs) e iniciar a criação da empresa através do Azure CSP.
* Reveja a lista de [serviços do Azure disponíveis no Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).
* [Ativar o Azure AD Domain Services com o PowerShell](active-directory-ds-enable-using-powershell.md)
* [Começar com os serviços de domínio do Azure AD](active-directory-ds-getting-started.md)
