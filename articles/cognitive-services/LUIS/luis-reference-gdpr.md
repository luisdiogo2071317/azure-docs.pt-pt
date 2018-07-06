---
title: A exportação e eliminação de dados de cliente - LUIS - serviços cognitivos do Azure | | Documentos da Microsoft
description: Referência para a exportação e eliminação de dados do cliente do serviço de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/23/2018
ms.author: v-geberr;
ms.openlocfilehash: 8b4f9c17557b405bc5f4e78947952826724fc784
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865067"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportar e eliminar os dados dos clientes na compreensão de idiomas (LUIS) nos serviços cognitivos

## <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados do cliente
Language Understanding Intelligent Service (LUIS) preserva o conteúdo de cliente para operar o serviço, mas o utilizador de LUIS tem controlo total sobre a exibição, exportar e eliminar os seus dados. Isso pode ser feito através da web de LUIS [portal](luis-reference-regions.md) ou o [APIs programáticas do LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Conteúdo de cliente é armazenado encriptada no armazenamento do Microsoft regional do Azure e inclui:

- Conteúdo de conta de utilizador recolhidas no registo
- Dados de treinamento necessários para criar modelos (ou seja, intenções e entidades)
- Consultas de utilizador com sessão iniciadas no tempo de execução para ajudar a melhorar os modelos de utilizador
  - Os utilizadores podem desativar o registo de consultas, acrescentando `&log=false` para o pedido, detalhes [aqui](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances)

## <a name="deleting-customer-data"></a>A eliminação de dados do cliente
Os utilizadores de LUIS tem controlo total para eliminar qualquer conteúdo de utilizador, através do portal web do LUIS ou as APIs de programação do LUIS. A tabela seguinte apresenta links ajudá-lo com ambos:

| | **Conta de utilizador** | **Aplicação** | **Utterance(s)** | **Consultas de utilizador final** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-how-to-account-settings.md) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) | [Link](create-new-app.md#delete-app) |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportar dados do cliente
Os utilizadores de LUIS tem controlo total para ver os dados no portal, no entanto, tem de ser exportada através das APIs de programação do LUIS. A tabela seguinte apresenta links ajudá-lo com exportações de dados por meio das APIs de programação do LUIS:

| | **Conta de utilizador** | **Aplicação** | **Utterance(s)** | **Consultas de utilizador final** |
| --- | --- | --- | --- | --- |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência de regiões de LUIS](./luis-reference-regions.md)
