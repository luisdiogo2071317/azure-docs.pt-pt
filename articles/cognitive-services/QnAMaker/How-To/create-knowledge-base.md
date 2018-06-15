---
title: Como criar uma base de dados de conhecimento - Maker de QnA - Azure cognitivos os serviços de | Microsoft Docs
description: Como criar uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93b64948ecc52feeb0f862f2b76ea898dce2333a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355936"
---
# <a name="create-a-knowledge-base"></a>Criar uma base de dados de conhecimento

Maker de QnA torna muito simples para carregar as origens de dados existente para criar uma base de dados de conhecimento. Pode criar uma nova base de dados de conhecimento Maker de QnA a partir de páginas de FAQ, como produtos, documentos estruturados ou adicioná-los editorially.

## <a name="steps"></a>Passos

1. Para começar, inicie sessão no [portal de QnA Maker](https://qnamaker.ai) com as suas credenciais do azure e clique em **criar novo serviço**.

    ![Criar KB ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Se já não tiver criado um serviço de QnA Maker, selecione **criar um serviço de QnA**. Caso contrário, escolha um serviço de QnA Maker listas pendentes no passo 2. Selecione o serviço de QnA Maker que irá alojar a Base de dados de conhecimento.

    ![Configurar o serviço de QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Introduza as seguintes informações para criar a base de dados de conhecimento.

    ![Conjunto de origens de dados](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Dê o seu serviço uma **nome.** São suportados nomes duplicados e carateres especiais são bem suportados.
    - Colar os URLs que serão extraídos da. Consulte informações adicionais sobre os tipos de origens suportados [aqui](../Concepts/data-sources-supported.md).
    - Em alternativa, carregar ficheiros a partir da qual os dados são extraídos. Consulte o [obter informações sobre preços](https://aka.ms/qnamaker-pricing
) pode adicionar ver o número de documentos.
    - Se pretender adicionar manualmente QnAs, pode ignorar os ficheiros de ligação.

4. Selecione **Criar**.

    ![Criar KB](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Demora alguns minutos para os dados a ser extraído.

    ![Extração](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Se a Base de dados de conhecimento foi criado com êxito, será redirecionado para o **base de dados de conhecimento** página.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Importar uma base de dados de conhecimento](../Tutorials/migrate-knowledge-base.md)
