---
title: 'Início rápido: Criar um KB - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Pode criar uma base de dados de conhecimento do QnA Maker (KB) a partir de seu próprio conteúdo, como a FAQs ou manuais de produto. O QnA Maker KB neste exemplo é criada a partir de uma página de FAQ Web se simples para responder perguntas sobre recuperação de chaves do BitLocker.
author: nitinme
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 2b8573da6cc87af39a7681fa369940fa7a0b8eb3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345238"
---
# <a name="create-train-and-publish-your-knowledge-base"></a>Criar, formar e publicar a sua base de dados de conhecimento

Pode criar uma base de dados de conhecimento do QnA Maker (KB) a partir de seu próprio conteúdo, como a FAQs ou manuais de produto. O QnA Maker KB neste exemplo é criada a partir de uma página de FAQ Web se simples para responder perguntas sobre recuperação de chaves do BitLocker.

## <a name="prerequisite"></a>Pré-requisito

> [!div class="checklist"]
> * Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-qna-maker-knowledge-base"></a>Criar uma base de dados de conhecimento do QnA Maker

1. Inicie sessão no QnAMaker.ai com as credenciais do Azure.

2. No site do QnA Maker, selecione **criar uma base de dados de conhecimento**.

   ![Criar novo KB](../media/qna-maker-create-kb.png)

3. Sobre o **Create** página, no passo 1, selecione **criar um serviço QnA**. Será direcionado para o [portal do Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) para configurar um serviço QnA Maker na sua subscrição. Se o portal do Azure exceder o tempo limite, selecione **volte a tentar** no site. Depois de ligar, é apresentado o dashboard do Azure.

4. Depois de criar com êxito um novo serviço QnA Maker no Azure, regresse ao qnamaker.ai/create. Selecione o seu serviço de QnA listas suspensas no passo 2. Se tiver criado um novo serviço do QnA, certifique-se de que Atualize a página.

   ![Selecione um serviço de QnA KB](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. Passo 3, dê um nome sua BDC **KB de QnA do meu exemplo**.

6. Para adicionar conteúdo à sua BDC, selecione os três tipos de origens de dados. No passo 4, em **preencher sua BDC**, adicione o [FAQ de recuperação do BitLocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) URL no **URL** caixa.

   ![Selecione um serviço de QnA KB](../media/qnamaker-quickstart-kb/add-datasources.png)

7. Passo 5, selecione **criar a sua BDC**.

8. Embora o BDC está a ser criada, é apresentada uma janela de pop-up. O processo de extração demora alguns minutos para ler a página HTML e identificar as perguntas e respostas.

9. Depois do KB é criado com êxito, o **base de dados de conhecimento** é aberta a página. Pode editar o conteúdo de KB nesta página.

10. No canto superior direito, selecione **par adicionar QnA** para adicionar uma nova linha na **Editorial** secção do KB. Sob **pergunta**, introduza **Olá.** Sob **resposta**, introduza **Hello. Pergunte-me perguntas do bitlocker.**

   ![Adicione um par de QnA](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. No canto superior direito, selecione **guardar e treinar** para guardar as edições e preparar o modelo do QnA Maker. As edições não são mantidas, a menos que eles são salvos.

   ![Guardar e formação](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. No canto superior direito, selecione **testar** para testar a que as alterações que efetuou ocorreu em vigor. ENTER **Olá lá** na caixa e selecione Enter. Deverá ver a resposta que criou como resposta.

13. Selecione **Inspect** para examinar a resposta mais detalhadamente. A janela de teste é utilizada para testar as alterações para o KB antes de que publicadas.

   ![Painel de teste](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Selecione **teste** novamente para fechar a **teste** pop-up.

15. No menu junto a **edite**, selecione **Publish**. Em seguida, para confirmar, selecione **publicar** na página.

16. O serviço QnA Maker agora é publicado com êxito. Pode utilizar o ponto final na sua aplicação ou código de bot.

   ![Publicar](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento](../How-To/create-knowledge-base.md)
