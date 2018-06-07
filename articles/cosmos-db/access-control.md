---
title: A definição de controlo de acesso do BD Azure Cosmos | Microsoft Docs
description: Saiba como definir o controlo de acesso do BD Azure Cosmos.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 2dae2b6291aa7ce18cc6f612b25cd5bdcc1ba753
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611148"
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
