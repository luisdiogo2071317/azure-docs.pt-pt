---
title: Ciclo de vida de desenvolvimento de uma base de dados de conhecimento - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Ciclo de vida de desenvolvimento de uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 9ecdd2c7823eed145621b214690eff7681065507
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356032"
---
# <a name="knowledge-base-lifecycle"></a>Ciclo de vida de base de dados de conhecimento
Maker de QnA Aprende melhor num ciclo iterativo de alterações de modelo, exemplos de utterance, publicação e a recolha de dados de consultas de ponto final. 

![Ciclo de criação](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Criar uma base de dados de conhecimento de QnA Maker
Ponto final de base de dados de conhecimento (KB) de QnA Maker fornece uma melhor correspondência resposta a uma consulta de utilizador com base no conteúdo do KB. Criar uma base de dados de conhecimento é uma ação única para configurar um repositório de conteúdo de perguntas, respostas e metadados associados. É possível criar uma base de dados de conhecimento por indexadores conteúdo já existente, como páginas FAQ, como de produto ou estruturados pares de Q-A. Saiba como [criar uma base de dados de conhecimento](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Testar e atualizar a base de dados de conhecimento
A base de dados de conhecimento está pronta para testar uma vez que este é preenchido com conteúdo, editorially ou através de extração automática. Pode ser feita testar o **teste** painel por introduzir consultas comuns do utilizador e verificar que as respostas devolvidas estão como esperado e com o modelo de pontuação de confiança suficientes. Pode adicionar perguntas alternativas para resolver pontuações de confiança baixa. Também pode adicionar novas respostas quando uma consulta não devolve "não foi encontrada no artigo KB a nenhuma correspondência" predefinido resposta. Este ciclo sólido da atualização de teste continua até estar satisfeito com os resultados. Saiba como [testar a sua base de dados de conhecimento](../How-To/test-knowledge-base.md).

Para grandes KBs, pode ser automatizada de teste através de generateAnswer APIs. 

## <a name="publish-the-knowledge-base"></a>Publicar a base de dados de conhecimento
Quando tiver terminado a base de dados de conhecimento de teste, pode publicá-lo. Publicar a versão mais recente da base de dados de conhecimento testada para uma pesquisa do Azure dedicada que representa o índice de pushes o **publicados** base de dados de conhecimento. Também cria um ponto final que pode ser chamado na sua aplicação ou chat bot.

Desta forma, quaisquer alterações efetuadas para a versão de teste de base de dados de conhecimento não afetam a versão publicada que poderão ser em direto de uma aplicação de produção.

Cada uma destas bases de dados de conhecimento pode ser visada para fins de teste em separado. Utilizar as APIs, pode visar a versão de teste de base de dados de conhecimento com `isTest=true` sinalizador na chamada de generateAnswer.

Saiba como [publicar a sua base de dados de conhecimento](../How-To/publish-knowledge-base.md).

## <a name="monitor-usage"></a>Monitorizar a utilização
Para poder-se de que os registos de chat do seu serviço de registo, terá de ativar o Application Insights quando que [criar o serviço de QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

Pode obter vários análise da utilização do serviço. Saiba mais sobre como utilizar o application insights para obter [analytics para o seu serviço de QnA Maker](../How-To/get-analytics-knowledge-base.md).

Com base no que aprender da sua análise, se apropriado [atualizações para a base de dados de conhecimento](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Classificação de confiança](./confidence-score.md)

## <a name="see-also"></a>Consulte também 

[Base de dados de conhecimento](./knowledge-base.md)
[descrição geral de QnA Maker](../Overview/overview.md)
