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
ms.openlocfilehash: 7f4dced6f82622ba735b1b059f30d88830347fba
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625853"
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Configurar o cluster AKS para utilizar os espaços de programador do Azure

Abra uma janela de comandos e introduza o seguinte comando do CLI do Azure, utilizando o grupo de recursos que contém o cluster AKS e o nome do cluster AKS. O comando configura o cluster com suporte para os espaços de programador do Azure.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

