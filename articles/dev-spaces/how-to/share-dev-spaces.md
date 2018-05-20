---
title: Como partilhar os espaços de programador do Azure | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desenvolvimento rápido Kubernetes com contentores e micro-serviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes serviço, contentores
manager: douge
ms.openlocfilehash: 140a487a0a605e0f03122868af0d2d12bcdba35b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="share-azure-dev-spaces"></a>Partilhar os espaços de programador do Azure

Com os espaços de programador do Azure, pode partilhar o seu espaço de desenvolvimento com outros utilizadores na sua equipa. Cada programador pode trabalhar no seu próprio espaço sem fear de danificando a outras pessoas. Além disso, trabalhar em conjunto num único espaço pode permitem-lhe testar código ponto-a-ponto sem ter de criar mocks ou simular dependências. Consulte o [Saiba mais sobre o desenvolvimento de equipa](../get-started-nodejs.md#learn-about-team-development) guia para obter mais informações.

Para configurar um ambiente para vários programadores:
1. Crie um espaço do Programador no Azure. Escolha [.NET Core e o código de VS](../get-started-netcore.md), [.NET Core e o Visual Studio](../get-started-netcore-visualstudio.md), ou [Node.js e o VS Code](../get-started-nodejs.md). Terá de ter o proprietário ou contribuinte acesso à subscrição do Azure selecionada.
1. Configure o espaço programador do Azure **grupo de recursos** para [conceder acesso de contribuinte](/azure/active-directory/role-based-access-control-configure) para cada membro do agrupamento. Pode verificar o grupo de recursos de um ambiente ao executar o comando: `azds resource list`
1. Peça a membros do agrupamento para **selecione o ambiente** para desenvolver no mesmo.
     * **Linha de comandos ou VS Code**: para ver Spacess de programador do Azure existente que tem acesso: `azds resource list`. Para selecionar um ambiente: `azds resource select`.
     * **IDE do Visual Studio**: abrir um projeto no Visual Studio, selecione **Azure Dev espaços** de iniciação definições lista pendente. Na caixa de diálogo que se abre, selecione um ambiente de desenvolvimento existente.

![Visual Studio iniciação definições pendente](../media/get-started-netcore-visualstudio/LaunchSettings.png)