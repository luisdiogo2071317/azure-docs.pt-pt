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
ms.openlocfilehash: e4bde4f521e0e19e7acd36260c98cfe80973e284
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
## <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Inicializar ativos de depuração com a extensão de VS Code
Terá primeiro de configurar o seu projeto de código, para que o VS Code irão comunicar com o nosso ambiente de desenvolvimento no Azure. A extensão de VS Code para espaços de programador do Azure fornece um comando do programa auxiliar para definir a configuração de depuração. 

Abra o **paleta de comando** (utilizando o **vista | Comando paleta** menu) e a conclusão automática para escrever e selecionar este comando de utilização: `Azure Dev Spaces: Create configuration files for connected development`. 

Esta ação adiciona a configuração de depuração para Azure Dev espaços sob o `.vscode` pasta.

![](../media/common/command-palette.png)

> [!Important]
> Resultam de erros, feche e volte a abrir o VS Code antes de continuar.