---
title: Exemplo do Script da CLI do Azure – Mapear um domínio personalizado para uma aplicação de funções | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Mapear um domínio personalizado para uma aplicação de funções no Azure.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: d2efd0891cb0010aac6135ce190ad77667f78efd
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843991"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Mapear um domínio personalizado para uma aplicação de funções

Este script de exemplo cria uma aplicação de funções com os respetivos recursos relacionados e, em seguida, mapeia `www.<yourdomain>` para o mesmo. Para mapear para um domínio personalizado, a aplicação de funções tem de ser criada num plano do Serviço de Aplicações e não num plano de consumo. As Funções do Azure apenas permitem mapear um domínio personalizado através de um registo A.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, tem de utilizar a versão 2.0 ou uma versão posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: cada comando na tabela está ligado a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Cria uma conta de armazenamento necessária para a aplicação de funções. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Cria um plano do Serviço de Aplicações necessário para mapear um domínio personalizado. |
| [az functionapp create]() | Cria uma aplicação de funções. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Mapeia um domínio personalizado para uma aplicação de funções. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI das Funções adicionais na [Documentação das Funções do Azure]().
