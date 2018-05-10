---
title: Exemplos de início rápido do Azure do Monitor CLI 2.0. | Microsoft Docs
description: Comandos de exemplo CLI 2.0 para funcionalidades de monitorização do Azure. Monitor do Azure é um serviço Microsoft Azure permite-lhe enviar notificações de alerta, chamar URLs web com base nos valores de dados de telemetria configurado e serviços de Cloud de dimensionamento automático, as máquinas virtuais e aplicações Web.
author: kamathashwin
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2018
ms.author: ashwink
ms.openlocfilehash: 475ee794d2b67639b447a1ca66b12d3d589425cb
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="azure-monitor-cli-20-quick-start-samples"></a>Exemplos de início rápido do Azure do Monitor CLI 2.0
Este artigo mostra-lhe exemplo comandos de interface de linha de comandos (CLI) para o ajudar a aceder às funcionalidades de monitorização do Azure. Monitor do Azure permite-lhe para serviços de Cloud de dimensionamento automático, as máquinas virtuais e aplicações Web e enviar notificações de alerta ou chamar URLs web com base nos valores de dados de telemetria configurado.

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não instalou a CLI do Azure, siga as instruções para [instalar o 2.0 CLI do Azure](/cli/azure/install-azure-cli). Também pode utilizar [Shell de nuvem do Azure](/azure/cloud-shell) para executar o CLI como uma experiência interativa no seu browser. 

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure
O primeiro passo consiste em início de sessão para a sua conta do Azure.

```azurecli
az login
```

Depois de executar este comando, tem de iniciar sessão através das instruções no ecrã. Todos os comandos funcionam no contexto da sua subscrição predefinida.

Para listar os detalhes da subscrição atual, utilize o seguinte comando.

```azurecli
az account show
```

Para alterar o contexto de trabalho para uma subscrição diferente, utilize o seguinte comando.

```azurecli
az account set -s <Subscription ID or name>
```

Para ver uma lista de todos os comandos de Azure Monitor suportados, execute o seguinte.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Ver o registo de atividade para uma subscrição

Para ver uma lista de eventos de registo de atividade, execute o seguinte.

```azurecli
az monitor activity-log list
```

Tente o seguinte para ver todas as opções disponíveis.

```azurecli
az monitor activity-log list -h
```

Eis um exemplo para registos de lista por um resourceGroup

```azurecli
az monitor activity-log list --resource-group <group name>
```

Exemplo para registos de lista pelo autor da chamada

```azurecli
az monitor activity-log list --caller myname@company.com
```

Exemplo para registos de lista pelo autor da chamada num tipo de recurso, dentro de um intervalo de datas

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Trabalhar com alertas

Pode utilizar as informações na secção para trabalhar com alertas.

### <a name="get-alert-rules-in-a-resource-group"></a>Obter as regras de alerta num grupo de recursos

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-rule"></a>Criar uma regra de alerta métrica

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-rule"></a>Eliminar uma regra de alerta

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Perfis de registo

Utilize as informações nesta secção para trabalhar com perfis de registo.

### <a name="get-a-log-profile"></a>Obter um perfil de registo

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Adicionar um perfil de registo com retenção

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Adicionar um perfil de registo com retenção e EventHub

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Remover um perfil de registo

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Diagnóstico

Utilize as informações nesta secção para trabalhar com definições de diagnóstico.

### <a name="get-a-diagnostic-setting"></a>Obter uma definição de diagnóstico

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-log-setting"></a>Criar uma definição de registo de diagnóstico 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

### <a name="delete-a-diagnostic-setting"></a>Eliminar uma definição de diagnóstico

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Dimensionamento Automático

Utilize as informações nesta secção para trabalhar com definições de dimensionamento automático. Terá de modificar estes exemplos.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Obter definições de dimensionamento automático para um grupo de recursos

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Obter definições de dimensionamento automático por nome de um grupo de recursos

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Configurar as definições de dimensionamento automático

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```
