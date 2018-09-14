---
title: Criar uma base de dados de conhecimento - QnA Maker
titleSuffix: Azure Cognitive Services
description: Adicionar chit-bate-papo ao seu bot torna mais conversacionais e apelativas. A ferramenta QnA Maker permite-lhe adicionar facilmente um conjunto preenchido previamente do chat-chit superior, em sua BDC. Isso pode ser um ponto de partida para chit-bate-papo o bot e economizar o tempo e o custo de escrevê-los a partir do zero.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 66705a0afdcdb04fe49bea0bfc03286df3611071
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543363"
---
# <a name="create-a-knowledge-base"></a>Criar uma base de dados de conhecimento

A ferramenta QnA Maker torna muito simples para carregar suas origens de dados existente para criar uma base de dados de conhecimento. Pode criar uma nova base de dados de conhecimento do QnA Maker de páginas de perguntas frequentes, manuais de produtos, documentos estruturados ou adicioná-los de forma editorial.

## <a name="steps"></a>Passos

1. Para começar, inicie sessão para o [portal do QnA Maker](https://qnamaker.ai) com as suas credenciais do azure e clique em **criar novo serviço**.

    ![Criar KB ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Se ainda não tenha criado um serviço QnA Maker, selecione **criar um serviço QnA**. Caso contrário, escolha um serviço QnA Maker de listas pendentes no passo 2. Selecione o serviço QnA Maker que irá alojar a Base de dados de conhecimento.

    ![Configurar o serviço QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Introduza as seguintes informações para criar a base de dados de conhecimento.

    ![Origens de dados do conjunto](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Dê o seu serviço um **nome.** São suportados nomes duplicados e carateres especiais são também suportados.
    - Cole os URLs que serão extraídos da. Ver mais informações sobre os tipos de origens suportadas [aqui](../Concepts/data-sources-supported.md).
    - Em alternativa, carregue ficheiros a partir do qual os dados são extraídos. Consulte a [obter informações sobre preços](https://aka.ms/qnamaker-pricing
) pode adicionar ver o número de documentos.
    - Se pretender adicionar manualmente QnAs, pode ignorar a vinculação de arquivos.

4. Selecione **Criar**.

    ![Criar KB](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Demora alguns minutos para os dados a ser extraído.

    ![Extração](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Se a sua Base de dados de conhecimento foi criado com êxito, será redirecionado para o **base de dados de conhecimento** página.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Importar uma base de dados de conhecimento](../Tutorials/migrate-knowledge-base.md)
