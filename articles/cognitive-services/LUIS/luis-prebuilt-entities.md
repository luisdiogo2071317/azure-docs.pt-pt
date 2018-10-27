---
title: Entidades previamente concebidas para a compreensão de idiomas (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS inclui um conjunto de entidades previamente concebidas para reconhecimento de tipos comuns de informações, como as datas, horas, números, medidas e moeda. Suporte de entidade pré-criados varia consoante a cultura da sua aplicação LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 0fe9dbed302fd2d61305167a3bda25b1b403b761
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139979"
---
# <a name="prebuilt-entities-to-recognize-common-data-types"></a>Entidades previamente concebidas para reconhecer os tipos de dados comuns

LUIS inclui um conjunto de entidades previamente concebidas para reconhecimento de tipos comuns de informações, como datas, horas, números, medidas e moeda. 

## <a name="add-a-prebuilt-entity"></a>Adicionar uma entidade pré-criados

1. Abra a sua aplicação ao clicar em seu nome na **as minhas aplicações** página e, em seguida, clique em **entidades** no lado esquerdo. 

1. Sobre o **entidades** página, clique em **adicionar entidade pré-criados**.

1. Na **adicionar entidades pré-concebidas** diálogo caixa, selecione o datetimeV2 entidade pré-criados. 

    ![Adicionar a caixa de diálogo de entidade predefinidos](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Selecione **Done** (Concluído).

## <a name="publish-the-app"></a>Publicar a aplicação

A maneira mais fácil para ver o valor de uma entidade pré-criados é a consulta do ponto de extremidade publicado. 

1. Na barra de ferramentas superior, selecione **publicar**. Publicar no **produção**. 

1. Quando a notificação de êxito verde é apresentada, selecione o **consulte a lista de pontos de extremidade** ligação para ver os pontos de extremidade.

1. Selecione um ponto de extremidade. É aberto um novo separador do browser para esse ponto final. Mantenha o separador do browser aberta e avance para o **teste** secção.

## <a name="test"></a>Teste
Depois da entidade é adicionada, não é necessário preparar a aplicação. 

Teste a intenção de novo no ponto final adicionado por um valor para o **p** parâmetro. Utilize a seguinte tabela para expressões sugeridas para **p**:

|Expressão de teste|Valor de entidade|
|--|:--|
|Programar um vôo de amanhã|2018-10-19|
|Cancelar a marcação de 3 de Março|LUIS devolveu o mais recente 3 de Março no passado (2018-03-03) e 3 de Março no futuro (2019-03-03) porque a expressão não especificou um ano.|
|Agendar uma reunião em 10 am|10:00:00|


## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Referência de entidade predefinidos](./luis-reference-prebuilt-entities.md)
