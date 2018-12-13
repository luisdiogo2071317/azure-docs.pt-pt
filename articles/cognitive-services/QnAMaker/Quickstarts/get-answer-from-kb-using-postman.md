---
title: 'Início rápido: Utilizar o Postman para receber resposta da base de dados de conhecimento - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Este início rápido explica como obter uma resposta de sua base de dados de conhecimento com o Postman.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: diberry
ms.openlocfilehash: 476e982bdddd41c1daf06c3a673d964ce2a85f98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270897"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Início rápido: Receber uma resposta da base de dados de conhecimento com o Postman

Este guia de introdução com base no Postman explica como obter uma resposta de sua base de dados de conhecimento.

## <a name="prerequisites"></a>Pré-requisitos

* Mais recente [ **Postman**](https://www.getpostman.com/).
* Tem de ter uma [serviço QnA Maker](../How-To/set-up-qnamaker-service-azure.md) e têm um [base de dados de conhecimento com perguntas e respostas](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Publicar para obter o ponto final

Quando estiver pronto para gerar uma resposta a uma pergunta da sua base de dados de conhecimento [publicar](../How-to/publish-knowledge-base.md) sua base de dados de conhecimento.

## <a name="use-production-endpoint-with-postman"></a>Utilizar o ponto final de produção com o Postman

Quando é publicado a sua base de dados de conhecimento, o **publicar** página apresenta as definições do pedido HTTP para gerar uma resposta. A vista predefinida mostra as definições necessárias para gerar uma resposta da [Postman](https://www.getpostman.com).

[![Publicar os resultados](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Para gerar uma resposta com o Postman, conclua os seguintes passos:

1. Abra o Postman. 
1. Selecione o bloco de construção para criar um pedido básico.
1. Definir o **nome do pedido** como `Generate QnA Maker answer`e o **coleção** como `Generate QnA Maker answers`.
1. Na área de trabalho, selecione o método HTTP **POST**.
1. Para o URL, concatenar o valor de HOST e o valor de Post para criar o URL completo. 

    [![No Postman, definir o método Post e o URL completo](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Selecione o **cabeçalhos** separador no URL, em seguida, selecione **editar em massa**. 
1. Copie os cabeçalhos para a área de texto.

    [![No Postman, definir os cabeçalhos](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Selecione o **corpo** separador.
1. Selecione o **brutos** formatar e introduza o JSON que representa a pergunta.

    [![No Postman, definir o valor JSON do corpo](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Selecione o **enviar** botão.
1. A resposta contém a resposta, juntamente com outras informações que podem ser importantes para a aplicação cliente. 

    [![No Postman, definir o valor JSON do corpo](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint-with-curl"></a>Utilizar o ponto final de teste com cURL

Se quiser obter uma resposta do ponto de extremidade de teste, utilize o parâmetro booleano da cadeia de consulta `isTest` com o valor do `true`.

`isTest=true`

## <a name="next-steps"></a>Passos Seguintes

Página de publicação também fornece informações que lhe [gerar uma resposta](get-answer-from-kb-using-curl.md) com cURL. 

> [!div class="nextstepaction"]
> [Metadados de utilização ao gerar uma resposta](../How-to/metadata-generateanswer-usage.md)