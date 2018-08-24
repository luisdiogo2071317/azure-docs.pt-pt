---
title: Ligar ao Excel Online - Azure Logic Apps | Documentos da Microsoft
description: Gerir dados com APIs de REST do Excel Online e o Azure Logic Apps
ms.service: logic-apps
services: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: 94960b95e6de30159ec34b3f97bb5119cac42c35
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818104"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Gerir os dados do Excel Online com o Azure Logic Apps

Com o Azure Logic Apps e o conector do Excel Online, pode criar tarefas automatizadas e fluxos de trabalho com base nos seus dados no Excel Online para empresas ou o OneDrive. Os fluxos de trabalho podem realizar estas ações e outras pessoas com os seus dados, por exemplo:

* Crie tabelas e folhas de cálculo nova.
* Obter e gerir folhas de cálculo, tabelas e linhas.
* Adicione únicas linhas e colunas de chave.

Pode incluir ações nas suas aplicações lógicas que utilizam a saída do Excel Online ações. Este conector proporciona apenas ações como, por isso, para iniciar a sua aplicação lógica, utilizam um acionador separado, como um **periodicidade** acionador. Por exemplo, se criar folhas de cálculo de todas as semanas, pode enviar por e-mail sobre essas folhas de cálculo nova com o conector do Outlook do Office 365.

Se estiver familiarizado com aplicações lógicas, reveja [o que é o Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> O [Excel Online para empresas](/connectors/excelonlinebusiness/) e [Excel Online para o OneDrive](/connectors/excelonline/) conectores trabalham com o Azure Logic Apps e ser diferente do [conector do Excel para PowerApps](/connectors/excel/).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. 

* Uma [conta do Office 365](https://www.office.com/) para a sua conta profissional ou uma conta Microsoft pessoal 

  Os dados do Excel podem existir como um ficheiro de valores separados por vírgulas (CSV) numa pasta de armazenamento, por exemplo, no OneDrive. 
  Também pode utilizar o mesmo ficheiro CSV com o [conector de arquivos simples](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* A aplicação de lógica onde pretende aceder aos dados do Excel Online. Este conector proporciona apenas ações como, por isso, para iniciar a sua aplicação lógica, selecione um acionador separado, por exemplo, o **periodicidade** acionador.

## <a name="add-excel-action"></a>Adicionar ação de Excel

1. Na [portal do Azure](https://portal.azure.com), abra a aplicação lógica no Estruturador da aplicação lógica, se não estiver já abrir.

1. No acionador, escolha **novo passo**.

1. Na caixa de pesquisa, introduza "excel" como o filtro. Abaixo da lista de ações, selecione a ação que pretende.

1. Se lhe for pedido para iniciar sessão na sua conta Office 365, escolha **iniciar sessão**. 

   As suas credenciais autorizar a aplicação lógica para criar uma ligação para o Excel Online e aceder aos seus dados.

1. Continue a fornecer os detalhes necessários para a ação selecionada e a criação de fluxo de trabalho da sua aplicação lógica.

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos, como ações e limites, descritos por ficheiros de Swagger dos conectores, consulte estas páginas de referência do conector:

* [Excel Online para empresas](/connectors/excelonlinebusiness/) 
* [Excel Online para o OneDrive](/connectors/excelonline/) 

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre outras [conectores do Logic Apps](../connectors/apis-list.md)
