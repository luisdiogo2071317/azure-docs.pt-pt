---
title: Registar uma aplicação de cliente confidenciais no Azure Active Directory - FHIR na API do Azure
description: Este artigo explica como registar uma aplicação cliente confidenciais no Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 064cf4bb0b6e9454a4049cc6c32b51b09e5b2a53
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824131"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registar uma aplicação de cliente confidenciais no Azure Active Directory

Neste artigo, aprenderá como registar uma aplicação cliente confidenciais no Azure Active Directory. Um registo de aplicação de cliente é uma representação do Azure Active Directory de uma aplicação que pode ser utilizada para autenticar em nome de um utilizador e pedir o acesso ao [aplicações de recurso](register-resource-azure-ad-client-app.md). Um aplicativo de cliente confidencial é um aplicativo que pode ser confiável para manter um segredo e apresentar esse segredo ao pedir tokens de acesso. Exemplos de aplicações confidenciais são aplicativos de servidor.

Para registar um novo aplicativo confidencial no portal, siga os passos abaixo.

## <a name="app-registrations-in-azure-portal"></a>Registos de aplicações no portal do Azure

1. No [portal do Azure](https://portal.azure.com), no painel de navegação do lado esquerdo, clique em **Azure Active Directory**.

2. Na **do Azure Active Directory** painel, clique em **registos das aplicações (pré-visualização)**:

    ![Portal do Azure. Novo registo de aplicação.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Clique nas **novo registo**.

## <a name="register-a-new-application"></a>Registar uma nova aplicação

1. Dar ao aplicativo com um nome a apresentar.

2. Forneça um URL de resposta. Estes detalhes podem ser alterados posteriormente, mas se souber o URL de resposta da sua aplicação, introduza-o agora.

    ![Novo registo de aplicação de cliente confidencial.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)

## <a name="api-permissions"></a>Permissões de API

Em seguida, adicione permissões de API:

1. Abra o **permissões de API** e selecione sua [registo de aplicação de recurso de API FHIR](register-resource-azure-ad-client-app.md):

    ![Cliente confidencial. Permissões de API](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

2. Clique em **adicionar uma permissão**

3. Selecione o recurso de API:

    ![Cliente confidencial. As minhas APIs](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-MyApis.png)

4. Selecione os âmbitos (permissões) que o aplicativo confidencial deve ser capaz de fazer para o nome de um utilizador:

    ![Cliente confidencial. Permissões Delegadas](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-DelegatedPermissions.png)

## <a name="application-secret"></a>Segredo de aplicação

1. Crie um segredo de aplicação (segredo do cliente):

    ![Cliente confidencial. Segredo de aplicação](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Forneça uma descrição e a duração do segredo do.

3. Depois de gerada, será apresentado no portal do apenas uma vez. Tome nota do mesmo e armazena de forma segura.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como registar uma aplicação cliente confidenciais no Azure Active Directory. Em seguida, implemente uma API de FHIR no Azure.
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-oss-powershell-quickstart.md)