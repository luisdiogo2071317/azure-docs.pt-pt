---
title: Registar uma aplicação de cliente público no Azure Active Directory - FHIR na API do Azure
description: Este artigo explica como registar uma aplicação de cliente público no Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 69504bc9ba0420b47a26519a0b112ff102171254
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824161"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Registar uma aplicação de cliente público no Azure Active Directory

Neste artigo, aprenderá como registar uma aplicação de pública no Azure Active Directory. Registos de aplicação de cliente são representações de Azure Active Directory das aplicações que podem autenticar e pedir permissões de API em nome de um utilizador. Os clientes públicos são aplicativos como aplicativos móveis e aplicativos de javascript de página única que não é possível manter os segredos confidenciais. O procedimento é semelhante à [registar um cliente confidencial](register-confidential-azure-ad-client-app.md), mas uma vez que os clientes públicos não podem ser confiáveis para armazenar um segredo de aplicação, não é necessário para adicionar um.

## <a name="app-registrations-in-azure-portal"></a>Registos de aplicações no portal do Azure

1. No [portal do Azure](https://portal.azure.com), no painel de navegação do lado esquerdo, clique em **Azure Active Directory**.

2. Na **do Azure Active Directory** painel, clique em **registos das aplicações (pré-visualização)**:

    ![Portal do Azure. Novo registo de aplicação.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Clique nas **novo registo**.

## <a name="application-registration-overview"></a>Descrição geral do registo de aplicação

1. Dar ao aplicativo com um nome a apresentar.

2. Forneça um URL de resposta. O URL de resposta é onde os códigos de autenticação serão retornados para a aplicação cliente. Pode adicionar mais URLs de resposta e editar as já existentes mais tarde.

    ![Portal do Azure. Novo registo de aplicações pública.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>Permissões de API

Da mesma forma para o [aplicação de cliente confidencial](register-confidential-azure-ad-client-app.md), terá de selecionar quais as permissões de API esta aplicação deve conseguir pedir em nome dos utilizadores:

1. Abra o **permissões de API** e selecione sua [registo de aplicação de recurso de API FHIR](register-resource-azure-ad-client-app.md):

    ![Portal do Azure. Novas permissões de API pública.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-API-PERMISSIONS.png)

2. Selecione os âmbitos que gostaria que o aplicativo para conseguir pedir.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como registar uma aplicação de cliente público no Azure Active Directory. Em seguida, implemente uma API de FHIR no Azure.
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-oss-powershell-quickstart.md)
