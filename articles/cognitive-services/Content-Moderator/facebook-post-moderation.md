---
title: Moderação de interrupção conteúda do Facebook com Moderator de conteúdo do Azure | Microsoft Docs
description: Páginas de Facebook moderadas com machine learning com base em Moderator conteúdo
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 316c7212c30e9fe1151cdf5ceabf875439ad4c65
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351866"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Moderação de interrupção conteúda do Facebook com Moderator de conteúdo

Neste tutorial, vamos aprender a utilizar o machine learning baseada em Moderator conteúdo para ajudar a moderada Facebook publicações e comentários.

O tutorial orienta-o estes passos:

1. Crie um agrupamento de conteúdo Moderator.
2. Crie as funções do Azure que escutar eventos HTTP de conteúdo Moderator e o Facebook.
3. Criar uma página do Facebook e a aplicação e ligá-lo ao Moderator conteúdo.

Depois de foi terminados, Facebook irá enviar o conteúdo publicado por de visitantes Moderator de conteúdo. Com base nos limiares correspondência, os fluxos de trabalho de conteúdo Moderator publicar o conteúdo ou criar revisões dentro da ferramenta de revisão. 

A figura seguinte mostra os blocos modulares da solução.

![Moderação de publicações do Facebook](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Criar um agrupamento de conteúdo Moderator

Consulte o [início rápido](quick-start.md) página inscreva Moderator conteúdo e criar um agrupamento.

## <a name="configure-image-moderation-workflow-threshold"></a>Configurar fluxo de trabalho do moderação interrupção de imagem (limiar)

Consulte o [fluxos de trabalho](review-tool-user-guide/workflows.md) página para configurar um fluxo de trabalho para a imagem personalizada (limiar). Tenha em atenção o fluxo de trabalho **nome**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Configurar fluxo de trabalho do moderação interrupção de texto (limiar)

Utilize os passos semelhantes para o [fluxos de trabalho](review-tool-user-guide/workflows.md) página para configurar um limiar de texto personalizado e um fluxo de trabalho. Tenha em atenção o fluxo de trabalho **nome**.

![Configurar fluxo de trabalho de texto](images/text-workflow-configure.PNG)

Teste o fluxo de trabalho com o botão "Executar o fluxo de trabalho".

![Fluxo de trabalho de texto de teste](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Criar as funções do Azure

Iniciar sessão para o [Azure Management Portal](https://portal.azure.com/) para criar as suas funções do Azure. Siga estes passos.

1. Criar uma aplicação de função do Azure, conforme mostrado no [das funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página.
2. Abra a aplicação de função recentemente criado.
3. Na aplicação, navegue para **funcionalidades de plataforma -> definições da aplicação**
4. Definir o seguinte [definições da aplicação](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> O **cm: região** deve ser o nome da região (sem quaisquer espaços).
> Por exemplo, **westeurope**, não Europa Ocidental, **westcentralus**, não Central EUA oeste e assim sucessivamente.
>

| Definição de aplicação | Descrição   | 
| -------------------- |-------------|
| cm:TeamId   | O conteúdo Moderator TeamId  | 
| cm:SubscriptionKey | A chave de subscrição de conteúdo Moderator - consulte [credenciais](/review-tool-user-guide/credentials.md) | 
| cm:Region | O nome de região Moderator conteúdo, sem os espaços. Consulte a nota anterior. |
| cm:ImageWorkflow | Nome do fluxo de trabalho para executar nas imagens |
| cm:TextWorkflow | Nome do fluxo de trabalho para executar em texto |
| cm:CallbackEndpoint | URL da aplicação de função CMListener que cria posteriormente neste guia |
| FB:VerificationToken | O token SECRETO, também é utilizado para subscrever o Facebook feed eventos |
| FB:PageAccessToken | O token de acesso de api de gráfico do Facebook não expire e permite que a função ocultar/eliminar publicações em seu nome. |

5. Crie um novo **HttpTrigger CSharp** função com o nome **FBListener**. Esta função recebe eventos do Facebook. Crie esta função, seguindo estes passos:

    1. Manter o [criação de funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página aberta para referência.
    2. Clique em de **+** adicionar para criar uma nova função.
    3. Em vez de modelos incorporados, escolha o **começar a utilizar na sua função de proprietário/personalizada** opção.
    4. Clique no mosaico que indica que **HttpTrigger CSharp**.
    5. Introduza o nome **FBListener**. O **nível de autorização** campo deve ser definido como **função**.
    6. Clique em **Criar**.
    7. Substitua os conteúdos do **run.csx** com o conteúdo do [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Crie um novo **HttpTrigger CSharp** função com o nome **CMListener**. Esta função recebe eventos do Facebook. Siga estes passos para criar esta função.

    1. Manter o [criação de funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página aberta para referência.
    2. Clique em de **+** adicionar para criar uma nova função.
    3. Em vez de modelos incorporados, escolha o **começar a utilizar na sua função de proprietário/personalizada** opção.
    4. Clique no mosaico que indica que **HttpTrigger CSharp**
    5. Introduza o nome **CMListener**. O **nível de autorização** campo deve ser definido como **função**.
    6. Clique em **Criar**.
    7. Substitua os conteúdos do **run.csx** com o conteúdo do [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Configurar a página do Facebook e aplicação
1. Crie uma aplicação do Facebook.

    1. Navegue para o [site de programador do Facebook](https://developers.facebook.com/)
    2. Clique em **as minhas aplicações**.
    3. Adicione uma nova aplicação.
    4. Selecione **Webhooks -> Get iniciado**
    5. Selecione **página -> subscrever a este tópico**
    6. Forneça o **FBListener Url** como o URL de chamada de retorno e o **verificar Token** configurado no **as definições de aplicação de função**
    7. Depois de subscrever, desloque para baixo até o feed e selecione **subscrever**.

2. Crie uma página do Facebook.

    1. Navegue para [Facebook](https://www.facebook.com/bookmarks/pages) e criar um **nova página do Facebook**.
    2. Permitir que a aplicação do Facebook aceder a esta página seguindo estes passos:
        1. Navegue para o [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
        2. Selecione **aplicação**.
        3. Selecione **Token de acesso de página**, enviar uma **obter** pedido.
        4. Clique em de **ID da página** na resposta.
        5. Agora acrescentar o **/subscribed_apps** para o URL e enviar um **obter** pedido (resposta vazia).
        6. Submeter um **Post** pedido. Obter a resposta como **êxito: true**.

3. Crie um token de acesso de Graph API não expira.

    1. Navegue para o [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
    2. Selecione o **aplicação** opção.
    3. Selecione o **obter acesso Token de utilizador** opção.
    4. Sob o **selecionar permissões**, selecione **manage_pages** e **publish_pages** opções.
    5. Utilizaremos o **token de acesso** (abreviado Lived Token) no próximo passo.

4. Utilizamos o Postman para os próximos passos.

    1. Abra **Postman** (ou obtê-lo [aqui](https://www.getpostman.com/)).
    2. Importe estes dois ficheiros:
        1. [Coleção de postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Ambiente de postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Estas variáveis de ambiente de atualização:
    
    | Chave | Valor   | 
    | -------------------- |-------------|
    | appId   | Inserir o identificador de aplicação do Facebook  | 
    | appSecret | Inserir aqui a segredo da aplicação do Facebook | 
    | short_lived_token | Inserir o token de acesso de utilizador suma lived gerados no passo anterior |
    4. Agora, execute o 3 APIs listados na coleção: 
        1. Selecione **gerar Long-Lived acesso Token** e clique em **enviar**.
        2. Selecione **obter ID de utilizador** e clique em **enviar**.
        3. Selecione **obter o Token de acesso de permanente página** e clique em **enviar**.
    5. Copiar o **access_token** valor da resposta e atribua-a para a definição de aplicação, **fb:PageAccessToken**.

Já está!

A solução envia todas as imagens e texto publicado na sua página do Facebook Moderator conteúdo. Os fluxos de trabalho que configurou anteriormente são invocados. O conteúdo que não cumpre os critérios definidos nos resultados de fluxos de trabalho no revisões dentro da ferramenta de revisão. O resto do conteúdo seja publicado.

## <a name="license"></a>Licença

Todos os Microsoft cognitivos serviços SDKs e amostras são licenciadas com a licença MIT. Para obter mais detalhes, consulte [licença](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Código de Conduta do Programador

Os programadores utilizando serviços cognitivos, incluindo esta biblioteca de cliente & exemplo, são esperados para seguir o "Programador código de conduta para Microsoft serviços cognitivos", encontrado em http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Passos Seguintes

1. [Veja uma demonstração (vídeo)](https://channel9.msdn.com/Events/Build/2017/T6033) desta solução da Microsoft compilação 2017.
1. [O exemplo do Facebook no Github](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
