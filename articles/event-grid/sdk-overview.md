---
title: SDKs de grelha de eventos do Azure
description: "Descreve os SDKs de grelha de eventos do Azure. Estes SDKs fornecem gestão, a publicação e consumo."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9c56e4c3314090ad55017d5c681a0cfd7bf5722c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>SDKs de grelha de eventos para gestão e de publicação

Grelha de evento disponibiliza SDKs que lhe permite gerir os recursos e publicar eventos programaticamente.

## <a name="management-sdks"></a>SDKs de gestão

A gestão SDKs permitem-lhe criar, atualizar e eliminar eventos grelha tópicos e subscrições. Atualmente, os SDKs seguintes estão disponíveis:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Aceda](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="publish-sdks"></a>Publicar SDKs

Publicar SDKs permitem-lhe publicar eventos de tópicos, tendo cuidado de autenticação, que formam o evento e publicar no modo assíncrono para o ponto final especificado. Atualmente, os SDKs seguintes estão disponíveis:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Aceda](https://github.com/Azure/azure-sdk-for-go)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [Novidades grelha de evento?](overview.md)
* Para comandos de grelha de evento na CLI do Azure, consulte [CLI do Azure](/cli/azure/eventgrid).
* Para comandos de grelha de eventos no PowerShell, consulte [PowerShell](/powershell/module/azurerm.eventgrid).