---
title: Exemplo de Script da CLI do Azure - Criar uma subscrição do Azure Event Grid | Microsoft Docs
description: Neste tópico, o script da CLI do Azure mostra como criar um subscrição do Azure Event Grid ao nível da conta para Alterações de Estado de Tarefa.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: a3cff649001adf569f1454d16a2a97b32972ef00
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612629"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Exemplo da CLI: Criar uma subscrição do Azure Event Grid 

Neste artigo, o script da CLI do Azure mostra como criar um subscrição do Azure Event Grid ao nível da conta para Alterações de Estado de Tarefa.

## <a name="prerequisites"></a>Pré-requisitos 

- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](../create-account-cli-how-to.md).

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais exemplos, veja [Azure CLI samples](../cli-samples.md) (Exemplos de CLI do Azure).
