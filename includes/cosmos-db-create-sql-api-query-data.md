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
ms.openlocfilehash: e6e70da1f939547cdb589ae7bcb6ed1f6148f22e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31429432"
---
Pode utilizar agora consultas no Data Explorer para obter e filtrar os seus dados.

1. Veja-o por predefinição, a consulta está definida para `SELECT * FROM c`. Esta consulta predefinida obtém e apresenta todos os documentos na coleção. 

    ![A consulta predefinida no Data Explorer é "SELECT * FROM c"](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)

2. Mantenha-se no separador **Documentos** e altere a consulta ao clicar no botão **Editar Filtro**, adicionar `ORDER BY c._ts DESC` à caixa de predicado de consulta e, em seguida, ao clicar em **Aplicar Filtro**.

    ![Altere a consulta predefinida ao adicionar ORDER BY c._ts DESC e clique em Aplicar Filtro](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

Esta consulta modificada lista os documentos por ordem decrescente com base no respetivo carimbo de data/hora, por isso agora o segundo documento está listado primeiro. Se estiver familiarizado com a sintaxe do SQL, pode introduzir quaisquer [consultas SQL](../articles/cosmos-db/sql-api-sql-query.md) suportadas nesta caixa. 

Esta ação conclui o nosso trabalho no Data Explorer. Antes de continuarmos para o trabalho com código, note que também pode utilizar o Data Explorer para criar procedimentos armazenados, UDFs e acionadores, para realizar lógica empresarial do lado do servidor, bem como débito de escala. O Data Explorer expõe todos os acessos a dados programáticos incorporados que estão disponíveis nas APIs, mas disponibiliza acesso fácil aos seus dados no portal do Azure.