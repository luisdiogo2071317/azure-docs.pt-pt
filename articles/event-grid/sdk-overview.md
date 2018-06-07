---
title: SDKs de grelha de eventos do Azure
description: Descreve os SDKs de grelha de eventos do Azure. Estes SDKs fornecem gestão, a publicação e consumo.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: ebba28e72b3b27729318956bad07b6b6df3f14ac
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823551"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>SDKs de grelha de eventos para gestão e de publicação

Grelha de evento disponibiliza SDKs que lhe permite gerir os recursos e publicar eventos programaticamente.

## <a name="management-sdks"></a>SDKs de gestão

A gestão SDKs permitem-lhe criar, atualizar e eliminar eventos grelha tópicos e subscrições. Atualmente, os SDKs seguintes estão disponíveis:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
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
