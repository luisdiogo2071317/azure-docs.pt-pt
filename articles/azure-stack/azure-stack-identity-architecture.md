---
title: Arquitetura de identidade para a pilha do Azure | Microsoft Docs
description: Saiba mais sobre a arquitetura de identidade que pode utilizar com a pilha do Azure.
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
ms.date: 2/28/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a6da27740efd613b8a81ffa85092d6b00b3e47d8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Arquitetura de identidade para a pilha do Azure
Antes de escolher um fornecedor de identidade para utilizar com a pilha do Azure, compreenda as diferenças importantes entre as opções do Azure Active Directory (Azure AD) e serviços de Federação do Active Directory (AD FS). 

## <a name="capabilities-and-limitations"></a>Capacidades e limitações 
O fornecedor de identidade que escolher pode limitar as suas opções, incluindo suporte para vários inquilinos. 

  

|Cenário ou capacidade        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|Ligado à internet     |Sim       |Opcional|
|Suporte para vários inquilinos     |Sim       |Não      |
|Sindicação do Marketplace       |Sim       |Sim. Requer a utilização do [offline Marketplace sindicação](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) ferramenta.|
|Suporte para o Active Directory Authentication Library (ADAL) |Sim |Sim|
|Suporte para as ferramentas, como a CLI do Azure, o Visual Studio e o PowerShell  |Sim |Sim|
|Criar principais de serviço através do portal do Azure     |Sim |Não|
|Criar principais de serviço com certificados      |Sim |Sim|
|Criar principais de serviço com segredos (chaves)    |Sim |Não|
|As aplicações podem utilizar o serviço de gráfico           |Sim |Não|
|As aplicações podem utilizar o fornecedor de identidade para início de sessão |Sim |Sim. Requer que as aplicações a federar com no local instâncias do AD FS. |

## <a name="topologies"></a>Topologias
As secções seguintes discus as várias topologias de identidade que pode utilizar.

### <a name="azure-ad-single-tenant-topology"></a>Do Azure AD: topologia de único inquilino 
Por predefinição, quando instala a pilha do Azure e utilizar o Azure AD, pilha do Azure utiliza uma topologia de inquilino único. 

Uma topologia de inquilino único é útil quando:
- Todos os utilizadores fazem parte do mesmo inquilino.
- Um fornecedor de serviços aloja uma instância de pilha do Azure para uma organização. 

![Topologia de inquilino único de pilha do Azure com o Azure AD](media/azure-stack-identity-architecture/single-tenant.png)

Esta topologia inclui as seguintes características:
- Pilha do Azure regista todas as aplicações e serviços para o mesmo Azure AD directory de inquilino. 
- Pilha do Azure efetua a autenticação apenas os utilizadores e aplicações a partir de nesse diretório, incluindo tokens. 
- Identidades de utilizadores de inquilinos e administradores (os operadores da nuvem) estão no mesmo inquilino do diretório. 
- Para ativar um utilizador de outro diretório aceder a este ambiente de pilha do Azure, terá [convidar o utilizador como convidado](azure-stack-identity-overview.md#guest-users) para o diretório do inquilino. 

### <a name="azure-ad-multi-tenant-topology"></a>Do Azure AD: topologia de multi-inquilino
Os operadores da nuvem podem configurar a pilha do Azure para permitir o acesso a aplicações por inquilinos de um ou mais organizações. Os utilizadores aceder às aplicações através do portal de utilizador. Nesta configuração, o portal de administração (utilizado pelo operador da nuvem) está limitado a utilizadores de um único diretório. 

Uma topologia de multi-inquilino é útil quando:
- Um fornecedor de serviços se pretende permitir que os utilizadores de várias organizações aceder a pilha do Azure.

![Topologia de multi-inquilino de pilha do Azure com o Azure AD](media/azure-stack-identity-architecture/multi-tenant.png)

Esta topologia inclui as seguintes características:
- Acesso a recursos deve ser numa base por organização. 
- Os utilizadores de uma organização devem ser não é possível conceder acesso a recursos para os utilizadores que estão fora da sua organização. 
- Identidades para os administradores (os operadores da nuvem) podem ser de um inquilino do diretório separado das identidades dos utilizadores. Esta separação fornece isolamento de conta ao nível do fornecedor de identidade. 
 
### <a name="ad-fs"></a>AD FS  
A topologia do AD FS é necessário quando uma das seguintes condições é verdadeira:
- Pilha do Azure não estabelecer ligação à internet.
- Pilha do Azure pode ligar à internet, mas optar por utilizar o AD FS para o fornecedor de identidade.
  
![Topologia de pilha do Azure utilizando o AD FS](media/azure-stack-identity-architecture/adfs.png)

Esta topologia inclui as seguintes características:
- Para suportar a utilização desta topologia de produção, tem de integrar a instância de pilha do Azure AD FS incorporada com uma instância do AD FS existente que é copiado em segurança pelo Active Directory, através de uma fidedignidade de Federação. 
- Pode integrar o serviço do gráfico na pilha do Azure com a instância existente do Active Directory. Também pode utilizar o serviço baseado em OData Graph API que suporte APIs que são consistentes com a Azure AD Graph API. 

  Para interagir com a instância do Active Directory, a Graph API requer credenciais de utilizador da sua instância do Active Directory que tem permissões só de leitura. 
  - A instância do AD FS incorporada é baseada no Windows Server 2016. 
  - As instâncias do AD FS e o Active Directory tem de ser baseadas no Windows Server 2012 ou posterior. 
  
  Entre a instância do Active Directory e a instância do AD FS incorporada, interações não estão limitadas a OpenID Connect e podem utilizar qualquer protocolo mutuamente suportado. 
  - Contas de utilizador são criadas e geridas na sua instância do Active Directory no local.
  - Principais de serviço e registos de aplicações são geridos na instância do Active Directory incorporada.



## <a name="next-steps"></a>Passos Seguintes
- [Descrição geral de identidade](azure-stack-identity-overview.md)   
- [Integração do Centro de dados - identidade](azure-stack-integrate-identity.md)
