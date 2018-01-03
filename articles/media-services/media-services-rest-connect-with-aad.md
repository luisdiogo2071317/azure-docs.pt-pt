---
title: "Autenticação de utilização do Azure AD para aceder à API de serviços de suporte de dados do Azure com o resto | Microsoft Docs"
description: "Saiba como aceder à API de serviços de suporte de dados do Azure com a autenticação do Azure Active Directory utilizando o resto."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/26/2017
ms.author: willzhan;juliako;johndeu
ms.openlocfilehash: ed78d6c6d4c695b841dbfbf917cd1681adc44ee7
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Utilize a autenticação do Azure AD para aceder à API de serviços de suporte de dados do Azure com REST

Quando estiver a utilizar a autenticação do Azure AD com Media Services do Azure, pode autenticar-se de uma das seguintes formas:

- **Autenticação de utilizador** efetua a autenticação de uma pessoa que está a utilizar a aplicação para interagir com recursos de Media Services do Azure. A aplicação interativa do primeiro deve solicitar credenciais ao utilizador. Um exemplo é uma aplicação de consola de gestão que é utilizada por utilizadores autorizados para monitorizar as tarefas de codificação ou transmissão em direto. 
- **Autenticação principal de serviço** autentica um serviço. As aplicações que utilizam frequentemente este método de autenticação são aplicações que executem serviços de daemon, serviços de camada média ou tarefas agendadas, tais como as web apps, aplicações de função, as logic apps, APIs ou micro-serviços.

    Este tutorial mostra como utilizar o Azure AD **principal de serviço** autenticação para aceder à API do AMS com REST. 

    > [!NOTE]
    > **Principal de serviço** é a melhor prática recomendada para a maioria das aplicações ligar aos Media Services do Azure. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Obter as informações de autenticação do portal do Azure
> * Obter o token de acesso utilizando o Postman
> * Teste o **ativos** API utilizando o token de acesso


> [!IMPORTANT]
> Atualmente, os Media Services suportam o modelo de autenticação de serviços de controlo de acesso do Azure. No entanto, vai ser preterida a autenticação de controlo de acesso 1 de Junho de 2018. Recomendamos que migre para o modelo de autenticação do Azure AD assim que for possível.

## <a name="prerequisites"></a>Pré-requisitos

- Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- [Criar uma conta de Media Services do Azure no portal do Azure](media-services-portal-create-account.md).
- Reveja o [aceder à API do Azure suporte de dados de serviços com descrição geral da autenticação do AAD](media-services-use-aad-auth-to-access-ams-api.md) artigo.
- Instalar o [Postman](https://www.getpostman.com/) cliente REST para executar as APIs REST apresentadas neste artigo. 

    Neste tutorial, estamos uring **Postman** , mas qualquer ferramenta REST seria adequada. Outras alternativas são: **Visual Studio Code** com o plug-in REST ou **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>Obter as informações de autenticação do portal do Azure

### <a name="overview"></a>Descrição geral

Para aceder à API de serviços de suporte de dados, terá de recolher os seguintes pontos de dados.

|Definição|Exemplo|Descrição|
|---|-------|-----|
|Domínio de inquilino do Azure Active Directory|Microsoft.onmicrosoft.com|Azure AD como um ponto final de proteger o Token serviço (STS) é criado utilizando o seguinte formato: https://login.microsoftonline.com/ {your-aad-tenant-name.onmicrosoft.com}/oauth2/token. O Azure AD emite um JWT para poder aceder aos recursos (um token de acesso).|
|Ponto final de API REST|https://amshelloworld.restv2.westus.Media.Azure.NET/API/|Este é o ponto final com que todas as API de REST de serviços de suporte de dados são efetuadas chamadas na sua aplicação.|
|ID de cliente (ID de aplicação)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|ID de aplicação (cliente) do Azure AD O ID de cliente é necessária para obter o token de acesso. |
|Segredo do Cliente|+ mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq + Dbim0 =|Chaves de aplicação do Azure AD (segredo do cliente). O segredo do cliente é necessária para obter o token de acesso.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>Obter informações de autenticação do AAD do portal do Azure

Para obter as informações, siga estes passos:

1. Inicie sessão no [Portal do Azure](http://portal.azure.com).
2. Navegue para a instância de AMS.
3. Selecione **acesso à API**.
4. Clique em **ligar a API de serviços de suporte de dados do Azure com o principal de serviço**.

    ![Acesso à API](./media/connect-with-rest/connect-with-rest01.png)

5. Selecione um existente **aplicação do Azure AD** ou crie um novo (mostrado abaixo).

    > [!NOTE]
    > Para o pedido de REST de suporte de dados do Azure com êxito, o utilizador chamado tem de ter um **contribuinte** ou **proprietário** função para a conta de Media Services está a tentar aceder. Se obtiver uma exceção que diz "o servidor remoto devolveu um erro: não autorizado (401)," consulte [controlo de acesso](media-services-use-aad-auth-to-access-ams-api.md#access-control).

    Se precisar de criar uma nova aplicação AD, siga estes passos:
    
    1. Prima **criar novos**.
    2. Introduza um nome.
    3. Prima **criar novo** novamente.
    4. Prima **guardar**.

    ![Acesso à API](./media/connect-with-rest/new-app.png)

    A nova aplicação aparece na página.

6. Obter o **ID de cliente** (ID de aplicação).
    
    1. Selecione a aplicação.
    2. Obter o **ID de cliente** na janela à direita. 

    ![Acesso à API](./media/connect-with-rest/existing-client-id.png).

7.  Obter a aplicação **chave** (segredo do cliente). 

    1. Clique em de **Gerir aplicação** botão (tenha em atenção que as informações de ID de cliente se encontra sob **ID da aplicação**). 
    2. Prima **chaves**.
    
        ![Acesso à API](./media/connect-with-rest/manage-app.png)
    3. Gerar a chave de aplicação (segredo do cliente) por preencher **Descrição** e **EXPIRA** e premindo **guardar**.
    
        Uma vez a **guardar** botão é premido, é apresentado o valor da chave. Copie o valor da chave antes de deixarem de painel.

    ![Acesso à API](./media/connect-with-rest/connect-with-rest03.png)

Pode adicionar valores para parâmetros de ligação do AD para o ficheiro Web. config ou o App. config, para utilização posterior no seu código.

> [!IMPORTANT]
> O **chave cliente** é um segredo importante e deve estar corretamente protegida no Cofre de chaves ou encriptados na produção.

## <a name="get-the-access-token-using-postman"></a>Obter o token de acesso utilizando o Postman

Esta secção mostra como utilizar **Postman** para executar uma API de REST que devolva um Token de portador do JWT (token de acesso). Para chamar qualquer API de REST de serviços de suporte de dados, tem de adicionar o cabeçalho "Authorization" para as chamadas e adicione o valor de "portador *your_access_token*" para cada chamada (conforme ilustrado na secção seguinte deste tutorial). 

1. Abra **Postman**.
2. Selecione **PUBLICAR**.
3. Introduza o URL que inclua o nome do inquilino com o seguinte formato: o nome de inquilino deve terminar com **. onmicrosoft.com** e o URL deve terminar com **oauth2/token**: 

    https://login.microsoftonline.com/ {your-aad-tenant-name.onmicrosoft.com}/oauth2/token

4. Selecione o **cabeçalhos** separador.
5. Introduza o **cabeçalhos** informações utilizando a grelha de dados de "Chave/valor". 

    ![Grelha de dados](./media/connect-with-rest/headers-data-grid.png)

    Em alternativa, clique em **editar em massa** ligação no lado direito da janela Postman e cole o seguinte código.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. Prima a **corpo** separador.
7. Introduza as informações de corpo utilizando a grelha de dados "Chave/valor" (substituir o ID de cliente e segredos valores). 

    ![Grelha de dados](./media/connect-with-rest/data-grid.png)

    Em alternativa, clique em **editar em massa** no lado direito da janela de Postman e cole o seguinte corpo (substituir o ID de cliente e segredos valores):

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your AAD Application}
        client_secret:{Your client secret that you got from your AAD Application's Keys}
        resource:https://rest.media.azure.net

8. Prima **Enviar**.

    ![Obtenha o token](./media/connect-with-rest/connect-with-rest04.png)

A resposta devolvida contém o **token de acesso** que terá de utilizar para aceder a qualquer APIs do AMS.

## <a name="test-the-assets-api-using-the-access-token"></a>Teste o **ativos** API utilizando o token de acesso

Esta secção mostra como pode aceder a **ativos** API utilizando **Postman**.

1. Abra **Postman**.
2. Selecione **OBTER**.
3. Cole o ponto final de REST API (por exemplo, https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. Selecione o **autorização** separador. 
5. Selecione **Token de portador**.
6. Cole o token que foi criado na secção anterior.

    ![Obtenha o token](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > O UX Postman pode ser diferente entre um Mac e o PC. Se a versão de Mac não tem a opção "Portador Token" no **autenticação** pendente da secção, deve adicionar o **autorização** cabeçalho manualmente no cliente Mac.

   ![Cabeçalho de autenticação](./media/connect-with-rest/auth-header.png)

7. Selecione **cabeçalhos**.
5. Clique em **editar em massa** ligação no lado direito da janela de Postman.
6. Cole os cabeçalhos seguintes:

        x-ms-version:2.15
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. Prima **Enviar**.

A resposta devolvida contém os elementos que estão na sua conta.

## <a name="next-steps"></a>Passos Seguintes

* Repita este código de exemplo no [do Azure AD Authentication para acesso de serviços de suporte de dados do Azure: ambos através da REST API](https://github.com/willzhan/WAMSRESTSoln)
* [Carregar ficheiros com o .NET](media-services-dotnet-upload-files.md)
