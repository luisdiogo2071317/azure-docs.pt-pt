---
title: Ligue ao Dropbox - Azure Logic Apps | Documentos da Microsoft
description: Carregar e gerir ficheiros com as APIs REST do Dropbox e o Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/15/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 256a0b34d5050e17abe5bb98ca0c13ab0b61787e
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094443"
---
# <a name="get-started-with-the-dropbox-connector"></a>Começar com o conector da Dropbox
Ligue-se à Dropbox para gerir os seus ficheiros. Pode executar várias ações como carregar, atualizar, obter e eliminar ficheiros no Dropbox.

Para utilizar [qualquer conector](apis-list.md), tem primeiro de criar uma aplicação lógica. Pode começar a utilizar pelo [criar uma aplicação lógica agora](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-dropbox"></a>Ligue à Dropbox
Antes da aplicação lógica pode aceder a qualquer serviço, tem primeiro de criar uma *ligação* ao serviço. Uma ligação fornece conectividade entre uma aplicação lógica e outro serviço. Por exemplo, para ligar a Dropbox, primeiro tem um Dropbox *ligação*. Para criar uma ligação, terá de fornecer as credenciais que normalmente utiliza para aceder ao serviço que deseja se conectar. Por isso, no exemplo Dropbox, seria necessário as credenciais para a sua conta do Dropbox para criar a ligação ao Dropbox. 

### <a name="create-a-connection-to-dropbox"></a>Criar uma ligação para a Dropbox
> [!INCLUDE [Steps to create a connection to Dropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>Utilizar um acionador do Dropbox
Um acionador é um evento que pode ser utilizado para iniciar o fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre os acionadores](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Neste exemplo, utilizamos o **quando é criado um ficheiro** acionador. Quando ocorre este acionador, vamos chamar o **obter conteúdo do ficheiro através do caminho** ação do Dropbox. 

1. Introduza *dropbox* na caixa de pesquisa no estruturador do Logic Apps, em seguida, selecione a **Dropbox - quando é criado um ficheiro** acionador.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Selecione a pasta em que pretende controlar a criação do ficheiro. Selecione... (identificado na caixa vermelha) e navegue até à pasta que pretende selecionar para o acionador da entrada.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>Utilize uma ação do Dropbox
Uma ação é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre as ações](../logic-apps/logic-apps-overview.md#logic-app-concepts).

Agora que o acionador foi adicionado, siga estes passos para adicionar uma ação que irá obter conteúdo do arquivo novo.

1. Selecione **+ novo passo** para adicionar a ação a tomar quando é criado um novo ficheiro.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Selecione **adicionar uma ação**. Ela abre a caixa de pesquisa, onde pode procurar qualquer ação gostaria de fazer.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Introduza *dropbox* para procurar ações relacionadas com o Dropbox.  
4. Selecione **Dropbox - obter conteúdo do ficheiro através do caminho** como a ação a tomar quando é criado um novo ficheiro na pasta Dropbox selecionada. É aberto o bloco de controle de ação. Será solicitado para autorizar a aplicação lógica para aceder à sua conta do Dropbox, se não o tiver feito isso anteriormente.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Selecione... (localizado no lado direito do **caminho do ficheiro** controlo) e navegue para o caminho de ficheiro que pretende utilizar. Em alternativa, utilizar o **caminho do ficheiro** token para acelerar a criação da aplicação lógica.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Guarde o seu trabalho e crie um novo ficheiro na Dropbox para ativar o seu fluxo de trabalho.  

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver os acionadores e as ações definidas no swagger e também ver quaisquer limites na [detalhes do conector](/connectors/dropbox/).

## <a name="more-connectors"></a>Mais conectores
Volte para o [lista APIs](apis-list.md).
