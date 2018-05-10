---
title: SDKs de grelha de eventos do Azure
description: Descreve os SDKs de grelha de eventos do Azure. Estes SDKs fornecem gestão, a publicação e consumo.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: d9bb4b3b161060f20fca34760872a24cbfcabf30
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>SDKs de grelha de eventos para gestão e de publicação

Grelha de evento disponibiliza SDKs que lhe permite gerir os recursos e publicar eventos programaticamente.

## <a name="management-sdks"></a>SDKs de gestão

A gestão SDKs permitem-lhe criar, atualizar e eliminar eventos grelha tópicos e subscrições. Atualmente, os SDKs seguintes estão disponíveis:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure.eventgrid-2018-01-01/azure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Dados plane SDKs

O plane dados SDKs permitem-lhe publicar eventos de tópicos, tendo cuidado de autenticação, que formam o evento e publicar no modo assíncrono para o ponto final especificado. Eles também permitem-lhe consumir primeiro eventos de terceiros. Atualmente, os SDKs seguintes estão disponíveis:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [Novidades grelha de evento?](overview.md)
* Para comandos de grelha de evento na CLI do Azure, consulte [CLI do Azure](/cli/azure/eventgrid).
* Para comandos de grelha de eventos no PowerShell, consulte [PowerShell](/powershell/module/azurerm.eventgrid).