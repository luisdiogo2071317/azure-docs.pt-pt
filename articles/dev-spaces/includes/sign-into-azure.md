---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: d8ef59b157dc01c50d96561df31bbca4a8505018
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38728909"
---
### <a name="sign-in-to-azure-cli"></a>Iniciar sessão na CLI do Azure
Inicie sessão no Azure. Escreva o seguinte comando numa janela de terminal:

```cmd
az login
```

> [!Note]
> Se não tiver uma subscrição do Azure, pode [criar uma conta gratuita](https://azure.microsoft.com/free).

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Se tiver várias subscrições do Azure...
Pode ver as suas subscrições ao executar: 

```cmd
az account list
```
Localize a subscrição que tem `isDefault: true` na saída do JSON.
Se esta não for a subscrição que pretende utilizar, pode alterar a subscrição predefinida:

```cmd
az account set --subscription <subscription ID>
```
