---
title: "A definição de controlo de acesso do BD Azure Cosmos | Microsoft Docs"
description: Saiba como definir o controlo de acesso do BD Azure Cosmos.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: mimig
ms.openlocfilehash: 5ef4a12c8229d8801a5d9749514a69c2c1e19499
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Controlo de acesso do BD Azure Cosmos

Para adicionar o acesso de leitor de conta de base de dados do Azure Cosmos a sua conta de utilizador, ter um proprietário de subscrição, execute os seguintes passos no portal do Azure.

1. Abra o portal do Azure e selecione a sua conta de base de dados do Azure Cosmos.
2. Clique em de **(IAM) do controlo de acesso** separador e, em seguida, clique em **+ adicionar**.
3. No **adicionar permissões** painel, no **função** caixa, selecione **função de leitor de conta do Cosmos DB**.
4. No **atribuir acesso à caixa**, selecione **utilizador do Azure AD, grupo ou aplicação**.
5. Selecione o utilizador, grupo ou aplicação no seu diretório para os quais pretende conceder acesso.  Pode procurar o diretório de por nome a apresentar, endereço de correio eletrónico ou identificadores de objetos.
    O utilizador selecionado, grupo ou aplicação é apresentada na lista de membros selecionados.
6. Clique em **Guardar**.

A entidade pode agora ler recursos de base de dados do Azure Cosmos.
