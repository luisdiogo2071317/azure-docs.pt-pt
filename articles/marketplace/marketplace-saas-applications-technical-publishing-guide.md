---
title: "Técnica de aplicações SaaS do Azure Marketplace publicação guia"
description: "Guia passo a passo e listas de verificação de publicação para a publicação de aplicações de SaaS no Azure Marketplace"
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: 
author: BrentL-Collabera
manager: 
editor: BrentL-Collabera
ms.assetid: 
ms.service: marketplace
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: pabutler
ms.openlocfilehash: 64becc80192e69bd332d6657637c845acf93748b
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>Guia publicação technical aplicações SaaS

Bem-vindo ao técnica de aplicações de SaaS do Azure Marketplace publicação guia. Este guia foi concebido para ajudar candidato e publicadores existentes para listar as respetivas aplicações e serviços no Azure Marketplace utilizando as aplicações de SaaS oferta.  
Pretende utilizar aplicações SaaS oferta quando a sua solução será implementada na sua própria subscrição do Azure e os clientes irão iniciar sessão através de uma interface que conceber e gerir para testar a aplicação. Fazê-lo utilizando [Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) tirar partido do seu ambiente de avaliação. Por outras palavras, é uma versão de avaliação gratuita guiado de cliente, é alojado de parceiro. É fundamental para expor a sua solução de forma a que dá a oportunidade de experimentar a sua solução de forma independente para uma taxa ou não encargos-buyers de nuvem e para que este tipo de oferta fornece uma experiência de avaliação gratuita que corresponde à forma como os clientes procurar soluções de nuvem.  

Para obter uma descrição geral de todas as outras ofertas de Marketplace, consulte o [Marketplace publicador guia](https://aka.ms/sellerguide).

## <a name="saas-application-technical-guidance"></a>Orientações para o técnica de aplicações SaaS
Os requisitos técnicos para aplicações SaaS são simples. Os publicadores só são necessárias para ser integrado com o Azure AD a ser publicado.  A integração do Azure AD com aplicações está bem documento e a Microsoft fornece várias SDKs e recursos para realizar esta tarefa.  

Para começar, é recomendável que tiver uma subscrição dedicada para a publicação do Azure Marketplace, permitindo-lhe isolar o trabalho de outros iniciativas. Além disso, se não estiver já instalado, recomendamos que tem as ferramentas seguintes como parte do seu ambiente de desenvolvimento: 
- [CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)  
- [O Azure powerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-5.0.0)  
- [Ferramentas de programador do Azure (rever o que está disponível)](https://azure.microsoft.com/tools/)  
- [Visual Studio Code](https://code.visualstudio.com/)  

### <a name="resources"></a>Recursos
As listas seguintes fornecem ligações para os recursos do Azure AD melhor para começar: 

**Documentação**

- [Guia para programadores do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

- [Integração com o Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-how-to-integrate)

- [Integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

- [Plano do Azure - segurança e identidade](https://azure.microsoft.com/roadmap/?category=security-identity)

**Vídeos**

- [Autenticação do Azure Active Directory com Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

- [Briefing de técnicas de identidade do Azure Active Directory - parte 1 de 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

- [Briefing de técnicas de identidade do Azure Active Directory - parte 2 de 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

- [Compilar aplicações com o Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

- [Vídeos do Microsoft Azure concentra-se no Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

**Formação**  
- [Microsoft Azure para a série de conteúdo de profissionais IT: do Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

**Atualizações de serviço do Azure Active Directory**  
- [Atualizações de serviço do Azure AD](https://azure.microsoft.com/updates/?product=active-directory)

Para obter suporte, pode utilizar os seguintes recursos:
- [Fóruns do MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
- [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="the-azure-active-directory-gallery"></a>A galeria do Azure Active Directory
Além de ter a sua aplicação listada no Azure Marketplace/AppSource, também pode ter a aplicação listada na Galeria de aplicações do Azure AD, que faz parte do Azure Marketplace AppStore. Os clientes que utilizam o Azure AD como um fornecedor de identidade pode encontrar os conectores de aplicações SaaS diferentes publicaram aqui. Os administradores de TI podem adicionar os conectores da Galeria de aplicações e, em seguida, configurar e utilizar os conectores de-início de sessão único (SSO) e o aprovisionamento. AD do Azure suporta todos os protocolos de Federação principal para SSO, incluindo SAML 2.0, OpenID Connect, OAuth e WS-Fed.  

Depois de ter testado que a integração de aplicações funciona com o Azure AD, submeta o pedido de acesso no Portal de rede a nossa aplicação. Se tiver uma conta do Office 365, utilize-o para iniciar sessão no portal. Se não tiver uma conta do Office 365, pode utilizar a sua conta Microsoft (tais como o Outlook ou Hotmail) para iniciar sessão.

## <a name="program-benefits"></a>Benefícios do programa
- Fornece a melhor possíveis único início de sessão experiência para os clientes
- Configuração mínima e Simple da aplicação
- Os clientes podem procurar para a aplicação e encontrá-lo na Galeria
- Qualquer cliente pode utilizar esta integração, independentemente da que SKU do AD do Azure utilizam (gratuito, Basic ou Premium)
- Tutorial de configuração passo a passo para os nossos clientes mútua
- Permite o aprovisionamento de utilizador para a mesma aplicação se estiver a utilizar o SCIM

## <a name="prerequisites"></a>Pré-requisitos
Para listar uma aplicação na galeria do Azure AD, a aplicação deve primeiro implementar um dos protocolos Federação suportados pelo Azure AD. Leia os termos e condições da Galeria de aplicações do Azure AD, localizado em [Informação Legal do Microsoft Azure](https://azure.microsoft.com/support/legal/).  

O seguinte descreve as informações de pré-requisitos adicionais, dependendo do protocolo de que está a utilizar:

**OpenID Connect** - criar a aplicação multi-inquilino no Azure AD e implementar a estrutura de consentimento para a sua aplicação. Envie o pedido de início de sessão para o ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador do cliente com base no ID de inquilino e de UPN do utilizador foi recebido no token.  
**SAML 2.0 ou WS-Fed** -a aplicação deve ter uma capacidade de efetuar a integração de SAML ou WS-Fed SSO no modo SP ou IdP.
