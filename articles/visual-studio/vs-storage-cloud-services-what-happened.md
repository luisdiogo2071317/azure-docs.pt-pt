---
title: O que aconteceu ao meu projeto de serviço em nuvem? | Microsoft Docs
description: Descreve o que acontece num projeto de serviços cloud, depois de ligar a uma conta de armazenamento do Azure com o Visual Studio serviços ligados
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e823bc16c500dfee44312a774d5e3bd6622e5fae
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057365"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao meu projeto de serviços cloud (serviço ligado do armazenamento do Azure do Visual Studio)?
## <a name="references-added"></a>Referências adicionadas
O pacote NuGet de armazenamento do Azure foi adicionado ao seu projeto do Visual Studio.  
Esse pacote adiciona as seguintes referências de .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de ligação do armazenamento do Azure adicionado
Elementos foram criados com a cadeia de ligação e a chave da conta de armazenamento selecionada. Foram efetuadas modificações para os seguintes ficheiros:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

