---
title: Utilizar o conector do servidor do SharePoint nas suas Logic Apps | Microsoft Docs
description: "Começar a utilizar o conector do SharePoint Server, nas suas Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: da863e0249cb46e4e569812a851f3199d57b2107
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-sharepoint-connector"></a>Começar a utilizar o conector do SharePoint
O conector do SharePoint fornece uma forma de trabalhar com apresenta uma lista do SharePoint.

Começar através da criação de uma aplicação lógica; consulte [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Criar uma ligação para o SharePoint
Para utilizar o conector do SharePoint, tem primeiro de criar um **ligação** , em seguida, forneça os detalhes para estas propriedades: 

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| Token |Sim |Fornecer Credenciais do SharePoint |

Para ligar ao **SharePoint**, introduza a sua identidade (nome de utilizador e palavra-passe, smart card credenciais, etc.) para SharePoint. Uma vez que já foi autenticado, pode continuar a utilizar o conector do SharePoint na sua aplicação lógica. 

Enquanto no designer da sua aplicação lógica, siga estes passos para iniciar sessão no SharePoint para criar a ligação **ligação** para utilização na sua aplicação lógica:

1. Introduza o SharePoint na caixa de pesquisa e aguarde que a procura para devolver todas as entradas com o SharePoint no nome:   
   ![Configurar o SharePoint][1]  
2. Selecione **SharePoint - quando é criado um ficheiro**   
3. Selecione **inicie sessão no SharePoint**:   
   ![Configurar o SharePoint][2]    
4. Fornecer as credenciais do SharePoint para iniciar sessão autenticar com o SharePoint   
   ![Configurar o SharePoint][3]     
5. Depois de concluir a autenticação irá ser redirecionado para a sua aplicação lógica para concluí-la através da configuração do SharePoint **quando é criado um ficheiro** caixa de diálogo.          
   ![Configurar o SharePoint][4]  
6. Em seguida, pode adicionar outros acionadores e ações que precisa para concluir a sua aplicação lógica.   
7. Guarde o trabalho selecionando **guardar** na barra de menus acima.  

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver todos os acionadores e ações definidas no swagger e consulte também os limites no [detalhes do conector](/connectors/sharepoint/).

## <a name="more-connectors"></a>Mais conectores
Volte à [lista APIs](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
