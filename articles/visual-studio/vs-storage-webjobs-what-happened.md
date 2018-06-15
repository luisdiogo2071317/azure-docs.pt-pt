---
title: O que aconteceu ao meu projeto do trabalho Web (serviço ligado da Visual Studio do armazenamento do Azure)? | Microsoft Docs
description: Descreve o que aconteceu num projeto trabalho Web do Azure após a ligação a uma conta de armazenamento com o Visual Studio ligação de serviços
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 056ccd572a04a436ff53dda6ae967711c9f9903d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790803"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao meu projeto do trabalho Web (serviço ligado da Visual Studio do armazenamento do Azure)?
## <a name="references-added"></a>Referências adicionadas
O pacote NuGet do armazenamento do Azure foi adicionado ou atualizado no seu projeto de Visual Studio.  
Este pacote adiciona as seguintes referências de .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de ligação para o armazenamento de Azure adicionado
No ficheiro App. config do seu projeto, a **AzureWebJobsStorage** e **AzureWebJobsDashboard** entradas foram atualizadas com a cadeia de ligação e a chave da conta de armazenamento selecionada.

Para obter mais informações, consulte [recursos de documentação de WebJobs do Azure](http://go.microsoft.com/fwlink/?linkid=390226).

