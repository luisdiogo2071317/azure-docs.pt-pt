---
title: Técnica de aplicações SaaS do Azure Marketplace publicação guia
description: Guia passo a passo e listas de verificação de publicação para a publicação de aplicações de SaaS no Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: BrentL-Collabera
manager: ''
editor: BrentL-Collabera
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.author: pabutler
ms.openlocfilehash: 48b0b4177dad6262105bf30be2b8714f6ea1228f
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>Guia publicação technical aplicações SaaS

Bem-vindo ao técnica de aplicações de SaaS do Azure Marketplace publicação guia. Este guia foi concebido para ajudar candidato e publicadores existentes para listar as respetivas aplicações e serviços no Azure Marketplace utilizando as aplicações de SaaS oferta. 

Para compreender melhor como publicar uma oferta de SaaS, este guia está dividido nas seguintes secções:
* Descrição geral da oferta
* Requisitos do Negócio
* Requisitos Técnicos
* Processo de publicação
* Utilizar o Azure Active Directory para ativar avaliações
* Certificar a integração do Azure AD para Marketplace

## <a name="offer-overview"></a>Descrição geral da oferta  

Aplicações SaaS estão disponíveis em ambos os Storefronts Azure a tabela seguinte descreve as opções disponíveis atuais:

| Opção storefront | Listagem | Versão de avaliação/Transact |  
| --- | --- | --- |  
| AppSource | Sim (contactar-Me) | Sim (PowerBI/Dynamics) |
| Mercado do Azure | Não | Sim (aplicações SaaS) |   

**Listagem:** a opção de publicação de listagem é composta por um contacto enviar-me um tipo de oferta e é utilizada quando uma participação de nível de versão de avaliação ou de transação não é exequível. A vantagem desta abordagem é que permite a publicadores com um solução de mercado iniciar imediatamente a receção de clientes potenciais clientes potenciais que podem ser ativadas em oportunidades para aumentar a sua empresa.  
**Versão de avaliação/Transact:** o cliente tem a opção para diretamente comprar ou pedir uma versão de avaliação para a sua solução. Fornecer uma experiência de avaliação aumenta o nível de envolvimento oferecido aos clientes e permite que os clientes explorar a sua solução antes de comprar. Com uma experiência de avaliação, terá uma melhor possibilidades de promoção de storefronts e deve esperar mais rico e mais clientes potenciais clientes potenciais de ações de envolvimento do cliente. Avaliações tem de incluir suporte gratuito, pelo menos, durante o período de avaliação.  

| Oferta de aplicações SaaS | Requisitos do Negócio | Requisitos Técnicos |  
| --- | --- | --- |  
| **Contacte-nos** | Sim | Não |  
| **PowerBI / Dynamics** | Sim | Sim (integração com o Azure AD) |  
| **Aplicações SaaS**| Sim | Sim (integração com o Azure AD) |     

