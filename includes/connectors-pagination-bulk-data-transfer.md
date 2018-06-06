---
title: incluir ficheiro
description: incluir ficheiro
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/09/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 524bc1d3a19ad8ecfc8d0d210e735d6a9ae0058b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678316"
---
### <a name="set-up-pagination"></a>Configurar a paginação

Para alguns conetores e respetivas ações que obtêm vários itens, os resultados podem exceder o tamanho da página o conector predefinido. Neste caso, a ação devolve a primeira página de resultados. Por exemplo, o tamanho de página predefinida para o **do SQL Server - Get linhas** ação é 2048, mas podem divergir com base nas outras definições. Para se certificar de que obtém todos os registos, ative o **paginação** a definição de ação. Esta definição não tem a aplicação lógica peça o conector de registos restantes, mas devolver todos os resultados como uma única mensagem quando a ação for concluída. 

Seguem-se apenas a alguns conectores onde pode ativar a paginação para as ações específicas: 

* <a href="https://docs.microsoft.com/connectors/db2/" target="_blank">DB2</a>
* <a href="https://docs.microsoft.com/connectors/dynamicscrmonline/" target="_blank">Dynamics 365 CRM Online</a>
* <a href="https://docs.microsoft.com/connectors/excel/" target="_blank">Excel</a>
* <a href="https://docs.microsoft.com/connectors/oracle/" target="_blank">Oracle Database</a>
* <a href="https://docs.microsoft.com/connectors/sharepointonline/" target="_blank">SharePoint</a>
* <a href="https://docs.microsoft.com/connectors/sql/" target="_blank">SQL Server</a> 

Eis um exemplo para o **obter linhas** ação:

1. Para determinar se a ação suportada paginação, abra a ação **definições**. 

   ![A ação de abrir "Definições"](./media/connectors-pagination-bulk-data-transfer/sql-action-settings.png)

2. Se a ação suportada paginação, altere o **paginação** definição de **desativar** para **no**. Para se certificar de que a ação devolve um conjunto mínimo de resultados, especifique um valor para **limite**.

   ![Especifique se a ação de devolver um número mínimo de resultados](./media/connectors-pagination-bulk-data-transfer/sql-action-settings-pagination.png)

3. Quando estiver pronto, selecione **feito**.