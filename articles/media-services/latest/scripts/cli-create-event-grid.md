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
ms.openlocfilehash: 5293ac55fdb13bba85996f5ed81034d4ebeeff12
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093025"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Exemplo da CLI: Criar uma subscrição do Azure Event Grid 

Neste artigo, o script da CLI do Azure mostra como criar um subscrição do Azure Event Grid ao nível da conta para Alterações de Estado de Tarefa.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Passos seguintes

Para obter mais exemplos, veja [Azure CLI samples](../cli-samples.md) (Exemplos de CLI do Azure).
