---
title: incluir ficheiro
description: incluir ficheiro
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: b656001c8a7d1bed21c208bc643018c5f751e09c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38733830"
---
Agora, pode utilizar a ferramenta Data Explorer no portal do Azure para criar uma base de dados de gráfico. 

1. Clique em **Data Explorer** > **Novo Gráfico**.

    A área **Adicionar Gráfico** é apresentada na extremidade direita, pelo que poderá ter de se deslocar para a direita para vê-la.

    ![O Data Explorer do portal do Azure, página Adicionar Gráfico](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. Na página **Adicionar gráfico**, introduza as definições para o novo gráfico.

    Definição|Valor sugerido|Descrição
    ---|---|---
    ID da base de dados|base de dados de exemplo|Designe a nova base de dados como *sample-database*. Os nomes das bases de dados têm de ter entre um e 255 carateres e não podem conter `/ \ # ?` nem espaços à direita.
    ID do Graph|gráfico de exemplo|Denomine a nova coleção como *sample-graph*. Os nomes dos gráficos têm os mesmos requisitos de carateres que os IDs das bases de dados.
    Capacidade de Armazenamento|Fixa (10 GB)|Deixe o valor predefinido de **Fixa (10 GB)**. Este valor é a capacidade de armazenamento da base de dados.
    Débito|400 RUs|Altere o débito para 400 unidades de pedido por segundo (RU/s). Se pretender reduzir a latência, pode aumentar o débito mais tarde.

3. Assim que o formulário estiver preenchido, clique em **OK**.
