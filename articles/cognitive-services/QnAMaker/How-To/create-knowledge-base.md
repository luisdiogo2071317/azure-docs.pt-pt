---
title: Criar uma base de dados de conhecimento - QnA Maker
titleSuffix: Azure Cognitive Services
description: Adicionar chit-bate-papo ao seu bot torna mais conversacionais e apelativas. A ferramenta QnA Maker permite-lhe adicionar facilmente um conjunto preenchido previamente do chat-chit superior, em sua BDC. Isso pode ser um ponto de partida para chit-bate-papo o bot e economizar o tempo e o custo de escrevê-los a partir do zero.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 274c2289c75f44c5a1c8dd3799612a23f46a6d67
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037699"
---
# <a name="create-a-knowledge-base"></a>Criar uma base de dados de conhecimento

A ferramenta QnA Maker torna simples para adicionar as suas origens de dados existente durante a criação de uma base de dados de conhecimento. Pode criar uma nova base de dados de conhecimento do QnA Maker partir os seguintes tipos de documento:

<!-- added for scanability -->
* Páginas de perguntas frequentes
* Manuais de produtos
* Documentos estruturados

## <a name="steps"></a>Passos

1. Iniciar sessão para o [portal do QnA Maker](https://qnamaker.ai) com as suas credenciais do Azure e selecione **criar novo serviço**.

    ![Criar KB ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Se ainda não tenha criado um serviço QnA Maker, selecione **criar um serviço QnA**. 

3. Selecione o seu inquilino do Azure, o nome da subscrição do Azure e o nome de recurso do Azure associados com o serviço QnA Maker listas no **passo 2** no portal do QnA Maker. Selecione o serviço QnA Maker do Azure que irá alojar a Base de dados de conhecimento.

    ![Configurar o serviço QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

4. Introduza o nome da sua base de dados de conhecimento e as origens de dados para a nova base de dados de conhecimento.

    ![Origens de dados do conjunto](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Dê o seu serviço um **nome.** São suportados nomes duplicados e carateres especiais.
    - Adicione URLs de dados que pretende extraídos. Ver mais informações sobre os tipos de origens suportadas [aqui](../Concepts/data-sources-supported.md).
    - Carregar ficheiros para dados que pretende extraídos. Consulte a [obter informações sobre preços](https://aka.ms/qnamaker-pricing) pode adicionar ver o número de documentos.
    - Se pretender adicionar manualmente QnAs, pode avançar **passo 4** mostrado na imagem anterior.

5. Adicione **Chit-bate-papo** para sua KB. Opte por adicionar o suporte para o bot, chit chat ao escolher entre uma as 3 personalidades predefinidas. 

    <!-- TBD: add back in when chit chat how-to is merged
    ![Add chit-chat to KB ](../media/qnamaker-how-to-chitchat/create-kb-chit-chat.png)
    -->

6. Selecione **criar a sua BDC**.

    ![Criar KB](../media/qnamaker-how-to-create-kb/create-kb.png)

7. Demora alguns minutos para os dados a ser extraído.

    ![Extração](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

8. Quando a sua Base de dados de conhecimento é criado com êxito, será redirecionado para o **base de dados de conhecimento** página.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Adicionar o pessoal de chit-bate-papo](./chit-chat-knowledge-base.md)
