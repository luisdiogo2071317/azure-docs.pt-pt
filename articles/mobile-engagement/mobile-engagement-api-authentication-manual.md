---
title: "Autenticar com APIs REST do Mobile Engagement - configuração manual"
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
ms.openlocfilehash: 3b678acbae225c28223a2ee76e5be2462a529362
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Autenticar com APIs REST do Mobile Engagement - configuração manual
Esta documentação é uma documentação apêndice para [autenticar com as APIs REST do Mobile Engagement](mobile-engagement-api-authentication.md). Certifique-se de que lê-lo primeiro para obter o contexto.
Descreve uma maneira alternativa a única configuração para configurar a autenticação para as APIs REST do Mobile Engagement no portal do Azure.

> [!NOTE]
> As instruções que se seguem baseiam-se neste [Guia do Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) e personalizada para o que é necessário para a autenticação de APIs do Mobile Engagement. Por isso, consulte-la se pretender que a compreender os passos abaixo em detalhe.

1. Inicie sessão na sua conta do Azure através de [portal do Azure](https://portal.azure.com/).
2. Selecione **do Active Directory** no painel esquerdo.

     ![Selecione o Active Directory][1]

3. Para ver as aplicações no seu diretório, clique em **registos de aplicação**.

     ![Ver aplicações][3]

4. Clique em **novo registo de aplicação**.

     ![Adicionar aplicação][4]

5. Preencha o nome da aplicação e deixe o tipo de aplicação como **aplicação/API Web** e clique no botão seguinte. Pode fornecer os URLs fictícias para **URL de início de sessão no**: não são utilizados para este cenário e os URLs próprios não são validados.
6. Quando tiver terminado, terá uma aplicação do Azure AD com o nome que forneceu. É o **AD\_aplicação\_nome**, tome nota do mesmo.

     ![nome da aplicação][8]

7. Clique no nome da aplicação.
8. Localizar **ID da aplicação**, tome nota do mesmo, está a ser o ID de cliente que está a ser utilizado como **cliente\_ID** para a API chamadas.

     ![Configure a aplicação][10]

9. Localizar o **chaves** secção à direita.

     ![Configure a aplicação][11]

10. Criar uma nova chave imediatamente copiá-lo e guardá-lo para utilização. Nunca irá ser apresentada novamente.

     ![Configure a aplicação][12]

    > [!IMPORTANT]
    > Esta chave expira no final da duração que especificou, pelo que certifique-se renová-lo quando o tempo vem caso contrário, a autenticação de API deixa de funcionar. Também pode eliminar e recriar esta chave, se pensa que o se tiver sido comprometido.
    >
    >
11. Clique em **pontos finais** botão na parte superior da página e copiar a **ponto final de TOKENS do OAUTH 2.0**.

    ![][14]

16. Este ponto final será o seguinte formato em que é o GUID no URL do seu **TENANT_ID** , por isso, tome nota do mesmo:`https://login.microsoftonline.com/<GUID>/oauth2/token`
17. Agora iremos irá continuar a configurar as permissões nesta aplicação. Para este terá de abrir o [portal do Azure](https://portal.azure.com). 
18. Clique em **grupos de recursos** e localize o **Mobile Engagement** grupo de recursos.

    ![][15]

19. Clique em de **Mobile Engagement** recursos de grupo e navegue para o **definições** secção aqui.

    ![][16]

20. Clique em **utilizadores** nas definições de secção e, em seguida, clique em **adicionar** para adicionar um utilizador.

    ![][17]

21. Clique em **selecionar uma função**.

    ![][18]

22. Clique em **proprietário**.

    ![][19]

23. Procure o nome da sua aplicação **AD\_aplicação\_nome** na caixa de pesquisa. Não verá esta por predefinição aqui. Depois de localizar, selecione-o e clique em **selecione** na parte inferior da secção.

    ![][20]

24. No **adicionar acesso** secção, irá aparecer como **1 utilizador, grupos de 0**. Clique em **OK** nesta secção para confirmar a alteração.

    ![][21]

Agora que concluiu o necessária configuração do Azure AD e são tudo pronto para chamar as APIs.

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



