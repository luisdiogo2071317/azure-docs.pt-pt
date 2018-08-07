---
title: Gerir certificados de Federação no Azure AD | Documentos da Microsoft
description: Saiba como personalizar a data de expiração para os certificados de Federação e como renovar certificados que irão expirar em breve.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/09/2017
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 7bae891bd16ecd3fbbad88022fbbffd32ff41eae
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577554"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Gerir certificados para início de sessão único federado no Azure Active Directory
Este artigo aborda as questões comuns e informações relacionadas com os certificados que cria do Azure Active Directory (Azure AD) para estabelecer único início de sessão Federado (SSO) às suas aplicações SaaS. Adicione aplicações de Galeria de aplicações do Azure AD ou utilizando um modelo de aplicativo externas à galeria. Configure a aplicação ao utilizar a opção de SSO federada.

Este artigo é relevante apenas para aplicações que estão configuradas para utilizar o SSO do Azure AD através de Federação de SAML, conforme mostrado no exemplo a seguir:

![O Azure AD Single Sign-On](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Certificado gerado automaticamente para as aplicações de galeria e externas à Galeria
Quando adicionar um novo aplicativo a partir da Galeria e configurar um SAML com base no início de sessão, o Azure AD gera um certificado para a aplicação que é válido durante três anos. Pode transferir este certificado a partir da **certificado de assinatura SAML** secção. Para aplicações de galeria, esta secção poderá mostrar uma opção para transferir o certificado ou metadados, dependendo do requisito da aplicação.

![O Azure AD início de sessão único](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>Personalizar a data de expiração para o seu certificado de Federação e acumulam-lo para um novo certificado
Por predefinição, os certificados estão definidos para expirar após três anos. Pode escolher uma data de expiração diferentes para o seu certificado, concluindo os passos seguintes.
As capturas de ecrã utilizam o Salesforce no exemplo, mas estes passos, podem aplicar a todas as aplicações SaaS federadas.

1. Na [portal do Azure](https://aad.portal.azure.com), clique em **aplicação empresarial** no painel esquerdo e clique em **novo aplicativo** no **descrição geral** página:

   ![Abrir o Assistente de configuração de SSO](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Procure a aplicação da Galeria e, em seguida, selecione a aplicação que pretende adicionar. Se não conseguir encontrar a aplicação necessária, adicionar a aplicação utilizando o **aplicação da Galeria não** opção. Esta funcionalidade está disponível apenas no SKU do Azure AD Premium (P1 e P2).

    ![O Azure AD início de sessão único](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Clique nas **início de sessão único** ligação no painel esquerdo e altere **modo de início de sessão único** para **baseado em SAML início de sessão**. Este passo gera um certificado de três anos para a sua aplicação.

4. Para criar um novo certificado, clique a **criar novo certificado** ligação na **certificado de assinatura SAML** secção.

    ![Gerar um novo certificado](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. O **criar um novo certificado** link abre o controle de calendário. Pode definir qualquer data e hora até três anos a contar da data atual. A data selecionada e a hora é a nova data de expiração e hora do seu novo certificado. Clique em **Guardar**.

    ![Transferir, em seguida, carregue o certificado](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Agora o novo certificado está disponível para download. Clique nas **certificado** link para baixá-lo. Neste momento, o certificado não está ativo. Quando quiser acumulam para este certificado, selecione o **ativar o novo certificado** caixa de verificação e clique em **guardar**. A partir desse ponto, do Azure AD é iniciado com o novo certificado para assinar a resposta.

7.  Para saber como carregar o certificado para a aplicação SaaS específica, clique a **tutorial de configuração de aplicação de vista** ligação.

## <a name="certificate-expiration-notification-email"></a>E-mail de notificação de expiração do certificado

Azure AD irá enviar um dias de 7, 30 e 60 de notificação de e-mail antes de expira o certificado SAML. Para especificar o endereço de e-mail para onde enviar a notificação:

- No Azure Active Directory página da aplicação único início de sessão, vá para o campo de E-Mail de notificação.
- Introduza o endereço de e-mail que deve receber o e-mail de notificação de expiração do certificado. Por predefinição, este campo utiliza o endereço de e-mail do administrador que adicionaram a aplicação de.

Irá receber o e-mail de notificação da aadnotification@microsoft.com. Para evitar o seu local de spam de e-mail, certifique-se de que adicionar este e-mail aos seus contatos. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Renovar um certificado que irá expirar em breve
Os seguintes passos de renovação devem resultar em sem períodos de indisponibilidade significativo para os seus utilizadores. As capturas de ecrã esta funcionalidade de secção Salesforce como um exemplo, mas estes passos, podem aplicar a todas as aplicações SaaS federadas.

1. Sobre o **do Azure Active Directory** aplicativo **início de sessão único** página, gerar o novo certificado para a sua aplicação. Pode fazê-lo ao clicar o **criar novo certificado** ligação na **certificado de assinatura SAML** secção.

    ![Gerar um novo certificado](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Selecione a data de expiração desejada e a hora para o novo certificado e clique em **guardar**.

3. Transferir o certificado no **certificado de assinatura de SAML** opção. Carregar o novo certificado para o ecrã de configuração de início de sessão único da aplicação SaaS. Para saber como fazê-lo para a sua aplicação SaaS específica, clique a **tutorial de configuração de aplicação de vista** ligação.
   
4. Para ativar o novo certificado no Azure AD, selecione o **ativar o novo certificado** caixa de verificação e clique nas **guardar** botão na parte superior da página. Isso é agregado ao longo do novo certificado no lado do Azure AD. O estado do certificado é alterado de **New** ao **Active Directory**. A partir desse ponto, do Azure AD é iniciado com o novo certificado para assinar a resposta. 
   
    ![Gerar um novo certificado](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>Artigos relacionados
* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](../saas-apps/tutorial-list.md)
* [Índice de artigo para a gestão de aplicações no Azure Active Directory](../active-directory-apps-index.md)
* [Acesso a aplicações e início de sessão único com o Azure Active Directory](what-is-single-sign-on.md)
* [Resolução de problemas com base em SAML início de sessão único](../develop/howto-v1-debug-saml-sso-issues.md)
