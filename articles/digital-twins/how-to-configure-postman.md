---
title: Como configurar o Postman duplos Digital do Azure | Documentos da Microsoft
description: Como configurar o Postman duplos Digital do Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.openlocfilehash: 705d96225c699c6e9824dadbd7fa1272ba5ddb2d
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213285"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Como configurar o Postman duplos Digital do Azure

Este artigo descreve como configurar o cliente de REST do Postman para interagir com e testar as APIs de gestão de duplos Digital do Azure. Especificamente, ele descreve:

* como configurar uma aplicação do Azure Active Directory para utilizar o fluxo de concessão implícita OAuth 2.0.
* como configurar o cliente de REST do Postman para fazer pedidos HTTP do efeito de token para as suas APIs de gestão.

## <a name="postman-summary"></a>Resumo do postman

Começar a duplos Digital do Azure, utilizando uma ferramenta de cliente REST como [Postman](https://www.getpostman.com/) para preparar o ambiente de teste local. O cliente do Postman ajuda a criar rapidamente complexos pedidos HTTP. Baixe a versão de área de trabalho do cliente do Postman ao aceder [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) é uma REST ferramenta de teste que localiza as principais funcionalidades de pedido HTTP num ambiente de trabalho útil e baseada em Plug-in GUI. Por meio do cliente do Postman, os desenvolvedores de soluções podem especificar o tipo de pedido HTTP (*POST*, *obter*, *ATUALIZAÇÃO*, *PATCH*e o  *ELIMINAR*), ponto final de API para chamar e a utilização do SSL. Postman também oferece suporte a adicionar cabeçalhos de pedido HTTP, parâmetros, dados do formulário e órgãos de desenvolvimento.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Configurar o Azure Active Directory para utilizar o fluxo de concessão implícita OAuth 2.0

Configure a sua aplicação do Azure Active Directory para utilizar o fluxo de concessão implícita OAuth 2.0.

1. Siga os passos em [este guia de introdução](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para criar uma aplicação do Azure AD do tipo nativo. Ou pode reutilizar um registo de aplicação nativa existente.

1. Sob **permissões obrigatórias**, selecione **Add** e introduza **duplos Digital do Azure** sob **adicionar acesso à API**. Se a pesquisa não localizar a API, procure **Azure Smart Spaces** como alternativa. Em seguida, selecione **conceder permissões > permissões delegadas** e **feito**.

    ![Api de adicionar registos de aplicações do Azure Active Directory](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Clique em **manifesto** para abrir o manifesto do aplicativo para a sua aplicação. Definir *oauth2AllowImplicitFlow* para `true`.

      ![Fluxo implícito de Active Directory do Azure][1]

1. Configurar uma **URL de resposta** para `https://www.getpostman.com/oauth2/callback`.

      ![URL de resposta do Azure Active Directory][2]

1. Copie o e mantenha os **ID da aplicação** da sua aplicação do Azure Active Directory. Ele é usado abaixo.

### <a name="configure-the-postman-client"></a>Configurar o cliente do Postman

Em seguida, definir e configurar o Postman para obter um token do Azure Active Directory. Em seguida, efetue um pedido HTTP autenticado no duplos Digital do Azure com o token obtido:

1. Aceda a [www.getpostman.com](https://www.getpostman.com/) para transferir a aplicação.
1. Certifique-se de que sua **URL de autorização** está correto. Deve demorar o formato:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Nome  | Substituir | Exemplo |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | O nome do seu inquilino ou a organização | `microsoft` |

1. Selecione o **autorização** separador, selecione **OAuth 2.0**e, em seguida, selecione **obter novo Token de acesso**.

    | Campo  | Valor |
    |---------|---------|
    | Tipo de Concessão | `Implicit` |
    | Url de chamada de retorno | `https://www.getpostman.com/oauth2/callback` |
    | Auth URL | Utilize o **URL de autorização** do passo 2 acima |
    | ID de Cliente | Utilize o **ID da aplicação** para a aplicação do Azure Active Directory que foi criada ou reobjetivada da seção anterior |
    | Âmbito | Deixar em branco |
    | Estado | Deixar em branco |
    | Autenticação de Cliente | `Send as Basic Auth header` |

1. O cliente deve agora ser semelhante:

   ![Exemplo de cliente do postman][3]

1. Selecione **pedir o Token**.

    >[!NOTE]
    >Se receber a mensagem de erro "Não foi possível concluir o OAuth 2", experimente o seguinte:
    > * Feche o Postman e abri-lo novamente e tente novamente.
  
1. Role para baixo e selecione **utilização Token**.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as APIs de gestão de duplos digitais e como utilizá-los, leia [como utilizar as APIs de gestão de duplos Digital do Azure](how-to-navigate-apis.md).

- Para saber mais sobre a autenticação com as APIs de gestão, leia [autenticar com as APIs](./security-authenticating-apis.md). 



<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png
