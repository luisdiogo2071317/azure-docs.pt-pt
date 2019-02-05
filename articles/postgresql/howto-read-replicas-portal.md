---
title: Gerir réplicas de leitura no portal do Azure para a base de dados do Azure para PostgreSQL
description: Este artigo descreve Gerir base de dados do Azure para PostgreSQL, leia as réplicas no portal do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 37150f67e29dae0357c978cfaea9abeebeef428c
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691410"
---
# <a name="how-to-create-and-manage-read-replicas-in-the-azure-portal"></a>Como criar e gerir ler réplicas no portal do Azure

> [!IMPORTANT]
> A funcionalidade de réplica de leitura está em pré-visualização pública.


Neste artigo, aprenderá como criar e gerir réplicas de leitura na base de dados do Azure para o serviço PostgreSQL no portal do Azure. Para obter mais informações acerca de réplicas de leitura [leia a documentação de conceitos](concepts-read-replicas.md).

## <a name="prerequisites"></a>Pré-requisitos
- Uma [base de dados do Azure para o servidor PostgreSQL](quickstart-create-server-database-portal.md) que será o servidor mestre.

## <a name="prepare-the-master-server"></a>Preparar o servidor mestre
Este passo de preparação mestre aplica-se aos servidores apenas para fins gerais e memória otimizada.

O **azure.replication_support** parâmetro tem de ser definido para a RÉPLICA no servidor principal. Alterar este parâmetro requer um reinício do servidor para entrar em vigor.

1. No portal do Azure, selecione a base de dados do Azure existente para o servidor PostgreSQL que pretende utilizar como um modelo.

2. Selecione **parâmetros do servidor** no menu à esquerda.

3. Procure **azure.replication_support**.

   ![Base de dados do Azure para PostgreSQL - azure.replication_support](./media/howto-read-replicas-portal/azure-replication-parameter.png)

4. Definir **azure.replication_support** à RÉPLICA. **Guardar** a alteração.

   ![Base de dados do Azure para PostgreSQL - RÉPLICA e guardar](./media/howto-read-replicas-portal/save-parameter-replica.png)

5. Depois de guardar está concluída, receberá uma notificação.

   ![Base de dados do Azure para PostgreSQL – guardar notificação](./media/howto-read-replicas-portal/parameter-save-notification.png)

6. Reinicie o servidor para aplicar a alteração depois de guardar. Ver [a documentação de reinício](howto-restart-server-portal.md) para saber como reiniciar um servidor.


## <a name="create-a-read-replica"></a>Criar uma réplica de leitura
Réplicas de leitura podem ser criadas através dos seguintes passos:
1.  Selecione a base de dados do Azure existente para o servidor PostgreSQL que pretende utilizar como um modelo. 

2.  Selecione os replicação no menu, em definições.

   Se ainda não tiver definido **azure.replication_support** para a RÉPLICA no fins gerais ou com otimização de memória principal e reiniciar o servidor, verá uma mensagem com instruções para fazer isso. Fazê-lo antes de continuar com o criar.

3.  Selecione Adicionar réplica.

   ![Base de dados do Azure para PostgreSQL - Adicionar réplica](./media/howto-read-replicas-portal/add-replica.png)

4.  Introduza um nome para o servidor de réplica e selecione OK para confirmar a criação da réplica.

   ![Base de dados do Azure para PostgreSQL - réplica de nome](./media/howto-read-replicas-portal/name-replica.png) 

> [!IMPORTANT]
> Réplicas de leitura são criadas com a mesma configuração de servidor como o modelo. Depois de uma réplica tiver sido criado, o escalão de preço (exceto para e do Basic), geração de computação, vCores, armazenamento e período de retenção de cópia de segurança podem ser alterados independentemente do servidor mestre.

> [!IMPORTANT]
> Antes de configuração do servidor de um modelo é atualizada para novos valores, a configuração das réplicas deve ser atualizada para valores iguais ou superior. Tentar fazê-lo caso contrário, fará com que um erro. Isto garante que as réplicas estão capazes de acompanhar as alterações efetuadas a mestre. 


Quando o servidor de réplica tiver sido criado, podem ser exibido na janela de replicação.

