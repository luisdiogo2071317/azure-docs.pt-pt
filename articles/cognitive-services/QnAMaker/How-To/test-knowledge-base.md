---
title: Como testar uma base de dados de conhecimento - QnA Maker
titlesuffix: Azure Cognitive Services
description: Teste a sua base de dados de conhecimento do QnA Maker é uma parte importante de um processo iterativo para melhorar a precisão das respostas a ser devolvido. Pode testar a base de dados de conhecimento através de uma interface de bate-papo avançada também permite que faça edições.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 6ce3fc774d23914ab150e1dc1371391fc9fc548b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034390"
---
# <a name="test-your-knowledge-base"></a>Testar a sua base de dados de conhecimento

Teste a sua base de dados de conhecimento do QnA Maker é uma parte importante de um processo iterativo para melhorar a precisão das respostas a ser devolvido. Pode testar a base de dados de conhecimento através de uma interface de bate-papo avançada também permite que faça edições.

## <a name="test-answer-matching"></a>Correspondência de resposta de teste

1.  Aceder à sua base de dados de conhecimento, selecionando o respetivo nome na **meu bases de dados de conhecimento** página.
2.  Para acessar o painel de slide-out de teste, selecione **teste** no painel superior do seu aplicativo.

    ![Teste de acesso](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Introduza uma consulta na caixa de texto e selecione Enter.

4.  A resposta mais correspondente a partir da base de dados de conhecimento é retornada como a resposta.

## <a name="clear-test-panel"></a>Painel de teste clara

Para limpar todas as consultas de teste inseridos e seus resultados a partir da consola de teste, selecione **recomeçar** no canto superior esquerdo do painel de teste.

## <a name="close-test-panel"></a>Painel de teste fechar

Para fechar o painel de teste, selecione o **teste** novamente no botão. Enquanto o painel de teste estiver aberto, não é possível editar o conteúdo de Base de dados de conhecimento.

## <a name="inspect-score"></a>Inspecionar a pontuação

Inspecione os detalhes do resultado do teste no painel de Inspect.

1.  Com o painel de deslizantes Test aberto, selecione **Inspect** para obter mais detalhes sobre que a resposta.

    ![Inspecionar as respostas](../media/qnamaker-how-to-test-kb/inspect.png)

2.  É apresentado o painel de inspeção. O painel inclui a parte superior de classificação intenção, bem como qualquer entidades identificadas. O painel mostra o resultado da expressão selecionada.

## <a name="correct-the-top-scoring-answer"></a>Corrija o superior de resposta de classificação

Se a parte superior de classificação resposta estiver incorreta, selecione a resposta correta na lista e selecione **guardar e treinar**.

![Teste de acesso](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Adicionar perguntas alternativas

Pode adicionar formas alternativas de uma pergunta para uma determinada resposta. Tipo a alternativa respostas na caixa de texto e clique em Inserir para adicioná-los. Selecione **guardar e treinar** para armazenar as atualizações.

![Teste de acesso](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Adicionar uma nova resposta

Pode adicionar uma nova resposta, se qualquer uma das respostas existentes que foram correspondidas estão incorretas ou a resposta não existe na base de dados de conhecimento (encontrada nenhuma correspondência boa no KB). Introduza a nova resposta à pergunta atual na caixa de texto e prima enter para adicioná-lo. 

Selecione **guardar e treinar** para manter esta resposta. Agora foi adicionado um novo par de pergunta-resposta à sua base de dados de conhecimento.

![Teste de acesso](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Todas as edições em sua base de dados de conhecimento apenas serão salvos de quando pressiona o **guardar e treinar** botão.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Publicar uma base de dados de conhecimento](./publish-knowledge-base.md)
