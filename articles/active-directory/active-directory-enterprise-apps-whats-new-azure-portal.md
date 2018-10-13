---
title: O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory | Documentos da Microsoft
description: Saiba quais são as novidades na gestão de aplicação empresarial no Azure Active Directory.
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
editor: ''
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: fefc508679a309262d07a582fc8f5bdf9f67cfe5
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310124"
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory 

Azure Active Directory (Azure AD) melhorou a ferramentas de gerenciamento de aplicativos empresariais, com novas funcionalidades e capacidades para tornar a gestão de aplicações mais simples e eficiente.

Seguem-se alguns dos aprimoramentos para o Azure AD no [portal do Azure](https://portal.azure.com).

- Uma galeria de aplicações melhorada experiência, com um modelo de criação de aplicativos simplificado e suporte para todos os tipos de aplicação que está habituado a. 
- Uma experiência de início rápido de novo em folha que pode ajudá-lo a começar com um piloto do seu aplicativo. 
- Configure políticas self-service com apenas alguns cliques. 
- Melhorias ao proxy de aplicações, única configuração de início de sessão e colocar suas próprias experiências de aplicativos, permitindo que obter mais done, que, antes.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Melhorias para a Galeria de aplicações do Azure Active Directory

Adicionar as suas aplicações favoritas, quer estejam a partir da [Galeria de aplicações](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery), aplicações personalizadas que está a expandir para a cloud ou novos aplicativos que está desenvolvendo.  Pode começar a utilizar com esta nova experiência, clicando em **Add** sob **aplicações empresariais** ou **todos os aplicativos**.
 
  ![Adicionar uma aplicação](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Uma vez na galeria, verá todas as aplicações em destaque que suportam o aprovisionamento de utilizadores apresentados frente e no centro. Pode procurar todos os tipos de categorias diferentes para explorar os aplicativos que mais lhe interessa, ou pode utilizar a experiência de pesquisa para encontrar rapidamente as aplicações que pretende integrar.

  ![A Galeria de aplicações](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Adicionar aplicações personalizadas num único local

Além de adicionar aplicações previamente integradas a partir da galeria, todas as experiências da configuração de aplicação personalizada que foram utilizados para no portal de gestão clássico agora são possíveis no novo portal. Se estiver a expandir uma aplicação no local com o proxy de aplicações, integrando sua própria palavra-passe ou o aplicativo de SSO federado, ou criar um aplicativo totalmente novo com o registo de aplicação, pode obter a ele tudo a partir desse único local.

  ![Adicionar a sua aplicação](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Para começar a adicionar a sua própria aplicação**:

1. Clique em **adicionar sua própria ligação** no topo da Galeria de aplicações. 
1. Verá duas opções na sua frente: **implementar uma aplicação existente** ou **desenvolver um novo aplicativo**. Continue a ler para saber a diferença entre as duas opções e como utilizá-los.

### <a name="deploying-existing-applications"></a>Implantação de aplicativos existentes

1. Se tem uma aplicação em execução já e quiser apenas integrá-lo para o Azure AD para início de sessão único no ou aprovisionamento, escolha o **implementar uma aplicação existente** opção. Escolha um nome para a sua aplicação, clique em **adicionar**.
1. Já está! Em vez de terem de conhecer todos os detalhes sobre a sua aplicação com antecedência, pode agora configurar o funcionamento sua nova aplicação ao navegar por meio do menu à esquerda e configurar a aplicação ao seu gosto em qualquer altura.

  ![Adicionar uma aplicação existente com um único clique](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Desenvolvimento de novos aplicativos

1. Se estiver desenvolvendo um novo aplicativo, há uma maneira fácil de entrar para o registo de aplicação correta da galeria:
1. Clique nas **adicionar sua própria** opção a partir da Galeria de aplicações, selecione **desenvolver uma aplicação existente**, e verá uma ligação rápida diretamente para a experiência de adicionar a aplicação.

  ![Adicionar uma aplicação recentemente desenvolvida em poucos cliques](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Depois de adicionar uma aplicação com o Registro do aplicativo, verá que apareça na lista de aplicações empresariais, onde poderá configurar o início de sessão único e gerir políticas de acesso para a sua nova aplicação.

  ![Gerir o acesso à sua nova aplicação em aplicações empresariais](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quickstart-get-going-with-your-new-application-right-away"></a>Início rápido: Comece a trabalhar com sua nova aplicação agora mesmo 

Depois de adicionar uma aplicação, se ser previamente integradas ou sua própria aplicação, criámos uma experiência de início rápido personalizada para ajudá-lo alicerçada às próprias a nova experiência de aplicativos rapidamente. Se seguir sistematicamente cada opção, vamos orientá-lo a interface do Usuário e mostrarei como lidar com um piloto de sua nova aplicação mais depressa possível. 
 
  ![A nova experiência de início rápido de aplicativos](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 Visite esta nova experiência de início rápido em qualquer altura, para qualquer aplicação, ao clicar no **guia de introdução** no menu de navegação à esquerda do aplicativo.


## <a name="updated-application-proxy-configuration"></a>Configuração de proxy de aplicação atualizada

Agora, vamos dizer uma das novas aplicações que adicionou está em execução no seu ambiente no local e pretender integrá-lo com o Azure AD.  Uma das coisas novas interessantes sobre a nova experiência de configuração de aplicação no Azure AD novo portal é que ao dividir modo de sessão o aplicativo da sua configuração de proxy de aplicação, pode agora facilmente expor SSO de palavra-passe ou as aplicações federadas que são executados no seu direito de rede empresarial para a cloud, sem ter de criar várias instâncias do aplicativo.

Também pode configurar qualquer um dos novos aplicativos que adicionou para utilização com o direito de Proxy de aplicações do Azure AD a partir do novo portal, incluindo aplicações que suportam a experiência de autenticação do Windows nativa.

  ![Configuração de uma aplicação para utilizar a opção de início de sessão em autenticação integrada do Windows](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 
Para começar a configurar uma aplicação de autenticação do Windows nativa com o Proxy de aplicações:
1. Clique no item de navegação de início de sessão único e escolha **a autenticação integrada do Windows** em configurações de logon e configure as definições ao seu gosto.
1. Além de oferecer suporte a esses novos modos de autenticação, também agora pode carregar certificados a partir de domínios personalizados para oferecer suporte a aplicativos em execução em pontos de extremidade seguros dentro da sua organização.  
 
   ![Carregar um certificado para ser utilizado com o Proxy de aplicações](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

1. Para carregar um novo certificado para a sua aplicação de Favoritos no local, clique nas **proxy de aplicações** no menu de navegação esquerdo, clique no **certificado** Seletor e carregue um certificado ficheiro que pode utilizar para encriptar pedidos do nosso ponto final da cloud à sua aplicação.

## <a name="advanced-federated-single-sign-on-configuration"></a>Federado única início de sessão numa configuração avançada

Para aqueles que utilizam aplicações federadas hoje, existem muitos recursos novos no início de sessão configuração baseada no SAML em. Para começar, agora pode totalmente personalizar, adicionar, remover e mapear os atributos de utilizador existente emitidos como afirmações nos SAML tokens.
 
  ![Personalizar os atributos de utilizador de token SAML passado para um aplicativo federado](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)

Para verificar que a nova configuração de SSO federado:
1. Abrir um aplicativo federado **início de sessão único** no menu de navegação esquerdo e certifique-se de que o '*baseado em SAML logon** modo está selecionado. 
1. Uma vez, ativar a caixa de verificação sob o **atributos de utilizador** passado de cabeçalho para modificar todos os atributos incluídos no SAML token para essa aplicação.

Pode também criar, rollover e gerir certificados utilizados para início de sessão único federado, bem como editar a quem é notificada quando o certificado está prestes a expirar. Verá que estas novas opções sob o **certificados** no mesmo único início de sessão no painel do cabeçalho.
 
  ![Criar um novo certificado, personalizar o e-mail de notificação de expiração e as opções de assinatura de certificado](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-parameter"></a>Parâmetro de estado de reencaminhamento
Por fim, podemos também estendi o conjunto de parâmetros de URL de SAML, fornecemos suporte para incluir o **parâmetro State de reencaminhamento**, que é a página de seus usuários serão direcionado para dentro de um aplicativo federado assim que o início de sessão for concluído. Esta é uma definição útil para configurar se pretende enviar os seus utilizadores para um local específico na aplicação para obtê-los daqui rapidamente.

  ![Definir o parâmetro de estado de reencaminhamento de SAML](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Para definir o parâmetro de estado de reencaminhamento**:

1. Ativar a **Mostrar definições de URL avançadas** caixa de verificação sob a **domínio e URLs** cabeçalho o início de sessão único no painel de configuração. 
1. Um conjunto de novo URL de entrada caixas aparecem que permitem-lhe definir este parâmetro e outras definições de URL de SAML.

## <a name="bring-your-own-password-sso-applications"></a>Traga a sua própria palavra-passe de aplicações de SSO

Sabemos que nem todos os aplicativos suporta o Federação prontos a utilizar. Por exemplo, talvez um dos novos aplicativos que adicionou tem um ecrã de início de sessão personalizado que os utilizadores utilizam um nome de utilizador e palavra-passe para iniciar sessão no. Ainda pode integrar esses tipos de aplicativos com o Azure AD com nosso **traga os seus próprios aplicativos** recurso, que agora está disponível para os configurar no novo portal.
 
  ![Integração de palavra-passe personalizada vaulting aplicativos com o Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**Para verificar a funcionalidade "Traga os seus próprios aplicativos"**:

1. Depois de definir o modo único início de sessão para um novo aplicativo personalizado que adicionar ao **baseado em palavra-passe de início de sessão**, introduza o URL em que a aplicação compõe o ecrã de início de sessão e 
1. Clique em **Guardar**.  
1. Depois de fazer isso, podemos automaticamente irá extrair essa URL para um nome de utilizador e palavra-passe de caixa de entrada e permite-lhe utilizar o Azure AD para transmitir palavras-passe para esse aplicativo usando a extensão de browser do painel de acesso de forma segura.

## <a name="configure-self-service-application-access"></a>Configurar o acesso de aplicações self-service

Depois de adicionar vários novos aplicativos, talvez queira permitir que os utilizadores procurar e adicionar esses aplicativos para o seus próprio painéis de acesso, sem a necessidade de se preocupar com a como administrador. Agora, com esta versão mais recente, pode configurar e gerir o acesso de aplicações self-service diretamente a partir do novo portal.

  ![Configurar o acesso de aplicações self-service para uma aplicação de SSO de palavra-passe](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Para configurar e gerir o acesso de aplicações self-service**:

1. Para começar a utilizar, pode selecionar o **self-service** opção da aplicação do esquerda do menu de navegação e defina a **permitir que os utilizadores peçam acesso a esta aplicação?** a opção de "**Sim**". 
1. Isso permitirá que configure quem tem permissão para aprovar o acesso a esta aplicação e os utilizadores self-service do grupo serão adicionados. Além disso, se a aplicação está configurada para a palavra-passe início de sessão único, também verá outra opção que permite-lhe, opcionalmente, permitir que os aprovadores gerir as palavras-passe atribuídas à aplicação.

## <a name="feedback"></a>Comentários

Esperamos que como usar a melhor experiência do Azure AD. Mantenha os comentários chegando! Poste seus comentários e ideias para aprimoramento no **do Portal de administração** secção do nosso [fórum de comentários](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Nós estamos muito entusiasmados sobre como criar algo novo e interessante todos os dias e utilize as diretrizes a forma e definir o que devemos criar a seguir.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais detalhes, consulte [gerenciamento de aplicativos com o Azure Active Directory](manage-apps/what-is-application-management.md).



