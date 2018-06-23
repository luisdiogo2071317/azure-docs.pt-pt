---
title: Exportação e eliminação de dados de cliente - LUIS - serviços cognitivos do Azure | | Microsoft Docs
description: Referência para exportar e eliminação de dados de cliente do serviço de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: f684b8ab875e2fbb774dc4a29bce25be41b24e6d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355447"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportar e eliminar os dados de cliente no idioma compreender (LUIS) nos serviços cognitivos

## <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados de cliente
Idioma compreender inteligente serviço (LUIS) preserva o conteúdo de cliente operar o serviço, mas o utilizador LUIS tem controlo total sobre a visualização, exportar e eliminar os respetivos dados. Isto pode ser feito através da web LUIS [portal](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-reference-regions) ou [APIs programático LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Conteúdo de cliente é armazenado encriptados no armazenamento do Azure regional Microsoft e inclui:

- Conteúdo de conta de utilizador recolhidos no registo
- Dados de formação necessários para criar os modelos (ou seja, intenção & entidades)
- Consultas de utilizador com sessão iniciadas no tempo de execução para ajudar a melhorar os modelos de utilizador
  - Os utilizadores podem desativar o registo de consultas, acrescentando `&log=false` para o pedido, detalhes [aqui](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-resources-faq#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>Eliminar dados de cliente
Utilizadores de LUIS ter controlo total para eliminar qualquer conteúdo de utilizador, através do portal web do LUIS ou APIs LUIS programática. A tabela seguinte apresenta as ligações a prestar assistência com ambos:

| | **Conta de utilizador** | **Aplicação** | **Utterance(s)** | **Consultas de utilizador final** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-account-settings) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) | [Link](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app#delete-app) |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportar dados de cliente
Utilizadores de LUIS ter controlo total para ver os dados no portal, no entanto, tem de ser exportado através das APIs programático LUIS. A tabela seguinte apresenta as ligações a prestar assistência com exportações de dados através de APIs programático LUIS:

| | **Conta de utilizador** | **Aplicação** | **Utterance(s)** | **Consultas de utilizador final** |
| --- | --- | --- | --- | --- |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência de regiões LUIS](./luis-reference-regions.md)
