---
title: "Autenticar com as APIs REST do Mobile Engagement: configuração manual"
description: "Descreve como configurar manualmente a autenticação para as APIs REST do Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0b4a999c6778040e71f862d3a010b6635e84b26e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis-manual-setup"></a>Autenticar com as APIs REST do Mobile Engagement: configuração manual
Esta documentação é de um anexo para [autenticar com as APIs REST do Mobile Engagement](mobile-engagement-api-authentication.md). Certifique-se de que esse artigo primeiro para compreender o contexto de leitura. Também descreve uma maneira alternativa para efetuar a configuração de autenticação única para as APIs REST do Mobile Engagement através do portal do Azure.

> [!NOTE]
> As instruções que se seguem baseiam-se no [neste guia do Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md). Estes são personalizadas para os requisitos de autenticação para as APIs do Mobile Engagement. Consultá-las para compreender os passos seguintes em detalhe.

1. Inicie sessão na sua conta do Azure através de [portal do Azure](https://portal.azure.com/).
2. Selecione **do Active Directory** no painel esquerdo.

   ![Selecione o Active Directory][1]

3. Para ver as aplicações no seu diretório, selecione **registos de aplicação**.

   ![Ver aplicações][3]

4. Selecione **novo registo de aplicação**.

   ![Adicionar aplicação][4]

5. Preencha o nome da aplicação. Deixe o tipo de aplicação como **aplicação/API Web**e, em seguida, selecione o **seguinte** botão. Pode fornecer os URLs fictícias para **URL de início de sessão no**. Não são utilizados para este cenário e os URLs próprios não são validados.

   Depois de concluir, terá uma aplicação do Azure Active Directory (Azure AD) com o nome que forneceu. É o **AD\_aplicação\_nome**, por isso, lembre-se de que tome nota do mesmo.

   ![Nome da aplicação][8]

7. Selecione o nome da aplicação.

8. Localizar **ID da aplicação** e tome nota do mesmo. É o ID de cliente que será utilizado como **cliente\_ID** para a API chamadas.

   ![Localizar o ID de aplicação][10]

9. Localizar o **chaves** secção à direita.

   ![Secção de chaves][11]

10. Criar uma nova chave e, em seguida, copie-o imediatamente. Não é mostrado novamente.

    ![Painel de chaves com os detalhes da chave][12]

    > [!IMPORTANT]
    > Esta chave expira no final da duração que especificou. Confirme renová-lo quando o tempo vem, caso contrário, a autenticação de API não deixa de funcionar. Se considerar a que esta chave tiver sido comprometida, pode eliminar e voltar a criar.
    >
    
11. Selecione o **pontos finais** botão na parte superior da página. Em seguida, copie o **ponto final de TOKENS do OAUTH 2.0**.

    ![Copie o ponto final][14]

16. Este ponto final faz o seguinte formato, em que é o GUID no URL do seu **TENANT_ID**:`https://login.microsoftonline.com/<GUID>/oauth2/token`

17. Em seguida, configure as permissões sobre esta aplicação. Para iniciar o processo, vá para o [portal do Azure](https://portal.azure.com).

18. Selecione **grupos de recursos**e, em seguida, localize o **MobileEngagement** grupo de recursos.

    ![Localizar MobileEngagement][15]

19. Selecione o **MobileEngagement** recurso, grupo e, em seguida, selecione **todas as definições**.

    ![Navegue para as definições para MobileEngagement][16]

20. Selecione **utilizadores** no **definições** secção. Em seguida, para adicionar um utilizador, selecione **adicionar**.

    ![Adicionar um utilizador][17]

21. Clique em **selecionar uma função**.

    ![Selecionar uma função][18]

22. Selecione **proprietário**.

    ![Selecione o proprietário como função][19]

23. Procure o nome da sua aplicação, **AD\_aplicação\_nome**, na caixa de pesquisa. Este nome não está disponível por predefinição. Depois de encontrá-lo, selecione-a. Em seguida, clique em **selecione** na parte inferior da secção.

    ![Selecione o nome][20]

24. No **adicionar acesso** secção, é apresentada como **1 utilizador, grupos de 0**. Para confirmar a alteração, selecione **OK**.

    ![Confirme o utilizador adicionado][21]

Agora que concluiu o necessária configuração do Azure AD e são todos os definir para chamar as APIs.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



