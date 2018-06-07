---
title: Ligar para o OneDrive - as do Azure Logic Apps | Microsoft Docs
description: Carregar e gerir os ficheiros com as APIs REST do OneDrive e do Azure Logic Apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 12df6efc21be30f1c42fa7db98823908ae0f5964
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34610060"
---
# <a name="get-started-with-the-onedrive-connector"></a>Começar a utilizar o conector do OneDrive
Ligar para o OneDrive para gerir os seus ficheiros, incluindo carregar, obter, eliminar ficheiros e muito mais. 

Com o OneDrive, pode: 

* Criar o fluxo de trabalho ao armazenar os ficheiros no OneDrive ou atualizar os ficheiros existentes no OneDrive. 
* Utilize acionadores para iniciar o fluxo de trabalho quando um ficheiro é criado ou atualizado no seu OneDrive.
* Utilize ações para criar um ficheiro, eliminar um ficheiro e muito mais. Por exemplo, quando é recebido um novo e-mail do Office 365 com um anexo (um acionador), crie um novo ficheiro no OneDrive (uma ação).

Este artigo mostra-lhe como utilizar o conector do OneDrive numa aplicação lógica e também apresenta uma lista de acionadores e ações.

Para saber mais sobre Logic Apps, consulte o artigo [que são logic apps](../logic-apps/logic-apps-overview.md) e [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Ligar para o OneDrive
Antes da aplicação lógica pode aceder a qualquer serviço, tem primeiro de criar um *ligação* ao serviço. Uma ligação oferece a conectividade entre uma aplicação lógica e outro serviço. Por exemplo, para ligar para o OneDrive, terá primeiro um OneDrive *ligação*. Para criar uma ligação, introduza as credenciais que normalmente utiliza para aceder ao serviço que pretende ligar. Por isso, com o OneDrive, introduza as credenciais para a sua conta do OneDrive para criar a ligação.

### <a name="create-the-connection"></a>Criar a ligação
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Utilizar um acionador
Um acionador é um evento que pode ser utilizado para iniciar o fluxo de trabalho definido numa aplicação lógica. Acionadores "consultam" o serviço de um intervalo e a frequência com que pretende. [Saiba mais sobre acionadores](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Na aplicação lógica, escreva "onedrive" para obter uma lista dos acionadores:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Selecione **quando um ficheiro é modificado**. Se já existe uma ligação, em seguida, selecione o botão de selecionador de mostrar para selecionar uma pasta.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Se lhe for pedido para iniciar sessão, em seguida, introduza o início de sessão nos detalhes para criar a ligação. [Criar a ligação](connectors-create-api-onedrive.md#create-the-connection) este artigo apresenta os passos. 
   
   > [!NOTE]
   > Neste exemplo, a aplicação lógica é executado quando um ficheiro na pasta que escolher é atualizado. Para ver os resultados deste acionador, adicione outra ação que envia-lhe uma mensagem de e-mail. Por exemplo, adicionar o Office 365 Outlook *enviar um e-mail* ação e-mails quando um ficheiro é atualizado. 

3. Selecione o **editar** botão e defina o **frequência** e **intervalo** valores. Por exemplo, se quiser acionar a consultar a cada 15 minutos, em seguida, defina o **frequência** para **minuto**e defina o **intervalo** para **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Guardar** as suas alterações (canto superior esquerdo da barra de ferramentas). A aplicação lógica é guardada e pode ser ativada automaticamente.

## <a name="use-an-action"></a>Utilizar uma ação
Uma ação é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre as ações](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Selecione o sinal de adição. Pode ver várias opções: **adicionar uma ação**, **adicionar uma condição**, ou um do **mais** opções.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Escolha **adicionar uma ação**.
3. Na caixa de texto, escreva "onedrive" para obter uma lista de todas as ações disponíveis.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. No nosso exemplo, escolha **OneDrive - criar o ficheiro**. Se já existe uma ligação, em seguida, selecione o **caminho da pasta** para colocar o ficheiro, introduza o **nome de ficheiro**e escolha o **conteúdo do ficheiro** pretende:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Se lhe for pedido para obter as informações de ligação, em seguida, introduza os detalhes para criar a ligação. [Criar a ligação](connectors-create-api-onedrive.md#create-the-connection) este artigo descreve estas propriedades. 
   
   > [!NOTE]
   > Neste exemplo, vamos criar um novo ficheiro numa pasta OneDrive. Pode utilizar o resultado de Acionador de outro para criar o ficheiro do OneDrive. Por exemplo, adicionar o Office 365 Outlook *quando chega um novo e-mail* acionador. Em seguida, adicione o OneDrive *criar ficheiro* ação que utiliza os anexos e o tipo de conteúdo campos dentro de um ForEach para criar o novo ficheiro no OneDrive. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Guardar** as suas alterações (canto superior esquerdo da barra de ferramentas). A aplicação lógica é guardada e pode ser ativada automaticamente.


## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver todos os acionadores e ações definidas no swagger e consulte também os limites no [detalhes do conector](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Mais conectores
Volte à [lista APIs](apis-list.md).