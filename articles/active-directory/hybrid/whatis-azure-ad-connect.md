---
title: O que é o Azure AD Connect e Connect Health. | Microsoft Docs
description: Descreve as ferramentas usadas para sincronizar e monitorizar o seu ambiente no local com o Azure AD.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2fbd6edb02dfc4080d7692e43324a5b3981091f9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109483"
---
# <a name="what-is-azure-ad-connect"></a>O que é o Azure AD Connect?

O Azure AD Connect é a ferramenta da Microsoft concebida para satisfazer e atingir os seus objetivos de identidade híbrida.  Proporciona as seguintes funcionalidades:
    
- [Sincronização de hash de palavra-passe](whatis-phs.md) -um método de início de sessão que sincroniza um hash de utilizadores no local palavras-passe do AD com o Azure AD.
- [Autenticação pass-through](how-to-connect-pta.md) -um método de início de sessão que permite aos utilizadores utilizar a mesma palavra-passe no local e na cloud, mas não requer a infraestrutura adicional de um ambiente federado.
- [Integração de Federação](how-to-connect-fed-whatis.md) -Federation é uma parte opcional do Azure AD Connect e pode ser utilizado para configurar um ambiente híbrido utilizando uma local infraestrutura do AD FS. Ele também fornece capacidades de gestão do AD FS, tais como implementações de servidor do AD FS adicionais e renovação de certificado.
- [Sincronização](how-to-connect-sync-whatis.md) - responsável pela criação de utilizadores, grupos e outros objetos.  Bem como a tornar-se de que informações de identidade para seus usuários locais e grupos é correspondência na nuvem.  Essa sincronização também inclui os hashes de palavra-passe.
-   [Monitorização do Estado de Funcionamento ](whatis-hybrid-identity-health.md) - o Azure AD Connect Health pode proporcionar uma monitorização robusta e uma localização central no portal do Azure para visualizar esta atividade. 


![O que é o Azure AD Connect](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>O que é o Azure AD Connect Health?

O Azure Active Directory (Azure AD) Connect Health fornece uma monitorização robusta de sua infraestrutura de identidade no local. Permite-lhe manter uma ligação fiável ao Office 365 e do Microsoft Online Services.  Este confiabilidade é conseguida ao fornecer capacidades de monitorização para os componentes de identidade chave. Além disso, torna os pontos de dados chave sobre estes componentes facilmente acessíveis.

As informações são apresentadas no [portal do Azure AD Connect Health](https://aka.ms/aadconnecthealth). Utilize o portal do Azure AD Connect Health para ver alertas, monitorização de desempenho, análise de utilização e outras informações. O Azure AD Connect Health ativa a lente única do estado de funcionamento dos componentes de identidade chave num único local.

![O que é o Azure AD Connect Health](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)

## <a name="why-use-azure-ad-connect"></a>Porquê utilizar o Azure AD Connect?
A integração dos diretórios no local com o Azure AD torna os utilizadores mais produtivos, ao fornecer-lhes uma identidade comum para acederem a recursos na nuvem e no local. Os utilizadores e as organizações podem tirar partido de:

* Os utilizadores podem utilizar uma identidade única para acederem às aplicações no local e aos serviços na nuvem, como o Office 365.
* Uma ferramenta única para proporcionar uma experiência fácil de implementação para sincronização e início de sessão.
* Fornece as capacidades mais recentes para os seus cenários. O Azure AD Connect substitui as versões anteriores das ferramentas de integração de identidade, como o DirSync e o Azure AD Sync. Para obter mais informações, consulte [Comparação das ferramentas de integração de diretórios de identidade híbrida](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Porquê utilizar o Azure AD Connect Health?
Quando com o Azure AD, os utilizadores sejam mais produtivos porque existe uma identidade comum para aceder aos recursos na cloud e no local. Garantir que o ambiente é confiável, para que os utilizadores podem aceder estes recursos, torna-se um desafio.  O Azure AD Connect Health ajuda a monitorizar e obter informações sobre a infraestrutura de identidade no local, que garante a confiabilidade de neste ambiente. É tão simples como instalar um agente em cada um dos servidores de identidade no local.


## <a name="next-steps"></a>Passos Seguintes

- [Hardware e pré-requisitos](how-to-connect-install-prerequisites.md) 
- [Definições rápidas](how-to-connect-install-express.md)
- [Definições personalizadas](how-to-connect-install-custom.md)
- [Instalar os agentes do Azure AD Connect Health](how-to-connect-health-agent-install.md) 
