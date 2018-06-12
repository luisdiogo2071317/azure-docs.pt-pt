---
title: Utilizar o conector do servidor do SharePoint nas suas Logic Apps | Microsoft Docs
description: Começar a utilizar o conector do SharePoint Server, nas suas Logic apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: c153b4987e37a5d97b95d4f1249de1ed92e851b3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295949"
---
# <a name="get-started-with-the-sharepoint-connector"></a>Começar a utilizar o conector do SharePoint
O conector do SharePoint fornece uma forma para trabalhar com apresenta uma lista do SharePoint.

Começar através da criação de uma aplicação lógica; consulte [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Criar uma ligação para o SharePoint
Para utilizar o conector do SharePoint, tem primeiro de criar um **ligação** , em seguida, forneça os detalhes para estas propriedades: 

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| Certificado de |Sim |Fornecer Credenciais do SharePoint |

Para ligar ao **SharePoint**, introduza a sua identidade (nome de utilizador e palavra-passe, as credenciais de smartcart e assim sucessivamente). Assim que tiver autenticado, pode continuar a utilizar o conector do SharePoint na sua aplicação lógica. 

Enquanto no designer da sua aplicação lógica, utilize os seguintes passos para iniciar sessão e criar o **ligação** para utilização na sua aplicação lógica:

1. Introduza o SharePoint na caixa de pesquisa e aguarde que a procura para devolver todas as entradas com o SharePoint no nome:   
   ![Configurar o SharePoint][1]  
2. Selecione **SharePoint - quando é criado um ficheiro**   
3. Selecione **inicie sessão no SharePoint**:   
   ![Configurar o SharePoint][2]    
4. Fornecer as credenciais do SharePoint para iniciar sessão autenticar com o SharePoint   
   ![Configurar o SharePoint][3]     
5. Após a conclusão da autenticação, está a ser redirecionado para a sua aplicação lógica para concluí-la através da configuração do SharePoint **quando é criado um ficheiro** caixa de diálogo.          
   ![Configurar o SharePoint][4]  
6. Em seguida, pode adicionar outros acionadores e ações que precisa para concluir a sua aplicação lógica.   
7. Guarde o trabalho selecionando **guardar** no menu (direção ao topo).

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver todos os acionadores e ações definidas no swagger e consulte também os limites no [detalhes do conector](/connectors/sharepoint/).

## <a name="more-connectors"></a>Mais conectores
Volte à [lista APIs](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