Para obter mais informações sobre os storefronts Marketplace e um descripiton de cada opção de publicação, consulte o [Marketplace publicador guia](https://aka.ms/sellerguide) e [opções de publicação](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#select-a-publishing-option).

## <a name="business-requirements"></a>Requisitos comerciais
Os SaaS oferecem requisitos empresariais podem ser concluídos em paralelo com os requisitos técnicos. A maioria dos requisitos de negócio e as informações são recolhidas quando criar a oferta de SaaS no Portal de parceiros de nuvem. Os requisitos comerciais são as seguintes: 
* Aceita as políticas de participação
* Integração com a Microsoft 
* Identificar audiência a oferta
* Definir e determinar a gestão de oportunidades potenciais a ser utilizado
* Configurar a política de privacidade e termos de utilização
* Definir os contactos de suporte  

Para obter mais informações, pode ser encontrado no tópico [pré-requisitos para a publicação do marketplace](https://docs.microsoft.com/en-us/azure/marketplace/marketplace-publishers-guide#prerequisites-for-marketplace-publishing)

## <a name="technical-requirements"></a>Requisitos técnicos

Os requisitos técnicos para aplicações SaaS são simples. Os publicadores só são necessárias para ser integrado com o Azure Active Directory (Azure AD) a serem publicadas. A integração do Azure AD com aplicações está bem documento e a Microsoft fornece várias SDKs e recursos para realizar esta tarefa.  

Para começar, é recomendável que tiver uma subscrição dedicada para a publicação do Azure Marketplace, permitindo-lhe isolar o trabalho de outros iniciativas. Após este procedimento é efetuado pode começar a implementar a aplicação SaaS nesta subscrição para iniciar o trabalho de desenvolvimento.  

A documentação do Azure Active Directory, exemplos e orientação melhor estão localizados em sites seguintes: 

* [Guia para programadores do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

* [Integração com o Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-how-to-integrate)

* [Integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

* [Plano do Azure - segurança e identidade](https://azure.microsoft.com/roadmap/?category=security-identity)

Para tutoriais de vídeos, reveja o seguinte:

* [Autenticação do Azure Active Directory com Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

* [Briefing de técnicas de identidade do Azure Active Directory - parte 1 de 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

* [Briefing de técnicas de identidade do Azure Active Directory - parte 2 de 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

* [Compilar aplicações com o Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

* [Vídeos do Microsoft Azure concentra-se no Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Formação gratuita do Azure Active Directory está disponível em  
* [Microsoft Azure para a série de conteúdo de profissionais IT: do Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Além disso, o Azure Active Directory fornece um site para verificar a existência de atualizações de serviço   
* [Atualizações de serviço do Azure AD](https://azure.microsoft.com/updates/?product=active-directory)

Para obter suporte, pode utilizar os seguintes recursos:
* [Fóruns do MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="publishing-process"></a>Processo de publicação

O processo de publicação de SaaS tem passos técnicos e empresariais.  A maioria do trabalho que é feita a desenvolver e a integração do Active Directory do Azure pode ser efetuada em paralelo com o trabalho necessário para satisfazer os requisitos comerciais da oferta. O volume dos requisitos de negócio fazem parte da configuração de oferta da aplicação SaaS o Portal de parceiros de nuvem.  
O diagrama seguinte mostra que os principais passos publicação para a versão de avaliação/Transact oferecem:  

![Passos de publicação de SaaS](./media/marketplace-saas-applications-technical-publishing-guide/saaspublishingsteps.png)  

A tabela seguinte descreve cada um dos passos principais publicação:  

| Passo de publicação | Descrição |   
| --- | --- |  
| **Criar a aplicação SaaS** | Inicie sessão no Portal de parceiros de nuvem, selecione **novo**e, em seguida, selecione o **aplicações SaaS** oferecem. |  
| **Criar a integração com o Azure AD** | Siga os requisitos técnicos descritos na secção anterior para integrar o SaaS oferta com o Azure AD. |  
| **Configurar as definições de oferta**| Introduza a todas as informações da oferta de SaaS iniciais. O ID de oferecer e oferecem nome que pretende utilizar. |     
| **Definir as informações técnicas sobre** | Introduza as informações técnicas sobre a oferta. Para aplicações de SaaS, é necessário a solução URI e tipo do botão de aquisição da oferta (gratuito, registo ou contactar-Me). |  
| **Teste Drive(Optional)** | Este é um tipo de avaliação, necessário principalmente para domínio outros tipos de Marketplace oferece opcional. Permite-lhe ter a versão de avaliação implementado em subscrições do publicador vs. o cliente do fim. |  
| **Definir os materiais Storefront oferta**| Nesta secção publicador irá ligar e carregar logótipos, materiais, documentos legais de Marketing e configurar o sistema de gestão de clientes potenciais clientes potenciais. |
| **Definir os contactos de oferta** | Introduza o contactos de engenharia e informações de contacto de suporte para a oferta de SaaS. |  
| **Certifique-se a integração do SaaS aplicação Azure AD** | Antes de submeter a sua aplicação SaaS para publicação, tem de verificar que a aplicação está integrada com o Azure AD |  
| **Publicar a oferta**| Depois da oferta e dos recursos técnicos concluídos, pode submeter a oferta. Esta ação iniciará o processo de publicação, na qual o modelo de solução é testado, validado, certificado e aprovado para publicação. |

## <a name="using-azure-active-directory-to-enable-trials"></a>Utilizar o Azure Active Directory para ativar avaliações  

Microsoft autentica todos os utilizadores de Marketplace com o Azure AD, por conseguinte, quando um utilizador autenticado clica através da sua lista de avaliação no Marketplace e é redirecionado para o seu ambiente de avaliação, pode aprovisionar o utilizador diretamente para uma versão de avaliação sem necessidade de um início de sessão no passo adicional. O token que recebe a aplicação do Azure AD durante a autenticação inclui informações de utilizador importantes que pode utilizar para criar uma conta de utilizador na sua aplicação, permitindo-lhe automatizar a experiência de aprovisionamento e aumentar a probabilidade de conversão. Para mais informações sobre o token, consulte [Tokens de exemplo](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims) .

Utilizar o Azure AD para ativar a autenticação de 1-Clique para a sua aplicação ou a versão de avaliação faz o seguinte:  
* Simplifica a experiência do cliente do Marketplace para avaliação.  
* Mantém o funcionamento de uma experiência' no produto', mesmo quando do utilizador é redirecionado do Marketplace para o domínio ou o ambiente de avaliação.  
* Diminui a probabilidade de abandonment no redirecionamento porque não existe um passo adicional do início de sessão.  
* Reduz barreiras as eficazes de implementação para o número elevado de utilizadores do Azure AD.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Certificar a integração do Azure AD para Marketplace  

Pode certificar a integração do Azure AD de algumas formas diferentes, dependendo se a aplicação é o único inquilino ou multi-inquilino e se estiver familiarizado como único federado do Azure AD início de sessão (SSO), ou já suportam.  

**Para aplicações de multi-inquilinos:**  

Se já suportam o Azure AD, efetue o seguinte:
1.  Registar a sua aplicação no portal do Azure
2.  Ative a funcionalidade de suporte de vários inquilinos no Azure AD para obter uma experiência de avaliação 'num só clique'. Pode encontrar informações mais específicas [aqui](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).  

Se estiver familiarizado com o Azure AD federado SSO, efetue o seguinte: 
1.  Registar a sua aplicação no portal do Azure
2.  Desenvolver SSO com o Azure AD com [OpenID Connect](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) ou [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Ativar o suporte de vários inquilinos funcionalidade do AAD para obter informações mais específicas de experiência de avaliação gratuita 'num só clique' pode ser encontrada [aqui](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Para a aplicação de inquilino único, utilize qualquer uma das seguintes opções:**  
* Adicionar utilizadores ao seu diretório, como os utilizadores convidados utilizando [B2B do Azure](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
* Aprovisionar manualmente avaliações para clientes através da utilização de 'Contactar-Me'
* Desenvolver por-cliente por unidade de teste
* Criar uma aplicação de demonstração de exemplo do multi-inquilino com o SSO

