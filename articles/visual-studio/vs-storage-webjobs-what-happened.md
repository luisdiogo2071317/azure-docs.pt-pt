---
title: O que aconteceu ao meu projeto do trabalho Web (serviço ligado do armazenamento do Azure do Visual Studio)? | Microsoft Docs
description: Descreve o que aconteceu num projeto de Web do Azure depois de ligar a uma conta de armazenamento com o Visual Studio serviços ligados
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: cf8b7849a603d5c1304846ab243478bb6afd5c18
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059698"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao meu projeto do trabalho Web (serviço ligado do armazenamento do Azure do Visual Studio)?
## <a name="references-added"></a>Referências adicionadas
O pacote NuGet de armazenamento do Azure foi adicionado ou atualizado no seu projeto do Visual Studio.  
Esse pacote adiciona as seguintes referências de .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Cadeia de ligação do armazenamento do Azure adicionado
No ficheiro App. config do seu projeto, o **AzureWebJobsStorage** e **AzureWebJobsDashboard** entradas foram atualizadas com a cadeia de ligação e a chave da conta de armazenamento selecionada.

Para obter mais informações, consulte [recursos de documentação de WebJobs do Azure](http://go.microsoft.com/fwlink/?linkid=390226).

