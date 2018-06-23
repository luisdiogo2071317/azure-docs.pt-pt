---
title: Como e por que razão são adicionadas aplicações ao Azure Active Directory
description: O significa para uma aplicação a adicionar ao Azure AD e como estes proceder para que não existe?
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, lenalepa
ms.openlocfilehash: 5c8ae9534e79b8dc801262f85d8a007e050f4da7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316964"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Como e por que razão são adicionadas aplicações para o Azure AD

Existem dois representações de aplicações no Azure AD: 
* [Objetos de aplicação](active-directory-application-objects.md#application-object) – apesar de existirem [exceções](#notes-and-exceptions), objetos de aplicação podem ser considerados a definição de uma aplicação.
* [Principais de serviço](active-directory-application-objects.md#service-principal-object) -pode ser considerada uma instância de uma aplicação. Principais de serviço geralmente referenciam um objeto de aplicação e um objeto de aplicação pode ser referenciado por várias principais de serviço em diretórios.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Quais são os objectos da aplicação e onde vêm da?
Pode gerir [objectos da aplicação](active-directory-application-objects.md#application-object) no portal do Azure através de [registos de aplicação](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) experiência. Objetos da aplicação descrevem a aplicação para o Azure AD e podem ser considerados a definição da aplicação, permitindo que o serviço para saber como emitir tokens para a aplicação com base nas respetivas definições. O objeto de aplicação só existem no respetivo diretório raiz, mesmo se for uma aplicação de multi-inquilino que suporta principais de serviço no outros diretórios. O objeto de aplicação pode incluir qualquer um dos seguintes (como obter informações adicionais, bem como não aqui mencionadas):
* Nome, o logótipo e fabricante
* URLs de Resposta
* Segredos (simétricas e/ou assimétricos chaves utilizados para autenticar a aplicação)
* Dependências de API (OAuth)
* APIs/recursos/âmbitos de publicada (OAuth)
* Funções (RBAC) de aplicação
* Metadados SSO e configuração
* Aprovisionamento de utilizadores metadados e configuração
* Metadados de proxy e de configuração

Objetos da aplicação podem ser criados através de vários caminhos, incluindo:
* Registos de aplicação no portal do Azure
* Criar uma nova aplicação com o Visual Studio e configurá-lo para utilizar a autenticação do Azure AD
* Quando o administrador adiciona uma aplicação da Galeria de aplicações (que também irá criar um serviço principal)
* Utilizar o Microsoft Graph API, o AD Graph API do Azure ou o PowerShell para criar uma nova aplicação
* Muitas outras, incluindo várias experiências do Programador no Azure e nas experiências do Explorador de API em todos os centros de programador

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Quais são os principais de serviço e onde vêm da?
Pode gerir [principais de serviço](active-directory-application-objects.md#service-principal-object) no portal do Azure através de [aplicações empresariais](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) experiência. Principais de serviço são o que regulam uma aplicação a ligar ao Azure AD e podem ser consideradas a instância da aplicação no seu diretório. Para qualquer aplicação específica, pode ter no máximo um objeto de aplicação (que é registado num diretório "raiz") e um ou mais serviço principal objetos que representa as instâncias da aplicação em cada diretório em que este aja. 

O principal de serviço pode incluir:

* Fazer uma cópia de uma referência a um objeto de aplicação através da propriedade de ID de aplicação
* Registos de utilizador local e atribuições de função de aplicação de grupo
* Registos de permissões de utilizador e administrador locais concedidas à aplicação
  * Por exemplo: permissão para a aplicação para aceder ao e-mail de um utilizador específico
* Registos de políticas locais, incluindo a política de acesso condicional
* Registos de alternativos definições locais para uma aplicação
  * Regras de transformação de afirmações
  * Mapeamentos de atributos (aprovisionamento de utilizadores)
  * Funções específicas do diretório de aplicação (se a aplicação suportar funções personalizadas)
  * Nome de diretório específico ou logótipo

Como objetos de aplicação, também podem ser criados principais de serviço através de vários caminhos, incluindo:

* Quando os utilizadores iniciarem sessão a uma aplicação de terceiros integrado com o Azure AD
  * Durante o início de sessão, é pedido aos utilizadores para conceder permissão para a aplicação para aceder ao respetivo perfil e outras permissões. A primeira pessoa a dar consentimento faz com que um principal de serviço que representa a aplicação a ser adicionado ao diretório.
* Quando os utilizadores iniciarem sessão Microsoft online Services como [do Office 365](http://products.office.com/)
  * Quando subscreve o Office 365 ou iniciar uma versão de avaliação, uma ou mais principais de serviço são criados no diretório que representa os serviços que são utilizados para fornecer todas as funcionalidades associadas com o Office 365.
  * Alguns serviços do Office 365, como o SharePoint criar principais do serviço de forma contínua para permitir a comunicação segura entre componentes, incluindo fluxos de trabalho.
* Quando o administrador adiciona uma aplicação da Galeria de aplicações (Isto vai também criar um objeto subjacente de aplicação)
* Adicionar uma aplicação para utilizar o [Proxy de aplicações do Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Ligar uma aplicação para início de sessão único utilizando SAML ou palavra-passe único início de sessão (SSO)
* Através de programação através do Azure AD Graph API ou o PowerShell

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Como são objetos de aplicação e principais de serviço relacionado com entre si?
Uma aplicação tem um objeto de aplicação no respetivo diretório raiz que é referenciado por um ou mais principais de serviço em cada um dos diretórios que opera (incluindo o diretório de raiz da aplicação).
![Um diagrama que ilustra como objetos de aplicação e principais de serviço interagem com entre si e instâncias do Azure AD.][apps_service_principals_directory]

No diagrama anterior, a Microsoft tem dois diretórios internamente (mostradas do lado esquerdo) que utiliza para publicar aplicações:

* Uma para Microsoft Apps (diretório de serviços da Microsoft)
* Uma para aplicações de terceiros previamente integradas (diretório de Galeria de aplicações)

Aplicação publicadores/fornecedores que integrar com o Azure AD são necessários para ter um diretório de publicação (apresentado à direita como "Diretório de SaaS algumas").

Aplicações que adicionou por si (representado como **aplicação (sua)** no diagrama) incluem:

* Aplicações que desenvolvidas (integradas com o Azure AD)
* Aplicações ligados para single-sign-on
* Aplicações publicadas a utilizar o proxy de aplicações do Azure AD

### <a name="notes-and-exceptions"></a>Notas e exceções
* Nem todos os principais de serviço apontam de volta para um objeto de aplicação. Quando o Azure AD foi criado originalmente os serviços fornecidos às aplicações mais limitados e o principal de serviço era suficiente para estabelecer uma identidade de aplicação. O principal de serviço original era próximo na forma da conta de serviço do Windows Server Active Directory. Por este motivo, é possível criar principais de serviço através de caminhos de diferentes, tais como utilizar o Azure AD PowerShell, sem primeiro criar um objeto de aplicação. O Azure AD Graph API requer um objeto de aplicação antes de criar um serviço principal.
* Nem todas as informações descritas acima atualmente está exposto através de programação. Seguem-se apenas disponível na IU:
  * Regras de transformação de afirmações
  * Mapeamentos de atributos (aprovisionamento de utilizadores)
* Para obter informações mais detalhadas sobre o principal de serviço e os objetos de aplicação, consulte a documentação de referência da API de REST do Azure AD Graph:
  * [Aplicação](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Principal de serviço](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Por que motivo integrar aplicações com o Azure AD?
São adicionadas aplicações ao Azure AD para tirar partido de uma ou mais serviços fornece, incluindo:

* Aplicação de autenticação e autorização
* Autorização e autenticação de utilizador
* SSO utilizando a palavra-passe ou Federação
* Aprovisionamento de utilizador e sincronização
* Utilize o diretório para definir funções da aplicação para efetuar a autorização baseada em funções de controlo de acesso baseado em Funções - verifica numa aplicação
* Serviços de autorização do OAuth - utilizados pelo Office 365 e outras aplicações da Microsoft para autorizar o acesso a APIs/recursos
* Publicação de aplicações e o proxy - publicar uma aplicação de uma rede privada à internet

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Quem tem permissão para adicionar aplicações para os meus instância do Azure AD?
Objetos que enquanto existirem algumas tarefas que apenas os administradores globais podem efetuar (como adicionar aplicações a partir da Galeria de aplicações e configuração de uma aplicação para utilizar o Proxy de aplicações) por predefinição todos os utilizadores no seu diretório tem direitos para registar a aplicação estão a desenvolver e discrição sobre quais as aplicações que partilham/conceder acesso aos respetivos dados organizacionais através de consentimento. Se uma pessoa é o primeiro utilizador no seu diretório para iniciar sessão a uma aplicação e conceder o consentimento, que irá criar um principal de serviço no seu inquilino; caso contrário, as informações de concessão de consentimento serão armazenadas no principal de serviço existente.

Permitir que os utilizadores registar e autorizar aplicações pode inicialmente som que dizem respeito à, mas tenha em atenção o seguinte:


* As aplicações foram capazes de tirar partido do Windows Server Active Directory para a autenticação de utilizador de muitos anos sem necessidade de aplicação a ser registado ou registado no diretório. Agora a organização vai ter melhorado visibilidade para exatamente como muitas aplicações estão a utilizar o diretório e para que fim.
* Delegar estas responsabilidades aos utilizadores negates a necessidade de um registo de aplicação condicionada por admin e o processo de publicação. Com os serviços de Federação do Active Directory (ADFS) foi provável que um administrador tinha de adicionar uma aplicação como uma entidade confiadora em nome dos programadores. Agora, os programadores podem Self-Service.
* Os utilizadores a iniciar sessão utilizando as contas da organização para fins empresariais de aplicações são uma boa coisa. Se estes subsequentemente, deixe a organização que automaticamente estas perdem o acesso à respetiva conta na aplicação que estavam a utilizar.
* Ter um registo das quais os dados que foi partilhados com a qual a aplicação é uma boa coisa. Os dados são mais transportable que nunca e é útil para ter um registo de quem partilhados os dados com as aplicações que encriptado.
* Os proprietários de API que utilizam o Azure AD para OAuth decidir exatamente conseguem que os utilizadores de permissões conceder a aplicações e que permissões necessitam de um administrador para aceitar. Apenas os administradores podem consentimento âmbitos maiores e mais significativas permissões, enquanto o consentimento do utilizador tem um âmbito para as capacidades e os dados dos utilizadores.
* Quando um utilizador adiciona ou lhe permite uma aplicação aceder aos respetivos dados, o evento pode ser auditado para que possa vê os relatórios de auditoria no portal do Azure para determinar a forma como uma aplicação foi adicionada ao diretório.

Se pretender continuar a impedir que os utilizadores no seu diretório, registo de aplicações e de iniciar sessão em aplicações sem a aprovação do administrador, existem duas definições que podem ser alteradas para desativar as capacidades:

* Para impedir que os utilizadores consenting para as aplicações no seu próprio nome:
  1. No portal do Azure, visite o [as definições de utilizador](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) secção em aplicações da empresa.
  2. Alteração **os utilizadores podem autorizar aplicações acedem aos dados da empresa em nome daqueles** para **não**. 
      > [!NOTE]
      > Se optar por desativar o consentimento do utilizador, um administrador será necessário consentimento para qualquer nova aplicação de que um utilizador tem de utilizar.    
* Para impedir que os utilizadores registar as suas próprias aplicações:
  1. No portal do Azure, visite o [as definições de utilizador](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) secção no Azure Active Directory
  2. Alteração **os utilizadores podem registar aplicações** para **não**.

> [!NOTE]
> A Microsoft próprio utiliza a configuração predefinida, com os utilizadores possam registar aplicações e autorizar aplicações no seu próprio nome.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

