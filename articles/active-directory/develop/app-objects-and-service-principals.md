---
title: Aplicação do Azure Active Directory e objetos de Principal de serviço
description: Uma discussão sobre a relação entre a aplicação e objetos de principal de serviço no Azure Active Directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
services: active-directory
editor: ''
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/19/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 057465567217cff080b189bcdabee3042f41468d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595880"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Aplicação e objetos de principal de serviço no Azure Active Directory (Azure AD)
Por vezes, o significado de "aplicação" pode ser mal compreendido quando utilizado no contexto do Azure AD. O objetivo deste artigo é esclarecer os aspectos de conceituais e concretos de integração de aplicações do Azure AD, com uma ilustração de registo e o consentimento para um [aplicação multi-inquilino](developer-glossary.md#multi-tenant-application).

## <a name="overview"></a>Descrição geral
Uma aplicação que foi integrada com o Azure AD tem implicações que vão além do aspecto de software. "Aplicação" é frequentemente utilizada como um termo conceitual, que faça referência a não apenas o software de aplicação, mas também o registo do Azure AD e função na autenticação/autorização "conversas" em tempo de execução. Por definição, um aplicativo pode funcionar num [cliente](developer-glossary.md#client-application) função (consumindo um recurso), uma [servidor recursos](developer-glossary.md#resource-server) função (expondo as APIs para os clientes), ou até mesmo ambos. O protocolo de conversação é definido por um [fluxo de concessão de autorização do OAuth 2.0](developer-glossary.md#authorization-grant), permitindo que o cliente/recurso para acesso/proteger os dados de um recurso, respetivamente. Agora vamos um nível mais aprofundado e ver como o modelo de aplicação do Azure AD representa uma aplicação em tempo de design e tempo de execução. 

## <a name="application-registration"></a>Registo da aplicação
Quando registar uma aplicação do Azure AD no [portal do Azure][AZURE-Portal], dois objetos são criados no seu inquilino do Azure AD: um objeto de aplicativo e um objeto principal de serviço.

#### <a name="application-object"></a>objeto de aplicativo
Uma aplicação do Azure AD é definida por seu um e apenas a objeto application, que reside no inquilino do Azure AD em que a aplicação foi registrada, conhecido como inquilino "principal" da aplicação. O Azure AD Graph [entidade de aplicativo] [ AAD-Graph-App-Entity] define o esquema para as propriedades de um objeto de aplicação. 

#### <a name="service-principal-object"></a>objeto do principal de serviço
Para poder aceder aos recursos que são protegidos pelo inquilino do Azure AD, a entidade que precisa de acesso tem de ser representada por uma entidade de segurança. Isso é verdadeiro para aplicativos (principal de serviço) e os utilizadores (principal de utilizador). A entidade de segurança define a política de acesso e permissões de utilizador/aplicação nesse inquilino. Isto permite que os principais recursos como a autenticação de utilizador/aplicação durante o início de sessão e autorização durante o acesso a recursos.

Quando um aplicativo é concedido permissão para aceder aos recursos num inquilino (após o registo ou [consentimento](developer-glossary.md#consent)), é criado um objeto principal de serviço. O Azure AD Graph [entidade ServicePrincipal] [ AAD-Graph-Sp-Entity] define o esquema para as propriedades de um serviço principal do objeto. 

#### <a name="application-and-service-principal-relationship"></a>Aplicação e a relação de principal de serviço
Considere o objeto de aplicativo como o *global* representação da sua aplicação para utilização em todos os inquilinos e o principal de serviço como o *local* representação para utilização num inquilino específico. O serve de objeto de aplicativo como o modelo a partir do qual comuns e as propriedades predefinidas é *derivado* para utilização na criação de objetos de principal de serviço correspondentes. Um objeto de aplicativo, portanto, tem uma relação de 1:1 com o aplicativo de software e uma relação de 1:many com seus objetos de principal de serviço correspondentes.

Um principal de serviço tem de ser criado em cada inquilino onde o aplicativo é usado, permitindo que ele estabeleça uma identidade para início de sessão e/ou acesso a recursos a serem protegidos pelo inquilino. Uma aplicação de inquilino único tem apenas um principal de serviço (no inquilino principal), criado e dar o seu consentimento para utilização durante o registo de aplicação. Uma multi-inquilino aplicação/API Web também tem um principal de serviço criado em cada inquilino em que um utilizador de inquilino deu consentimento à sua utilização. 

> [!NOTE]
> As alterações que fizer para seu objeto de aplicativo, também são refletidas no seu objeto principal de serviço no inquilino principal da aplicação apenas (o inquilino onde foi registada). Para aplicações multi-inquilino, alterações para o objeto de aplicativo não são refletidas em objetos de principal de serviço dos inquilinos qualquer consumidor, até que o acesso seja removido através do [painel de acesso de aplicação](https://myapps.microsoft.com) e concederam novamente.
><br>  
> Observe também que os aplicativos nativos são registados como multi-inquilino por predefinição.
> 
> 

## <a name="example"></a>Exemplo
O diagrama seguinte ilustra a relação entre um aplicativo objeto application e objetos de principal, no contexto de uma aplicação multi-inquilino de exemplo chamados de serviço correspondente **aplicação de RH**. Neste cenário, existem três inquilinos do Azure AD: 

* **Adatum** -o inquilino utilizado pela empresa que desenvolveu o **aplicação de RH**
* **Contoso** -o inquilino utilizado pela organização Contoso, que é um consumidor do **aplicação de RH**
* **A Fabrikam** -o inquilino utilizado pela organização Fabrikam, que também consome o **aplicação de RH**

![Relação entre um objeto de aplicativo e um objeto principal de serviço](./media/app-objects-and-service-principals/application-objects-relationship.png)

No diagrama anterior, o passo 1 é o processo de criação do aplicativo e objetos de principal de serviço no inquilino principal da aplicação.

No passo 2, quando os administradores de Contoso e Fabrikam concluir consentimento, um objeto principal de serviço é criado no inquilino do Azure AD da empresa e atribuído as permissões que o administrador concedido. Observe também que a aplicação de RH pode ser configurado/projetado para permitir que o consentimento por utilizadores para utilização individual.

No passo 3, os inquilinos de consumidor do RH aplicativo (Contoso e Fabrikam) cada tem seu próprio objeto principal do serviço. Cada representa seu uso de uma instância do aplicativo em tempo de execução, regido pelas permissões consentidas pelo administrador do respectivo.

## <a name="next-steps"></a>Passos Seguintes
Objeto de aplicativo de um aplicativo pode ser acedido através da API do Azure AD Graph, o [do portal do Azure] [ AZURE-Portal] editor de manifesto de aplicativo, ou [cmdlets do Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), como representado pelo seu OData [entidade de aplicativo][AAD-Graph-App-Entity].

Objeto principal do serviço de um aplicativo pode ser acedido através da API do Azure AD Graph ou [cmdlets do PowerShell do Azure AD](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), conforme representado pelo seu OData [ServicePrincipal entidade] [ AAD-Graph-Sp-Entity].

O [do Azure AD Graph](https://graphexplorer.azurewebsites.net/) é útil para consultar o aplicativo e os objetos principais de serviço.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
