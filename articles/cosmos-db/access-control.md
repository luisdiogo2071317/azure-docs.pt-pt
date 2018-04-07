---
title: A definição de controlo de acesso do BD Azure Cosmos | Microsoft Docs
description: Saiba como definir o controlo de acesso do BD Azure Cosmos.
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 740f1ca560207ada95dd03fbecc7f9940ee7b2a0
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
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
