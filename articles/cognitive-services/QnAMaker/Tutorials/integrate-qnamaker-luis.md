---
title: Integrar Maker de QnA e LUIS - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: um tutorial passo a passo sobre como integrar Maker de QnA e LUIS
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 0a0eeb3815b793ed81f60b2b239bc459e5574788
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354110"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Integrar Maker de QnA e LUIS para distribuir a sua base de dados de conhecimento
À medida que a base de dados de conhecimento de QnA Maker cresce grande, torna-se difícil mantê-lo como um único monolithic definido e não existe uma necessidade de dividir a base de dados de conhecimento em segmentos de lógicos mais pequeno.

Enquanto é simples criar várias bases de dados de conhecimento no Maker de QnA, terá algumas lógica para encaminhar a pergunta de entrada para a base de dados de conhecimento adequada. Pode fazê-lo utilizando LUIS.

## <a name="architecture"></a>Arquitetura

![Arquitetura de luis Maker de QnA](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

No cenário acima, Maker de QnA primeiro obtém a intenção da pergunta recebida de um modelo de LUIS e, em seguida, utilize-a para encaminhar para a base de dados de conhecimento Maker de QnA correta.

## <a name="prerequisites"></a>Pré-requisitos
- Inicie sessão no [LUIS](https://www.luis.ai/) portal e [criar uma aplicação](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app).
- [Adicionar pendentes](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) de acordo com o seu cenário.
- [Formação](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-train) e [publicar](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/publishapp) aplicação LUIS.
- Inicie sessão no [Maker de QnA](https://qnamaker.ai) e [criar dados de conhecimento]() bases de acordo com o seu cenário.
- [Teste]() e [publicar]() constituir a base de dados de conhecimento.

## <a name="qna-maker--luis-bot"></a>Maker de QnA + LUIS Bot
1. Primeiro criar um bot de aplicação Web com o modelo de LUIS, ligá-lo com a aplicação de LUIS que criou acima e modificar os pendentes. Consulte os passos detalhados [aqui](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Adicione as dependências na parte superior do ficheiro, com as outras dependências:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Adicionar o abaixo classe para chamar o serviço de QnA Maker:

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

3. Aceda a https://qnamaker.ai -> meu bases de dados de conhecimento -> código do Vista da sua base de dados de conhecimento correspondente. Obter as seguintes informações:

    ![Pedido de QnA Maker HTTP](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. Instanciar a classe de QnAMakerService para cada uma das suas bases de dados de conhecimento:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Chamada de base de dados de conhecimento correspondente para o objetivo.
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
1. No editor de código, clique com botão direito no `build.cmd` e selecione **executar a partir da consola**.

    ![executar a partir da consola](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. A vista de código é substituída por uma janela de terminal que mostra o progresso e os resultados de compilação.

    ![compilação de consola](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Testar o bot
No portal do Azure, selecione **testar na Web Chat** para testar o bot. Tipo de mensagens de diferentes pendentes para obter a resposta a partir da base de dados de conhecimento correspondente.

![teste de chat de Web](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar um plano de continuidade do negócio para Maker de QnA](../How-To/business-continuity-plan.md)
