---
title: Como importar uma base de dados de conhecimento - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como importar uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce8f98f9bdb37d5f326e942fe5b5e815e5272c56
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355943"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar uma base de dados de conhecimento utilizando exportar-importar
Maker de QnA anunciou a disponibilidade geral em 7 de Maio de 2018 no \\\build\ conferência. GA Maker de QnA tem uma nova arquitetura incorporada no Azure. Bases de dados de conhecimento criados Maker de QnA livre pré-visualização terão de ser migrados para depois da disponibilidade geral Maker de QnA Pré-visualização Maker de QnA vão ser preterida em Novembro de 2018. Para obter mais informações sobre as alterações no GA Maker de QnA, consulte o anúncio de QnA Maker GA [blogue](https://aka.ms/qnamakerga-blog).

Maker de QnA tem agora um [modelo de preços](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Pré-requisitos
> [!div class="checklist"]
> * Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
> * O programa de configuração uma nova [serviço Maker de QnA](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Migrar uma base de dados de conhecimento a partir do portal de pré-visualização de Maker de QnA
1. Navegue para [portal de pré-visualização de Maker de QnA](https://aka.ms/qnamaker-old-portal
) e clique em **os meus serviços**.
2. Selecione a base de dados de conhecimento que pretende migrar clicando no ícone de edição.

    ![Editar a base de dados de conhecimento](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Clique em **transferir a base de dados de conhecimento** transferir um ficheiro de .tsv que contém o conteúdo da sua base de dados de conhecimento - questões, respostas, metadados, e a origem de dados nomes de qual foram extraídos.

    ![Transferir a base de dados de conhecimento](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Inicie sessão para o [portal de QnA Maker](https://qnamaker.ai) com as suas credenciais do azure e clique em **criar novo serviço**.

    ![Criar KB ](../media/qnamaker-how-to-create-kb/create-new-service.png)
    
5. Se já não tiver criado um serviço de QnA Maker, selecione **criar um serviço de QnA**. Caso contrário, escolha um serviço de QnA Maker listas pendentes no passo 2. Selecione o serviço de QnA Maker que irá alojar a Base de dados de conhecimento.

    ![Configurar o serviço de QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Criar uma base de dados de conhecimento vazia 

    ![Conjunto de origens de dados](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Dê o seu serviço uma **nome.** São suportados nomes duplicados e carateres especiais são bem suportados.
    - Ignorar carregamento de ficheiros ou URLs que pretende utilizar os dados da sua base de dados de conhecimento de pré-visualização. Por agora, irá criar uma base de dados de conhecimento vazia.

7. Selecione **Criar**.

    ![Criar KB](../media/qnamaker-how-to-create-kb/create-kb.png)

8. Nesta nova base de dados de conhecimento, abra o **definições** separador e clique em **base de dados de conhecimento de importação**. Esta ação importa as perguntas, respostas e metadados e mantém os nomes de origem de dados da qual foram extraídos.

   ![Base de dados de conhecimento de importação](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Teste** a nova base de dados de conhecimento através do painel de teste. Saiba como [testar a sua base de dados de conhecimento](../How-To/test-knowledge-base.md).
10. **Publicar** a base de dados de conhecimento. Saiba como [publicar a sua base de dados de conhecimento](../How-To/publish-knowledge-base.md).
11. Utilize o ponto final abaixo no código da aplicação ou bot. Consulte aqui como [criar um bot de QnA](../Tutorials/create-qna-bot.md).

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Neste momento, todo o base de dados de conhecimento conteúdo - perguntas, respostas e metadados, juntamente com os nomes dos ficheiros de origem e os URLs são importados para a nova base de dados de conhecimento. 

## <a name="chatlogs-and-alterations"></a>Chatlogs e expansões
Expansões (sinónimos) não são importados automaticamente. Utilize o [V2 APIs](https://aka.ms/qnamaker-v2-apis) para exportar as expansões de pilha de pré-visualização e o [V4 APIs](https://aka.ms/qnamaker-v4-apis) para substituir na pilha de novo.

Não é possível migrar chatlogs, uma vez que a nova pilha utiliza o Application Insights para armazenar chatlogs. No entanto, pode transferir chatlogs do [portal de pré-visualização](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Editar uma base de dados de conhecimento](../How-To/edit-knowledge-base.md)
