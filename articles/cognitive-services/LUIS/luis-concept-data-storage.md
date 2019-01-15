---
title: Armazenamento de dados
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS armazena dados criptografados num arquivo de dados do Azure correspondente para a região especificada pela chave.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: db6a3d09dbcffcd72e5508f8385e2347ddb86f51
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264704"
---
# <a name="data-storage-and-removal-in-language-understanding-luis-cognitive-services"></a>Armazenamento de dados e remoção nos serviços cognitivos de compreensão de idiomas (LUIS)
LUIS armazena dados criptografados num arquivo de dados do Azure correspondente para a região especificada pela chave. Estes dados são armazenados durante 30 dias. 

## <a name="export-and-delete-app"></a>Exportar e eliminar a aplicação
Os utilizadores têm controlo total sobre [exportação](luis-how-to-start-new-app.md#export-app) e [eliminar](luis-how-to-start-new-app.md#delete-app) a aplicação. 

## <a name="utterances-in-an-intent"></a>Expressões com numa intenção
Eliminar usadas para treinamento de expressões de exemplo [LUIS](luis-reference-regions.md). Se eliminar uma expressão de exemplo da sua aplicação LUIS, ele é removido do serviço web LUIS e não está disponível para exportação.

## <a name="utterances-in-review"></a>Expressões com revisão
Pode eliminar expressões com na lista de expressões de utilizador que LUIS sugere no  **[página de expressões de ponto final de revisão](luis-how-to-review-endoint-utt.md)**. A eliminar expressões com desta lista evita que sejam sendo sugerida, mas não a eliminá-los a partir de registos.

## <a name="accounts"></a>Contas
Se eliminar uma conta, todas as aplicações são eliminadas, juntamente com os seus registos e as expressões de exemplo. Os dados são retidos durante 60 dias antes da conta e os dados são eliminados permanentemente.

A eliminar a conta está disponível a partir da **definições** página. Selecione o nome da sua conta na barra de navegação direita superior para ir para o **definições** página.

## <a name="data-inactivity-as-an-expired-subscription"></a>Inatividade de dados como uma assinatura expirada
Para efeitos de retenção de dados e eliminação, uma aplicação LUIS inativa pode _discrição da Microsoft_ seja tratado como uma assinatura expirada. Uma aplicação é considerada inativa se cumpre os seguintes critérios para os últimos 90 dias: 

* Teve **nenhuma** as chamadas feitas a ela.
* Não foi modificado.
* Não tem uma chave atual atribuído a ele.
* Não tenha tido um utilizador iniciar sessão na mesma.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre a exportar e eliminar uma aplicação](luis-how-to-start-new-app.md)