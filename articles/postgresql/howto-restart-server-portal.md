---
title: Reinicie a base de dados do Azure para o servidor PostgreSQL com o portal do Azure
description: Este artigo descreve como pode reiniciar uma base de dados do Azure para o servidor PostgreSQL com o Portal do Azure.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: 28e99f64fdee414549c55f9666bfd53f07fb3efb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892699"
---
# <a name="restart-azure-database-for-postgresql-server-using-azure-portal"></a>Reinicie a base de dados do Azure para o servidor PostgreSQL com o portal do Azure
Este tópico descreve como pode reiniciar uma base de dados do Azure para o servidor PostgreSQL. Terá de reiniciar o servidor por motivos de manutenção, o que faz com que uma interrupção curta como o servidor executa a operação.

O reinício do servidor será bloqueado se o serviço está ocupado. Por exemplo, o serviço poderá estar a processar uma operação de pedido anteriormente, como o dimensionamento vCores.
 
O tempo necessário para concluir um reinício depende do processo de recuperação do PostgreSQL. Para diminuir o tempo de reinício, recomendamos que estará a minimizar a quantidade de atividade ocorrida no servidor antes do reinício.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de procedimentos, terá de:
- Um [base de dados do Azure para PostgreSQL server e base de dados](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Reiniciar o servidor

Os seguintes passos de reiniciar o servidor PostgreSQL:

1. No portal do Azure, selecione a base de dados do Azure para o servidor PostgreSQL.

2. Na barra de ferramentas do servidor **descrição geral** página, clique em **reiniciar**.

   ![Base de dados do Azure para PostgreSQL - descrição geral - Restart button](./media/howto-restart-server-portal/2-server.png)

3. Clique em **Sim** para confirmar o reinício do servidor.

   ![Confirmar a base de dados do Azure para PostgreSQL - reinício ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que o estado do servidor é alterado para "Reiniciar".

   ![Base de dados do Azure para PostgreSQL - estado de reinício ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme o reinício do servidor é efetuada com êxito.

   ![Base de dados do Azure para PostgreSQL - êxito de reinício ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Passos Seguintes

[Quickstart: Criar base de dados do Azure para o servidor PostgreSQL com o portal do Azure](./quickstart-create-server-database-portal.md)