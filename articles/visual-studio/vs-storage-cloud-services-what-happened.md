---
title: O que aconteceu ao meu projeto de serviço em nuvem? | Microsoft Docs
description: Descreve o que acontece num projeto de serviços em nuvem depois de ligar a uma conta de armazenamento do Azure com o Visual Studio ligada a serviços
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e88e41edbd062f89e24915889f5b74660ec45513
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao meu projeto de serviços em nuvem (Visual Studio do armazenamento do Azure ligado service)?
## <a name="references-added"></a>Referências adicionadas
O pacote NuGet do armazenamento do Azure foi adicionado ao seu projeto de Visual Studio.  
Este pacote adiciona as seguintes referências de .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de ligação para o armazenamento de Azure adicionado
Elementos foram criados com a cadeia de ligação e a chave da conta de armazenamento selecionada. Foram efetuadas modificações para os seguintes ficheiros:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

