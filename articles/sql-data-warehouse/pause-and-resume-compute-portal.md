---
title: "Colocar em pausa e retomar de computação no armazém de dados de SQL do Azure - portal do Azure | Microsoft Docs"
description: "Tarefas de portais do Azure que colocar em pausa para um Azure SQL Data Warehouse reduzir os custos de computação. Retomar a computação quando estiver pronto para utilizar o armazém de dados."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/23/2018
ms.author: barbkess
ms.openlocfilehash: d5200cc2e07dea9e34d23b0f3fb89e76a65515d9
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Colocar em pausa e retomar a computação para um Azure SQL Data Warehouse no portal do Azure
Computação pausa de um Azure SQL Data Warehouse reduzir os custos. Retomar a computação quando estiver pronto para utilizar o armazém de dados.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de começar

Utilize [criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados chamado **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Computação pausa
Para reduzir os custos, pode colocar em pausa e retomar a computação recursos a pedido. Por exemplo, se não utilizar a base de dados durante a noite e no fim de semana, pode colocar em pausa-lo durante essas horas e retomá-lo durante o dia. Não lhe será cobrado recursos de computação enquanto a base de dados está em pausa. No entanto, continuará a ser-lhe cobrados de armazenamento. 

Siga estes passos para colocar em pausa um SQL data warehouse.

1. Clique em **bases de dados SQL** na página da esquerda do portal do Azure.
2. Selecione **mySampleDataWarehouse** do **bases de dados SQL** página. Esta ação abre o armazém de dados. 
3. No **mySampleDataWarehouse** página, repare **estado** é **Online**.

    ![Online de computação](media/pause-and-resume-compute-portal/compute-online.png)

4. Para colocar em pausa o armazém de dados, clique em de **colocar em pausa** botão. 
5. É apresentada uma pergunta de confirmação perguntar se pretende continuar. Clique em **Sim**.
6. Aguarde uns instantes e, em seguida, repare a **estado** é **Pausing**.

    ![A colocar em pausa](media/pause-and-resume-compute-portal/pausing.png)

7. Depois de concluída a operação de pausa, o estado é **em pausa** e o botão de opção **iniciar**.
8. Os recursos de computação para o armazém de dados estão agora offline. Não lhe será cobrado da computação até retomar o serviço.

    ![Offline de computação](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Retomar de computação
Siga estes passos para retomar um SQL data warehouse.

1. Clique em **bases de dados SQL** na página da esquerda do portal do Azure.
2. Selecione **mySampleDataWarehouse** do **bases de dados SQL** página. Esta ação abre o armazém de dados. 
3. No **mySampleDataWarehouse** página, repare **estado** é **em pausa**.

    ![Offline de computação](media/pause-and-resume-compute-portal/compute-offline.png)

4. Para retomar o armazém de dados, clique em **iniciar**. 
5. É apresentada uma pergunta de confirmação perguntar se pretende iniciar. Clique em **Sim**.
6. Tenha em atenção o **estado** é **a retomar**.

    ![A retomar](media/pause-and-resume-compute-portal/resuming.png)

7. Quando o armazém de dados estiver novamente online, o estado é **Online** e o botão de opção **pausa**.
8. Os recursos de computação para o armazém de dados agora estão online e pode utilizar o serviço. Custos de computação têm retomado.

    ![Online de computação](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Limpar recursos

Estão a ser-lhe cobrados para unidades do data warehouse e os dados armazenados no seu armazém de dados. Estes recursos de computação e armazenamento são faturados em separado. 

- Se pretender manter os dados no armazenamento, coloque em pausa computação.
- Se quiser remover futuras cobranças, pode eliminar o armazém de dados. 

Siga estes passos para limpar os recursos conforme quiser.

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)e clique no seu armazém de dados.

    ![Limpar recursos](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Para interromper a computação, clique no botão **Pausar**. Quando o armazém de dados estiver em pausa, verá um botão **Iniciar**.  Para retomar a computação, clique em **Iniciar**.

2. Para remover o armazém de dados para não lhe ser cobrada a computação ou o armazenamento, clique em **Eliminar**.

3. Para remover o servidor SQL que criou, clique em **mynewserver 20171113.database.windows.net**e, em seguida, clique em **eliminar**.  Tenha cuidado com esta eliminação, uma vez que eliminar o servidor também elimina todas as bases de dados atribuídas ao mesmo.

4. Para remover o grupo de recursos, clique em **myResourceGroup** e, em seguida, clique em **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Passos Seguintes
Agora que tem em pausa e retomar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
