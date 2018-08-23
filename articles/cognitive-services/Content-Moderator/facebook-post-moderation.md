---
title: Moderação de conteúdos do Facebook com o Azure Content Moderator | Documentos da Microsoft
description: Páginas de Facebook moderadas com aprendizagem automática com base em Content Moderator
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 4570b514bef33c8dfa6d220ee4cd88ad068ba4d0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "41987964"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Moderação de conteúdos do Facebook com o Content Moderator

Neste tutorial, vamos aprender a utilizar baseados em machine-learning Content Moderator para ajudar a moderado publicações do Facebook e seus comentários.

O tutorial orienta-o ao longo destes passos:

1. Crie uma equipa de Content Moderator.
2. Crie funções do Azure que escutar eventos HTTP do Content Moderator e o Facebook.
3. Criar uma aplicação e a página do Facebook e ligá-lo ao Content Moderator.

Depois que terminar, Facebook enviará o conteúdo publicado por dos visitantes moderador de conteúdo. Com base nos limiares de correspondência, os fluxos de trabalho do Content Moderator ou publicar o conteúdo ou criar revisões dentro da ferramenta de revisão. 

A figura seguinte mostra os blocos de construção da solução.

![Moderação de publicações do Facebook](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Criar uma equipa de Content Moderator

Consulte a [guia de introdução](quick-start.md) página para inscrever-se o Content Moderator e criar uma equipa.

## <a name="configure-image-moderation-workflow-threshold"></a>Configurar fluxo de trabalho do moderação de imagem (threshold)

Consulte a [fluxos de trabalho](review-tool-user-guide/workflows.md) página para configurar um fluxo de trabalho para a imagem personalizada (threshold). Tenha em atenção o fluxo de trabalho **nome**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Configurar fluxo de trabalho do moderação de texto (threshold)

Utilize os passos semelhantes para o [fluxos de trabalho](review-tool-user-guide/workflows.md) página para configurar um limiar de texto personalizado e o fluxo de trabalho. Tenha em atenção o fluxo de trabalho **nome**.

![Configurar fluxo de trabalho de texto](images/text-workflow-configure.PNG)

Teste o fluxo de trabalho com o botão "Executar o fluxo de trabalho".

![Fluxo de trabalho de texto de teste](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Criar as funções do Azure

Inicie sessão para o [Portal de gestão do Azure](https://portal.azure.com/) para criar as suas funções do Azure. Siga estes passos.

1. Criar uma aplicação de função do Azure, como mostra a [as funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página.
2. Abra a aplicação de função recentemente criada.
3. Na aplicação, navegue para **funcionalidades de plataforma -> definições da aplicação**
4. Defina as seguintes opções [as definições da aplicação](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> O **cm: região** deve ser o nome da região (sem espaços).
> Por exemplo, **westeurope**, não a Europa Ocidental, **westcentralus**, não e.u.a. centro-oeste e assim por diante.
>

| Definição de aplicação | Descrição   | 
| -------------------- |-------------|
| cm:TeamId   | Sua TeamId o Content Moderator  | 
| cm:SubscriptionKey | A chave de subscrição do Content Moderator - consulte [credenciais](/review-tool-user-guide/credentials.md) | 
| cm:Region | O nome da região Content Moderator, sem os espaços. Consulte nota anterior. |
| cm:ImageWorkflow | Nome do fluxo de trabalho para serem executadas em imagens |
| cm:TextWorkflow | Nome do fluxo de trabalho para serem executadas em texto |
| cm:CallbackEndpoint | URL da aplicação de função de CMListener que cria mais tarde neste guia |
| FB:VerificationToken | Token SECRETO, também utilizado para subscrever o Facebook feed de eventos |
| FB:PageAccessToken | O token de acesso de api de gráfico do Facebook não expire e permite que a função ocultar/eliminar postagens em seu nome. |

5. Criar uma nova **HttpTrigger-CSharp** função chamada **FBListener**. Esta função recebe eventos do Facebook. Crie esta função, seguindo estes passos:

    1. Manter o [criação de funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página aberta para referência.
    2. Clique nas **+** adicionar para criar a nova função.
    3. Em vez dos modelos internos, escolha o **começar a sua função de proprietário/personalizado** opção.
    4. Clique no mosaico que diz **HttpTrigger-CSharp**.
    5. Introduza o nome **FBListener**. O **nível de autorização** campo deve ser definido como **função**.
    6. Clique em **Criar**.
    7. Substitua os conteúdos do **csx** com o conteúdo a partir do [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Criar uma nova **HttpTrigger-CSharp** função chamada **CMListener**. Esta função recebe eventos do Content Moderator. Siga estes passos para criar esta função.

    1. Manter o [criação de funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) página aberta para referência.
    2. Clique nas **+** adicionar para criar a nova função.
    3. Em vez dos modelos internos, escolha o **começar a sua função de proprietário/personalizado** opção.
    4. Clique no mosaico que diz **HttpTrigger-CSharp**
    5. Introduza o nome **CMListener**. O **nível de autorização** campo deve ser definido como **função**.
    6. Clique em **Criar**.
    7. Substitua os conteúdos do **csx** com o conteúdo a partir do [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Configurar a página de Facebook e a aplicação
1. Crie uma aplicação do Facebook.

    1. Navegue para o [site de desenvolvedor do Facebook](https://developers.facebook.com/)
    2. Clique em **as minhas aplicações**.
    3. Adicione uma nova aplicação.
    4. Selecione **Webhooks -> Get iniciado**
    5. Selecione **página -> inscrever-se a este tópico**
    6. Forneça o **FBListener Url** como o URL de retorno de chamada e o **verificar Token** que configurou no **definições da aplicação de funções**
    7. Uma vez inscrito, desloque-se para baixo para o feed e selecione **subscrever**.

2. Crie uma página de Facebook.

    1. Navegue para [Facebook](https://www.facebook.com/bookmarks/pages) e criar um **nova página de Facebook**.
    2. Permitir que a aplicação de Facebook acessar esta página através dos seguintes passos:
        1. Navegue para o [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
        2. Selecione **aplicativo**.
        3. Selecione **Token de acesso da página**, enviar um **obter** pedido.
        4. Clique nas **ID de página** na resposta.
        5. Agora acrescentar a **/subscribed_apps** ao URL e enviar um **obter** pedido (resposta vazia).
        6. Submeter uma **Post** pedido. Obtém a resposta como **sucesso: Verdadeiro**.

3. Crie um token de acesso do Graph API que não expira.

    1. Navegue para o [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
    2. Selecione o **aplicativo** opção.
    3. Selecione o **obter utilizador acesso Token** opção.
    4. Sob o **selecionar permissões**, selecione **manage_pages** e **publish_pages** opções.
    5. Nós usaremos o **token de acesso** (breve Vivi Token) no próximo passo.

4. Utilizamos o Postman para os passos seguintes.

    1. Open **Postman** (ou obtê-lo [aqui](https://www.getpostman.com/)).
    2. Importe esses dois arquivos:
        1. [Coleção do postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Ambiente do postman](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Estas variáveis de ambiente de atualização:
    
    | Chave | Valor   | 
    | -------------------- |-------------|
    | appId   | Inserir o identificador de aplicação do Facebook  | 
    | appSecret | Insira aqui a segredo da aplicação do Facebook | 
    | short_lived_token | Insira o token de acesso de utilizador resumo vida geradas no passo anterior |
    4. Agora, execute o 3 APIs listados na coleção: 
        1. Selecione **gerar Long-Lived Token de acesso** e clique em **enviar**.
        2. Selecione **obter ID de utilizador** e clique em **enviar**.
        3. Selecione **obter Token de acesso de permanente página** e clique em **enviar**.
    5. Copiar o **access_token** valor da resposta e atribuí-la para a definição de aplicação **fb:PageAccessToken**.

Já está!

A solução envia todas as imagens e texto publicado na sua página de Facebook para o Content Moderator. Os fluxos de trabalho que configurou anteriormente são chamados. O conteúdo que não é transmitido aos critérios definidos nos resultados de fluxos de trabalho nas revisões dentro da ferramenta de revisão. O restante do conteúdo é publicado.

## <a name="license"></a>Licença

Todos os exemplos e SDKs serviços cognitivos da Microsoft são licenciados com a licença do MIT. Para obter mais detalhes, consulte [licença](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Código de Conduta do Programador

Os desenvolvedores que usam os serviços cognitivos, incluindo esta biblioteca de cliente e o exemplo, espera-se para seguir a "desenvolvedor código de conduta para serviços cognitivos da Microsoft", encontrado em http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Passos Seguintes

1. [Veja uma demonstração (vídeo)](https://channel9.msdn.com/Events/Build/2017/T6033) desta solução da Microsoft Build 2017.
1. [O exemplo do Facebook no Github](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
