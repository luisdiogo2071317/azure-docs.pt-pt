---
title: Como testar um serviços cognitivos do Azure de base de dados de conhecimento - Maker de QnA - | Microsoft Docs
description: Teste a sua base de dados de conhecimento antes de a publicar.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: cffb63666edab25e1b3b0739d0e0f2f828600f3a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354097"
---
# <a name="test-your-knowledge-base"></a>Testar a sua base de dados de conhecimento

Testar a sua base de dados de conhecimento de QnA Maker é uma parte importante de um processo iterativo para melhorar a precisão das respostas a ser devolvido. Pode testar a base de dados de conhecimento através de uma interface de chat avançada que também permite que efetuar edições.

## <a name="test-answer-matching"></a>Correspondência de resposta do teste

1.  Aceder à sua base de dados de conhecimento, selecionando o respetivo nome na **meu bases de dados de conhecimento** página.
2.  Para aceder ao painel de gradualmente Escalamento de teste, selecione **teste** no painel superior da sua aplicação.

    ![Teste de acesso](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Introduza uma consulta na caixa de texto e selecione Enter.

4.  A resposta best-correspondente a partir da base de dados de conhecimento é devolvida como resposta.

## <a name="clear-test-panel"></a>Painel de teste simples

Para limpar todas as consultas de teste introduzido e os respetivos resultados a partir da consola de teste, selecione **recomeçar** no canto superior esquerdo do painel de teste.

## <a name="close-test-panel"></a>Painel de teste fechar

Para fechar o painel de teste, selecione o **teste** botão novamente. Enquanto o painel de teste estiver aberto, não é possível editar o conteúdo da Base de dados de conhecimento.

## <a name="inspect-score"></a>Inspecione o modelo de pontuação

Inspecione os detalhes do resultado do teste no painel de inspecionar.

1.  Com o painel de escalamento gradualmente teste aberto, selecione **inspecionar** para obter mais detalhes sobre essa resposta.

    ![Inspecione respostas](../media/qnamaker-how-to-test-kb/inspect.png)

2.  É apresentado o painel de inspeção. O painel inclui na parte superior da classificação de intenção, bem como quaisquer entidades identificadas. O painel mostra o resultado do utterance selecionado.

## <a name="correct-the-top-scoring-answer"></a>Corrija a parte superior da classificação de resposta

Se a parte superior da classificação de resposta está incorreta, selecione a resposta correta da lista e selecione **guardar e formação**.

![Teste de acesso](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Adicionar perguntas alternativas

Pode adicionar formulários alternativos de uma pergunta para uma determinada resposta. Tipo de atende o alternativo na caixa de texto e clique em introduza adicioná-los. Selecione **guardar e formação** para armazenar as atualizações.

![Teste de acesso](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Adicionar uma nova resposta

Pode adicionar uma nova resposta se qualquer um das respostas existentes que foram correspondidas estão incorretas ou a resposta não existe na base de dados de conhecimento (não encontrada nenhuma correspondência boa no KB). Introduza a nova resposta à pergunta atual na caixa de texto e prima enter para adicioná-lo. 

Selecione **guardar e formação** para manter esta resposta. Um novo par de resposta de pergunta foi adicionado à sua base de dados de conhecimento.

![Teste de acesso](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Todas as edições para a base de dados de conhecimento são apenas guardadas quando prime o **guardar e formação** botão.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Publicar uma base de dados de conhecimento](./publish-knowledge-base.md)
