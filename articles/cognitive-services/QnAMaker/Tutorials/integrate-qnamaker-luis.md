---
title: LUIS e QnAMaker - integração de Bot
titleSuffix: Azure Cognitive Services
description: Um tutorial passo a passo sobre como integrar o QnA Maker e o LUIS num bot.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: nstulasi
ms.openlocfilehash: cfadaa836eef0763c9dd56c71d177995ab658c3e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736041"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Integrar o QnA Maker e o LUIS para distribuir a sua base de dados de conhecimento
À medida que sua base de dados de conhecimento do QnA Maker cresce grandes, torna-se difícil mantê-lo como um único monolítico definido e há a necessidade de dividir a base de dados de conhecimento em partes lógicas menores.

Embora seja fácil para criar várias bases de dados de conhecimento no Criador de FAQ, terá de alguma lógica para rotear a pergunta de entrada para a base de dados de conhecimento adequada. Pode fazê-lo ao utilizar o LUIS.

## <a name="architecture"></a>Arquitetura

![Arquitetura de luis do QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

No cenário acima, o QnA Maker obtém primeiro a intenção da pergunta recebida de um modelo do LUIS e, em seguida, usá-lo para encaminhar para a base de dados de conhecimento do QnA Maker correta.

## <a name="prerequisites"></a>Pré-requisitos
- Inicie sessão para o [LUIS](https://www.luis.ai/) portal e [criar uma aplicação](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
- [Adicionar intenções](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) de acordo com o seu cenário.
- [Train](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) e [publicar](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp) sua aplicação LUIS.
- Inicie sessão no [QnA Maker](https://qnamaker.ai) e [criar](https://www.qnamaker.ai/Create) bases de dados de conhecimento de acordo com o seu cenário.
- Testar e publicar as bases de dados de conhecimento.

## <a name="qna-maker--luis-bot"></a>A ferramenta QnA Maker + Bot de LUIS
1. Primeiro criar um bot de aplicação Web com o modelo do LUIS, ligá-lo com a aplicação do LUIS que criou acima e modificar as intenções. Veja os passos detalhados [aqui](https://docs.microsoft.com/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Adicione as dependências na parte superior do arquivo, com as outras dependências:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Adicionar a abaixo da classe para chamar o serviço QnA Maker:

    ```
        /// <summary>
        /// QnAMakerService is a wrapper over the QnA Maker REST endpoint
        /// </summary>
        [Serializable]
        public class QnAMakerService
        {
            private string qnaServiceHostName;
            private string knowledgeBaseId;
            private string endpointKey;
    
            /// <summary>
            /// Initialize a particular endpoint with it's details
            /// </summary>
            /// <param name="hostName">Hostname of the endpoint</param>
            /// <param name="kbId">Knowledge base ID</param>
            /// <param name="ek">Endpoint Key</param>
            public QnAMakerService(string hostName, string kbId, string ek)
            {
                qnaServiceHostName = hostName;
                knowledgeBaseId = kbId;
                endpointKey = ek;
            }
    
            /// <summary>
            /// Call the QnA Maker endpoint and get a response
            /// </summary>
            /// <param name="query">User question</param>
            /// <returns></returns>
            public string GetAnswer(string query)
            {
                var client = new RestClient( qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer");
                var request = new RestRequest(Method.POST);
                request.AddHeader("authorization", "EndpointKey " + endpointKey);
                request.AddHeader("content-type", "application/json");
                request.AddParameter("application/json", "{\"question\": \"" + query + "\"}", ParameterType.RequestBody);
                IRestResponse response = client.Execute(request);
    
                // Deserialize the response JSON
                QnAAnswer answer = JsonConvert.DeserializeObject<QnAAnswer>(response.Content);
    
                // Return the answer if present
                if (answer.answers.Count > 0)
                    return answer.answers[0].answer;
                else
                    return "No good match found.";
            }
        }
    
        /* START - QnA Maker Response Class */
        public class Metadata
        {
            public string name { get; set; }
            public string value { get; set; }
        }
    
        public class Answer
        {
            public IList<string> questions { get; set; }
            public string answer { get; set; }
            public double score { get; set; }
            public int id { get; set; }
            public string source { get; set; }
            public IList<object> keywords { get; set; }
            public IList<Metadata> metadata { get; set; }
        }
    
        public class QnAAnswer
        {
            public IList<Answer> answers { get; set; }
        }
        /* END - QnA Maker Response Class */
    ```

3. Aceda a https://qnamaker.ai -> meus bases de dados de conhecimento -> código do modo de exibição, da sua base de dados de conhecimento correspondente. Obter as seguintes informações:

    ![Pedido de HTTP do QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. Criar uma instância da classe QnAMakerService para cada uma das suas bases de dados de conhecimento:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Chame a base de dados de conhecimento correspondente para o objetivo.
    ```
            // HR Intent
            [LuisIntent("HR")]
            public async Task CancelIntent(IDialogContext context, LuisResult result)
            {
                // Ask the HR knowledge base
                await context.PostAsync(hrQnAService.GetAnswer(result.Query));
            }
    
            // Payroll intent
            [LuisIntent("Payroll")]
            public async Task GreetingIntent(IDialogContext context, LuisResult result)
            {
                // Ask the payroll knowledge base
                await context.PostAsync(payrollQnAService.GetAnswer(result.Query));
            }
    
            // Finance intent
            [LuisIntent("Finance")]
            public async Task HelpIntent(IDialogContext context, LuisResult result)
            {
                // Ask the finance knowledge base
                await context.PostAsync(financeQnAService.GetAnswer(result.Query));
            }
    ```

## <a name="build-the-bot"></a>Criar o bot
1. No editor de código, faça duplo clique em `build.cmd` e selecione **executar consola**.

    ![executar a partir da consola](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. A visualização de código é substituída por uma janela de terminal que mostra o progresso e os resultados da compilação.

    ![compilação de consola](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Testar o bot
No portal do Azure, selecione **teste na Web Chat** para testar o bot. Escreva mensagens desde intenções diferentes para obter a resposta da base de dados de conhecimento correspondente.

![teste de chat na Web](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar um plano de continuidade de negócio para o QnA Maker](../How-To/business-continuity-plan.md)
