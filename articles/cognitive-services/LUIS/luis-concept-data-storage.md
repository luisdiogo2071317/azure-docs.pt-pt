---
title: Compreender o armazenamento de dados no LUIS - Azure | Microsoft Docs
description: Saiba como os dados são armazenados na compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
ms.openlocfilehash: f235c787e7d2064696e5421219a297d914b5882d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266010"
---
# <a name="data-storage-and-removal"></a>Armazenamento de dados e de remoção
LUIS armazena os dados encriptados num arquivo de dados do Azure correspondente para a região especificada pela chave. Estes dados são armazenados durante 30 dias. 

## <a name="export-and-delete-app"></a>Exportar e eliminar a aplicação
Os utilizadores têm controlo total sobre [exportar](create-new-app.md#export-app) e [eliminar](create-new-app.md#delete-app) a aplicação. 

## <a name="utterances-in-an-intent"></a>Utterances no objetivo
Eliminar utterances de exemplo utilizados para formação [LUIS][LUIS]. Se eliminar um utterance de exemplo da sua aplicação LUIS, é removido do serviço web LUIS e não está disponível para exportação.

## <a name="utterances-in-review"></a>Utterances em revisão
Pode eliminar utterances da lista de utterances de utilizador que sugere LUIS no  **[página de utterances de ponto final de revisão](label-suggested-utterances.md)**. Eliminar utterances desta lista evitará que está a ser sugeridos, mas não a eliminá-los de registos.

## <a name="accounts"></a>Contas
Se eliminar uma conta, todas as aplicações são eliminadas, juntamente com os seus registos e utterances de exemplo. Os dados são mantidos durante 60 dias antes da conta e os dados são eliminados permanentemente.

A eliminação da conta está disponível a partir de **definições** página. Selecione o nome da sua conta na barra de navegação superior de direita para obter o **definições** página.

## <a name="data-inactivity-as-an-expired-subscription"></a>Inatividade de dados como uma subscrição expirada
Para efeitos de retenção de dados e eliminação, uma aplicação LUIS inativa pode em _discrição da Microsoft_ ser tratado como uma subscrição expirada. Uma aplicação é considerada inativa se cumpre os seguintes critérios para os últimos 90 dias: 

* Tenha tido **não** chamadas efetuadas ao mesmo.
* Não foi modificado.
* Não ter uma chave atual atribuídas ao mesmo.
* Não tenha tido um utilizador iniciar sessão, ao mesmo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre como exportar e eliminar uma aplicação](create-new-app.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions