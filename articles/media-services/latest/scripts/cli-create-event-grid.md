---
title: Exemplo de Script da CLI do Azure - Criar uma subscrição do Azure Event Grid | Microsoft Docs
description: Neste tópico, o script da CLI do Azure mostra como criar um subscrição do Azure Event Grid ao nível da conta para Alterações de Estado de Tarefa.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/11/2018
ms.author: juliako
ms.openlocfilehash: b46cd5cf8f730a680078dde06b58eb31846e0a76
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161317"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Exemplo da CLI: Criar uma subscrição do Azure Event Grid 

Neste artigo, o script da CLI do Azure mostra como criar um subscrição do Azure Event Grid ao nível da conta para Alterações de Estado de Tarefa.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Passos seguintes

Para obter mais exemplos, veja [Azure CLI samples](../cli-samples.md) (Exemplos de CLI do Azure).
