---
title: O que aconteceu ao meu projeto ASP.NET? | Microsoft Docs
description: Descreve o que acontece depois de adicionar o armazenamento do Azure para um projeto ASP.NET com o Visual Studio ligada a serviços
services: storage
author: ghogen
manager: douge
ms.assetid: e1fe1b6d-4e3d-476d-8b2f-f7ade050515e
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 84b608d335dc26198c3af6f89407758fd1d020dc
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>O que aconteceu ao projeto do ASP.NET (o Visual Studio do armazenamento do Azure ligado service)?
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
No ficheiro Web. config do seu projeto, um elemento foi criado com a cadeia de ligação e a chave da conta de armazenamento selecionada.

Para obter mais informações, consulte [ASP.NET](http://www.asp.net).

