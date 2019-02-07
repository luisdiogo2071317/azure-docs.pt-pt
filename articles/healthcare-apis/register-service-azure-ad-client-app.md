---
title: Registar uma aplicação de serviço no Azure Active Directory - FHIR na API do Azure
description: Este artigo explica como registar uma aplicação de serviço no Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 19f730c1d8a0fc0f809e8e16016795e725d80b61
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824173"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Registar uma aplicação de cliente do serviço no Azure Active Directory

Neste artigo, aprenderá como registar uma aplicação de cliente do serviço no Azure Active Directory. Registos de aplicação de cliente são representações de Azure Active Directory das aplicações que podem ser utilizadas para autenticar e obter os tokens. Um cliente de serviço destina-se para ser utilizado por uma aplicação para obter um token de acesso sem autenticação interativa de um utilizador. Ele teria determinadas permissões de aplicação e utilizar um segredo de aplicação (palavra-passe) ao obter os tokens de acesso.

Siga os passos abaixo para criar um novo cliente de serviço.

## <a name="app-registrations-in-azure-portal"></a>Registos de aplicações no portal do Azure

1. No [portal do Azure](https://portal.azure.com), no painel de navegação do lado esquerdo, clique em **Azure Active Directory**.

2. Na **do Azure Active Directory** painel, clique em **registos das aplicações (pré-visualização)**:

    ![Portal do Azure. Novo registo de aplicação.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Clique em **novo registo**.

## <a name="service-client-application-details"></a>Detalhes de aplicação de cliente do serviço

* O cliente do serviço tem um nome a apresentar e também pode fornecer um URL de resposta, mas ele normalmente não será utilizado.

    ![Portal do Azure. Registo de aplicações do cliente de serviço novo.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>Permissões de API

Terá de conceder o cliente do serviço de funções de aplicação. 

1. Abra o **permissões de API** e selecione sua [registo de aplicação de recurso de API FHIR](register-resource-azure-ad-client-app.md):

    ![Portal do Azure. Permissões de API do cliente do serviço](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Selecione as funções de aplicação é uma das que são definidos na aplicação de recurso:

    ![Portal do Azure. Permissões de aplicação de cliente do serviço](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Consentimento de conceder à aplicação. Se não tiver as permissões necessárias, consulte o administrador do Azure Active Directory:

    ![Portal do Azure. Consentimento de administrador do cliente de serviço](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Segredo de aplicação

O cliente do serviço tem um segredo (palavra-passe), que será utilizado ao obter os tokens.

1. Clique em **certificados &amp; segredos**

2. Clique em **novo segredo do cliente**

    ![Portal do Azure. Segredo do cliente de serviço](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Indique uma duração do segredo.

4. Assim que for gerado, ele só será apresentado uma vez no portal. Tome nota do mesmo e armazenar numa forma segura.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como registar uma aplicação de cliente do serviço no Azure Active Directory. Em seguida, implemente uma API de FHIR no Azure.
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-oss-powershell-quickstart.md)