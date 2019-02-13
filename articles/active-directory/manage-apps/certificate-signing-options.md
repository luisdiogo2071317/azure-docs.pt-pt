---
title: Opções no token SAML para aplicações previamente integradas no Azure Active Directory de assinatura de certificado avançada | Documentos da Microsoft
description: Saiba como utilizar as opções no token SAML para aplicações previamente integradas no Azure Active Directory de assinatura de certificado avançado
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fc60d137c45abb99dd029a42c45e8575fc9cede
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182069"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Advanced opções no token SAML para aplicações de galeria no Azure Active Directory de assinatura de certificado
Atualmente o Azure Active Directory (Azure AD) suporta milhares de aplicações previamente integradas na Galeria de aplicações do Azure Active Directory. Este número inclui mais de 500 aplicações que suportam o início de sessão único com o protocolo SAML 2.0. Quando um utilizador efetua a autenticação para uma aplicação através do Azure AD com o SAML, o Azure AD envia um token para a aplicação (através de um HTTP POST). Em seguida, o aplicativo valida e utiliza o token para fazer logon do usuário em vez de solicitar um nome de utilizador e palavra-passe. Estes tokens SAML são assinadas com o certificado exclusivo que é gerado no Azure AD e pelos algoritmos padrão específicos.

Azure AD utiliza algumas das configurações padrão para as aplicações de galeria. Os valores predefinidos são configurados com base nos requisitos da aplicação.

O Azure AD suporta as definições de assinatura de certificado avançado. Para selecionar estas opções, selecione o **Mostrar definições de assinatura de certificado avançadas** caixa de verificação:

![Mostrar definições de assinatura de certificado avançadas](./media/certificate-signing-options/saml-advance-certificate.png)

Depois de selecionar esta caixa de verificação, pode configurar opções de assinatura de certificado e o algoritmo de assinatura de certificado.

## <a name="certificate-signing-options"></a>Opções de assinatura de certificado

O Azure AD suporta três opções de assinatura de certificado:

* **Assinar asserção SAML**. Esta opção de predefinição está definida para a maioria das aplicações de galeria. Se esta opção estiver selecionada, o Azure AD como um IdP assina a asserção de SAML e o certificado com o X509 certificado da aplicação. Além disso, ele usa o algoritmo de assinatura, que é selecionado na **algoritmo de assinatura** na lista pendente.

* **Assinar resposta SAML**. Se esta opção estiver selecionada, o Azure AD como um IdP assina a resposta SAML com X509 certificado da aplicação. Além disso, ele usa o algoritmo de assinatura, que é selecionado na **algoritmo de assinatura** na lista pendente.

* **Assinar asserção e resposta SAML**. Se esta opção estiver selecionada, o Azure AD como um IdP assina o token SAML todo com X509 certificado da aplicação. Além disso, ele usa o algoritmo de assinatura, que é selecionado na **algoritmo de assinatura** na lista pendente.

    ![Opções de assinatura de certificado](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Algoritmos de assinatura de certificado

O Azure AD suporta dois algoritmos de assinatura para assinar resposta SAML:

* **SHA-256**. Azure AD utiliza esse algoritmo padrão para assinar resposta SAML. Ele é o algoritmo mais recente e é tratado como mais segura do que o SHA-1. A maioria dos aplicativos de suportar o algoritmo SHA-256. Se uma aplicação suporta apenas SHA-1, como o algoritmo de assinatura, pode alterá-lo. Caso contrário, recomendamos que utilize o algoritmo SHA-256 para assinar resposta SAML.

    ![Algoritmo de assinatura de certificado de SHA-256](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**. Este é o algoritmo mais antigo, e ela é tratada como menos seguro do que SH-256. Se uma aplicação suportar apenas este algoritmo de assinatura, pode selecionar esta opção no **algoritmo de assinatura** na lista pendente. O Azure AD, em seguida, inicia a resposta SAML com o algoritmo SHA-1.

    ![Algoritmo de assinatura de certificado de SHA-1](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Passos Seguintes
* [Configurar o início de sessão único para aplicações que não estão na Galeria de aplicações do Azure Active Directory](configure-federated-single-sign-on-non-gallery-applications.md)
* [Resolver problemas com base em SAML início de sessão único](../develop/howto-v1-debug-saml-sso-issues.md)


