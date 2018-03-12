---
title: Utilizar a CLI do Azure no Windows | Microsoft Docs
description: Utilizar a CLI do Azure no Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: e54ea62b7b7a7adb4874866a540ed9a6a097fc42
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-azure-cli-on-windows"></a>Utilizar a CLI do Azure no Windows

A Interface de linha de comandos do Azure (CLI) fornece uma linha de comandos e o ambiente de script para criar e gerir recursos do Azure. A CLI do Azure está disponível para macOS, Linux e sistemas operativos Windows. Entre estes sistemas operativos, os comandos da CLI são idênticos, no entanto, a sintaxe de scripting específicos de sistema operativo pode ser diferente.

Este documento fornece detalhes sobre as formas em que a CLI do Azure pode ser instalada e executada no Windows detalhes syntactical as considerações de e para cada. Para aprofundada CLI do Azure documentação, consulte [documentação da CLI do Azure]( https://docs.microsoft.com/cli/azure).

## <a name="windows-subsystem-for-linux"></a>Subsistema Windows para Linux

O subsistema Windows para Linux (WSL) fornece um ambiente de Ubuntu Linux aniversário do Windows 10 e edições posteriores. Quando ativada, WSL fornece uma experiência de Bash nativa, o que pode ser utilizada para criar e executar scripts da CLI do Azure. Porque WSL fornece uma experiência de Bash nativa, os scripts da CLI do Azure podem ser partilhados entre macOS, Linux e Windows sem modificação.

Para utilizar a CLI do Azure no WSL, efetue o seguinte.

|Tarefa | Instruções |
|---|---|
| Ativar WSL | [Instalar documentação WSL ](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
| Instalar a CLI do Azure |[Instalar a CLI no WSL/Ubuntu 14.04](https://docs.microsoft.com/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

A CLI do Azure pode ser executada nativamente no Windows. Nesta configuração, o pacote do CLI do Azure está instalado no sistema operativo Windows e comandos podem ser executados a partir do PowerShell. Nesta configuração, scripts e comandos da CLI do Azure podem ser executados em qualquer versão suportada do Windows, no entanto, não é necessária sintaxe de scripting específicas de plataforma. Por este motivo, scripts não não necessariamente ser partilhados entre macOS, Linux e Windows sem modificação.

Para utilizar a CLI do Azure no Windows, instale o pacote a utilizar estas instruções, [instalar a CLI no Windows](https://docs.microsoft.com/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Imagem de docker

Ao utilizar o Docker para Windows, pode ser iniciada uma imagem de Docker que inclui a CLI do Azure. Esta imagem baseia-se remotas Linux e inclui uma experiência nativa do Bash.  Ao utilizar o Docker para Windows e a imagem da CLI do Azure, scripts a serem partilhadas entre macOS, Linux e Windows. 

Para utilizar a CLI do Azure no Docker para Windows, certifique-se de que o Docker para Windows está em execução e execute o seguinte comando.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Depois de concluído, um Bash será de início de sessão que é pré-carregado com as ferramentas da CLI do Azure.

## <a name="next-steps"></a>Próximos Passos

[Exemplo CLI para máquinas virtuais do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Exemplos da CLI para Web Apps do Azure](../../app-service/app-service-cli-samples.md)

[Exemplos da CLI para SQL do Azure](../../sql-database/sql-database-cli-samples.md)
