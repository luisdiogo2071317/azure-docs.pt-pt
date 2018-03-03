---
title: Arquitetura de identidade para a pilha do Azure | Microsoft Docs
description: Saiba mais sobre a arquitetura de identidade que pode utilizar com a pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 7f2ec78da38f3c97fde810fb8fc965cfbb6fda08
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Arquitetura de identidade para a pilha do Azure
Antes de escolher um fornecedor de identidade para utilizar com a pilha do Azure, compreenda importantes diferenças entre as opções do Azure Active Directory (Azure AD) e o Active Directory Federated Services (AD FS). 

## <a name="capabilities-and-limitations"></a>Capacidades e limitações 
O fornecedor de identidade que escolher pode limitar as suas opções, incluindo suporte para vários inquilinos. 

  

|Cenário ou capacidade        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Ligado à internet     |Sim       |Opcional|
|Suporte para vários inquilinos     |Sim       |Não      |
|Sindicação do Marketplace       |Sim       |Sim - requer a utilização do [offline Marketplace sindicação](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) ferramenta.|
|Suporte para o Active Directory Authentication Library (ADAL) |Sim |Sim|
|Suporte para ferramentas de interface de linha de comandos (CLI) do Azure, o Visual Studio (VS) e o PowerShell  |Sim |Sim|
|Criar principais de serviço através do portal     |Sim |Não|
|Criar principais de serviço com certificados      |Sim |Sim|
|Criar principais de serviço com segredos (chaves)    |Sim |Não|
|As aplicações podem utilizar o serviço de gráfico           |Sim |Não|
|As aplicações podem utilizar o fornecedor de identidade para início de sessão |Sim |Sim - necessita de aplicações a federar com no local do AD FS. |

## <a name="topologies"></a>Topologias
As secções seguintes fornecem informações sobre topologias de identidade, que pode utilizar.

### <a name="azure-ad--single-tenant"></a>Azure AD – único inquilino 
Por predefinição, quando instala a pilha do Azure e utilizar o Azure AD, pilha do Azure utiliza uma topologia de inquilino único. 

Uma topologia de inquilino único é útil quando:
- Todos os utilizadores fazem parte do mesmo inquilino.
- Um fornecedor de serviços aloja uma instância de pilha do Azure para uma organização.  

![Topologia de pilha do Azure com uma topologia de inquilino único com o Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Com esta topologia:
- Pilha do Azure regista todas as aplicações e serviços para o mesmo Azure AD directory de inquilino. 
- Pilha do Azure efetua a autenticação apenas os utilizadores e aplicações a partir de nesse diretório, incluindo Tokens. 
- Identidades de utilizadores de inquilinos e administradores (os operadores da nuvem) estão no mesmo inquilino do diretório. 
- Para ativar um utilizador de outro diretório aceder a este ambiente de pilha do Azure, terá [convidar o utilizador como convidado](azure-stack-identity-overview.md#guest-users) para o diretório do inquilino.  

### <a name="azure-ad--multi-tenant"></a>Azure AD – multi-inquilino
Os operadores da nuvem podem configurar a pilha do Azure para permitir o acesso a aplicações por inquilinos de um ou mais organizações. Os utilizadores aceder às aplicações através do portal de utilizador. Nesta configuração, o portal de administração (utilizado pelo operador da nuvem) está limitado a utilizadores de um único diretório. 

Uma topologia de multi-inquilino é útil quando:
- Um fornecedor de serviços se pretende permitir que os utilizadores de várias organizações aceder a pilha do Azure.

![Topologia de pilha do Azure com uma topologia de multi-inquilino com o Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Com esta topologia:
- Acesso a recursos deve ser numa base por organização. 
- Os utilizadores da um organização não devem ser capazes de conceder acesso a recursos para os utilizadores que estão fora da sua organização.  
- Identidades para os administradores (os operadores da nuvem) podem ser de um inquilino do diretório separado que as identidades dos utilizadores. Esta separação fornece isolamento de conta ao nível do fornecedor de identidade. 
 
### <a name="ad-fs"></a>AD FS  
A topologia do AD FS é necessário quando uma das seguintes condições é verdadeira:
- Pilha do Azure não estabelecer ligação à Internet.
- Pilha do Azure pode ligar à Internet, mas optar por utilizar o AD FS para o fornecedor de identidade.
  
![Topologia de pilha do Azure utilizando o AD FS](media/azure-stack-identity-architecture/adfs.png)

Com esta topologia:
- Para suportar a utilização em produção, tem de integrar a instância de pilha do Azure AD FS incorporada com uma instância existente do AD FS efetuada pelo Active Directory (AD), através de uma fidedignidade de Federação. 
- Pode integrar o serviço do gráfico na pilha do Azure com o AD existente.  Também pode utilizar o OData com base em serviço Graph API que suporte APIs que são consistentes com a Azure AD Graph API.  

  Para interagir com o AD, a Graph API requer credenciais de utilizador do AD, que têm permissão só de leitura do AD. 
  - AD FS incorporado baseia-se no Server 2016. 
  - O AD FS e o AD devem basear-se no Server 2012 ou posterior.  
  
  Entre o AD e o AD FS incorporado, interações não estão restringidas a OpenID Connect e podem utilizar qualquer protocolo mutuamente suportado.  
  - Contas de utilizador são criadas e geridas no seu local AD.
  - Principais de serviço e registos de aplicações são geridos no AD incorporado.



## <a name="next-steps"></a>Passos Seguintes
- [Descrição geral de identidade](azure-stack-identity-overview.md)   
- [Integração do Centro de dados - identidade](azure-stack-integrate-identity.md)