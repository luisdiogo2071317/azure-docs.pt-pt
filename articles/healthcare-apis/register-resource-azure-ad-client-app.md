---
title: Registar uma aplicação de recurso no Azure Active Directory - FHIR na API do Azure
description: Este artigo explica como registar uma aplicação de recurso no Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: c0602d44a5b01e31cd2082e2b05d17c4289adaa1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824149"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Registar uma aplicação de recurso no Azure Active Directory

Neste artigo, aprenderá como registar um recurso (ou a API) aplicação no Azure Active Directory. Uma aplicação de recurso é uma representação do Azure Active Directory do servidor FHIR API propriamente dita e aplicações de cliente podem pedir acesso ao recurso durante a autenticação. A aplicação de recurso também é conhecido como o *público-alvo* no linguajar do OAuth.

## <a name="app-registrations-in-azure-portal"></a>Registos de aplicações no portal do Azure

1. No [portal do Azure](https://portal.azure.com), no painel de navegação do lado esquerdo, clique em **Azure Active Directory**.

2. Na **do Azure Active Directory** painel, clique em **registos das aplicações (pré-visualização)**:

    ![Portal do Azure. Novo registo de aplicação.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Clique nas **novo registo**.

## <a name="add-a-new-application-registration"></a>Adicionar um novo registo de aplicação

Preencha os detalhes para a nova aplicação. Existem não requisitos específicos para o nome a apresentar, mas defini-la para o URI do servidor FHIR facilita encontrar:

![Novo registo de aplicação](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

## <a name="set-identifier-uri-and-define-scopes"></a>Definir o URI identificador e definir âmbitos

Uma aplicação de recurso tem um identificador URI (URI de ID de aplicação), os clientes que podem utilizar ao pedir acesso ao recurso. Este valor irá preencher a `aud` de afirmações do token de acesso. É recomendado que defina este URI para ser o URI do seu servidor FHIR. Para SMART nas aplicações FHIR, presume-se que o *público-alvo* é o URI do servidor FHIR.

1. Clique em **expor uma API**

2. Clique em **definir** junto a *URI de ID de aplicação*.

3. Clique em **Adicionar âmbito** e adicionar qualquer âmbitos que pretende definir para API. O Azure AD não permite atualmente barras (`/`) em nomes de âmbito. Recomendamos que utilize `$` em vez disso. Como um âmbito `patient/*.read` seria `patient$*.read`.

    ![Público-alvo e âmbito](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

## <a name="define-application-roles"></a>Definir funções de aplicação

A API do Azure para FHIR e o servidor de FHIR de sistemas operacionais para utilização do Azure [funções de aplicação do Azure Active Directory](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) para controlo de acesso baseado em funções. Para definir quais funções devem estar disponíveis para a API de servidor FHIR, abra a aplicação de recurso [manifesto](https://docs.microsoft.com/azure/active-directory/active-directory-application-manifest/):

1. Clique em **manifesto**:

    ![Funções da Aplicação](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. Na `appRoles` propriedade, adicionar as funções que gostaria que os usuários ou aplicativos tenham:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como registar uma aplicação de recurso no Azure Active Directory. Em seguida, implemente uma API de FHIR no Azure.
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-oss-powershell-quickstart.md)