![Base de dados do Azure para PostgreSQL - nova réplica](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Parar replicação

> [!IMPORTANT]
> A parar a replicação para um servidor é irreversível. Assim que a replicação foi parado entre um mestre e de réplica, não pode ser anulada. O servidor de réplica, em seguida, torna-se um servidor autónomo e agora oferece suporte a leitura e escrita. Este servidor não pode se transformar numa réplica novamente.

Para parar a replicação entre um mestre e uma réplica no portal do Azure, utilize os seguintes passos:
1.  No portal do Azure, selecione a sua base de dados principal do Azure para o servidor PostgreSQL.

2.  Selecione os replicação no menu, em definições.

3.  Selecione o servidor de réplica que pretende parar a replicação de.

   ![Base de dados do Azure para PostgreSQL - réplica selecione](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Selecione a paragem da replicação.

   ![Base de dados do Azure para PostgreSQL - selecione paragem da replicação](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5.  Confirme que pretende parar a replicação ao clicar em OK.

   ![Base de dados do Azure para PostgreSQL - Confirmar paragem da replicação](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master"></a>Eliminar um modelo

> [!IMPORTANT]
> A eliminar um servidor principal para a replicação para todos os servidores de réplica. Servidores de réplica se tornar a servidores autónomos que agora oferecem suporte a leitura e escrita.
A eliminar um mestre segue os mesmos passos como para a base de dados autónomo para o servidor PostgreSQL. Para eliminar um servidor do portal do Azure, efetue o seguinte:

1.  No portal do Azure, selecione a sua base de dados principal do Azure para o servidor PostgreSQL.

2.  Desde a descrição de geral, selecione eliminar.

   ![Base de dados do Azure para PostgreSQL - eliminar servidor](./media/howto-read-replicas-portal/delete-server.png)
 
3.  Escreva o nome do servidor mestre e selecione Delete para confirmar a eliminação do servidor mestre.

   ![Base de dados do Azure para PostgreSQL - Confirmar eliminação](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Eliminar uma réplica
Para eliminar uma réplica de leitura, pode seguir os mesmos passos tal como acontece com a eliminação de um servidor mestre acima. Primeiro abrir a página de descrição geral da réplica, em seguida, selecione eliminar.

   ![Base de dados do Azure para PostgreSQL - Eliminar réplica](./media/howto-read-replicas-portal/delete-replica.png)
 
Em alternativa, pode eliminá-la na janela de replicação.
1.  No portal do Azure, selecione a sua base de dados principal do Azure para o servidor PostgreSQL.

2.  Selecione os replicação no menu, em definições.

3.  Selecione o servidor de réplica que pretende eliminar. 

   ![Base de dados do Azure para PostgreSQL - réplica selecione](./media/howto-read-replicas-portal/select-replica.png)
 
4.  Selecione Eliminar réplica.

   ![Base de dados do Azure para PostgreSQL - selecione Eliminar réplica](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5.  Escreva o nome da réplica e selecione Delete para confirmar a eliminação da réplica.

   ![Base de dados do Azure para PostgreSQL - confirmar eliminar réplica](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Monitorizar uma réplica
### <a name="max-lag-across-replicas"></a>Atraso máximo entre as réplicas
O **atraso máx. entre as réplicas** métrica mostra o desfasamento de bytes entre o modelo global e o máximo lagging réplica. 

1.  No portal do Azure, selecione o **mestre** base de dados do Azure para o servidor PostgreSQL.

2.  Selecione as métricas. Na janela de métricas, selecione **máximo de atraso em réplicas**.

    ![Base de dados do Azure para PostgreSQL – atraso máx. do Monitor em réplicas](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Selecione **Max** como a agregação. 

### <a name="replica-lag"></a>Atraso de réplica
O **desfasamento de réplica** métrica mostra o tempo, uma vez que o último reproduzidos transação nesta réplica. Se não houver nenhuma transação que ocorrem no seu controlador, a métrica reflete este intervalo de tempo.

1.  No portal do Azure, selecione um **réplica** base de dados do Azure para o servidor PostgreSQL.

2.  Selecione as métricas. Na janela de métricas, selecione **desfasamento de réplica**.

   ![Base de dados do Azure para PostgreSQL – atraso de réplica do Monitor](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3.  Selecione **Max** como a agregação. 
 
## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [ler réplicas na base de dados do Azure para PostgreSQL](concepts-read-replicas.md).