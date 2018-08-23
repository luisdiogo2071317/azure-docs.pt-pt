---
title: A definição de controlo de acesso no Azure Cosmos DB | Documentos da Microsoft
description: Saiba como configurar o controlo de acesso no Azure Cosmos DB.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: rafats
ms.openlocfilehash: 0d4595bcf8a9f009dce928d3f3cbc442328ec39b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42058455"
---
# <a name="access-control-in-azure-cosmos-db"></a>Controlo de acesso no Azure Cosmos DB

Para adicionar o acesso de leitor de conta do Azure Cosmos DB à sua conta de utilizador, ter um proprietário de subscrição, execute os seguintes passos no portal do Azure.

1. Abra o portal do Azure e selecione a sua conta do Azure Cosmos DB.
2. Clique nas **controlo de acesso (IAM)** separador e, em seguida, clique em **+ adicionar**.
3. Na **adicionar permissões** painel, na **função** caixa, selecione **função de leitor de conta do Cosmos DB**.
4. Na **atribuir acesso a caixa**, selecione **utilizador do Azure AD, grupo ou aplicação**.
5. Selecione o utilizador, grupo ou aplicação no seu diretório ao qual pretende conceder acesso.  Pode procurar o diretório por nome a apresentar, endereço de e-mail ou identificadores de objetos.
    O utilizador selecionado, grupo ou aplicação é apresentada na lista de membros selecionados.
6. Clique em **Guardar**.

A entidade já pode ler os recursos do Azure Cosmos DB.
