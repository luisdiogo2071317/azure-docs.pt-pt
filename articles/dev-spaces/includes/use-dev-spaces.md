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
ms.openlocfilehash: 44ce986fcfdf079d3077c007a378f3467073d00d
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823185"
---
### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Configurar o cluster AKS para utilizar os espaços de programador do Azure

Abra uma janela de comandos e introduza o seguinte comando do CLI do Azure, utilizando o grupo de recursos que contém o cluster AKS e o nome do cluster AKS. O comando configura o cluster com suporte para os espaços de programador do Azure.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

