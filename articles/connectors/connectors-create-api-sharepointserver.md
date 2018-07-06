---
title: Utilizar o conector de servidor do SharePoint nas suas aplicações lógicas | Documentos da Microsoft
description: Começar a utilizar o conector de servidor do SharePoint nas suas aplicações lógicas
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
ms.openlocfilehash: 3ac3105231017cdbf8bfcf143b26451a68da5283
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868706"
---
# <a name="get-started-with-the-sharepoint-connector"></a>Introdução ao conector do SharePoint
O conector do SharePoint fornece uma maneira de trabalhar com listas no SharePoint.

Comece por criar uma aplicação de lógica; ver [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Criar uma ligação para SharePoint
Para utilizar o conector do SharePoint, primeiro de criar uma **ligação** , em seguida, forneça os detalhes para estas propriedades: 

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| Certificado de |Sim |Fornecer Credenciais do SharePoint |

Para ligar à **SharePoint**, introduza a sua identidade (nome de utilizador e palavra-passe, credenciais de smart card etc.). Depois de autenticado, pode continuar a utilizar o conector do SharePoint na sua aplicação lógica. 

Enquanto no designer da sua aplicação lógica, utilize os seguintes passos para iniciar sessão e criar os **ligação** para utilização na sua aplicação lógica:

1. Introduza o SharePoint na caixa de pesquisa e aguarde que a procura para devolver todas as entradas com o SharePoint no nome:   
   ![Configurar o SharePoint][1]  
2. Selecione **SharePoint - quando é criado um ficheiro**   
3. Selecione **iniciar sessão no SharePoint**:   
   ![Configurar o SharePoint][2]    
4. Forneça as suas credenciais do SharePoint para iniciar sessão autenticar com o SharePoint   
   ![Configurar o SharePoint][3]     
5. Após a conclusão da autenticação, está redirecionado para a aplicação lógica para concluí-lo através da configuração do SharePoint **quando é criado um ficheiro** caixa de diálogo.          
   ![Configurar o SharePoint][4]  
6. Em seguida, pode adicionar outros acionadores e ações que precisa de executar a aplicação lógica.   
7. Guarde o seu trabalho, selecionando **guardar** no menu (em direção ao topo).

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver os acionadores e as ações definidas no swagger e também ver quaisquer limites na [detalhes do conector](/connectors/sharepoint/).

## <a name="more-connectors"></a>Mais conectores
Volte para o [lista APIs](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
