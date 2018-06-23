---
title: Como criar uma base de dados de conhecimento não ingleses - Maker de QnA - Azure cognitivos os serviços de | Microsoft Docs
description: Como criar uma base de dados de conhecimento não ingleses.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 3fbd590229044af0daa60968fd8d556d539a58c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354050"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Suporte de idiomas do conteúdo da base de dados de conhecimento para Maker de QnA
Maker de QnA suporta conteúdo de base de dados de conhecimento em vários idiomas. No entanto, cada serviço de QnA Maker deve ser reservado para numa única linguagem. A primeira base de dados de conhecimento criada direcionada para um determinado serviço de QnA Maker define o idioma de que o serviço. Consulte [aqui](../Overview/languages-supported.md) para obter a lista de idiomas suportados.

O idioma é automaticamente reconhecido do conteúdo das origens de dados que está a ser extraído. Depois de criar um novo serviço de Maker de QnA e uma nova Base de dados de conhecimento em que o serviço, pode verificar se o idioma foi definido corretamente.

1. Navegue para o [Portal do Azure](https://portal.azure.com/).

2. Selecione **grupos de recursos** e navegue para o grupo de recursos em que o serviço de QnA Maker é implementado e selecione o **da Azure Search** recursos.

    ![Selecione o recurso de pesquisa do Azure](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Selecione o **testkb** índice. Este índice da Azure Search é sempre o primeiro criado e contém o conteúdo de todas as bases de dados de conhecimento que o serviço guardado. 

    ![Selecione o teste KB](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Selecione **campos** secção que mostra os detalhes de testkb.

    ![Selecionar campos](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Marque a caixa de **analisador** para ver detalhes do idioma.

    ![Selecione Analyzer](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. Deverá considerar de que o analisador de está definido para um idioma específico. Neste idioma automaticamente foi detetado durante o passo de criação da base de dados de conhecimento. Não pode ser alterado neste idioma assim que o recurso é criado.

    ![Analisador selecionado](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar um bot de QnA com o serviço de Bot do Azure](../Tutorials/create-qna-bot.md)
