---
title: Como configurar o Azure Machine Learning Workbench para trabalhar com um IDE?  | Microsoft Docs
description: Um guia para configurar o Azure Machine Learning Workbench para trabalhar com o seu IDE.
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 8158d6faee5ec4d28f0c7e16963fc3d78392b857
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978966"
---
# <a name="how-to-configure-azure-machine-learning-workbench-to-work-with-an-ide"></a>Como configurar o Azure Machine Learning Workbench para trabalhar com um IDE 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

O Azure Machine Learning Workbench pode ser configurado para trabalhar com IDEs de Python populares (ambiente de desenvolvimento integrado). Permite uma experiência de desenvolvimento de ciência de dados uniforme a movimentação entre a preparação de dados, a criação de código, a execução de controlo e a operacionalização. Atualmente, os IDEs suportados são:
- Microsoft Visual Studio Code 
- JetBrain PyCharm 

## <a name="configure-workbench"></a>Configurar a Bancada de trabalho
1. Clique nas **ficheiro** menu no canto superior esquerdo da aplicação. 
2. Selecione o **configurar o projeto IDE** opção a partir da lista de opções 
3. Escreva `VS Code` ou `PyCharm` no **nome** campo (o nome é arbitrário)
4. Introduza a localização para o executável IDE (completo com o nome executável e extensão) na **caminho de execução**

### <a name="default-install-path-for-visual-studio-code"></a>Caminho de instalação predefinido para o Visual Studio Code  

* Windows de 32 bits `C:\Program Files (x86)\Microsoft VS Code\Code.exe`
* Windows de 64 bits `C:\Program Files\Microsoft VS Code\Code.exe`
* macOS – selecione o caminho. App, por exemplo `/Applications/Visual Studio Code.app`, e a aplicação acrescenta o resto do caminho para. O caminho completo para o executável por predefinição é `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code`. Se executou o `Shell Command: Install 'code' command in PATH` no VS Code, de comando, em seguida, também pode referenciar o script do VS Code no `/usr/local/bin/code`

### <a name="default-install-path-for-pycharm"></a>Caminho de instalação predefinido para PyCharm 

* Windows de 32-bit - `C:\Program Files (x86)\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm.exe`. 
* Windows de 64 bits `C:\Program Files\JetBrains\PyCharm Community Edition 2017.2.1\bin\pycharm64.exe`.
* macOS – selecione o caminho. App, por exemplo "/ aplicações/PyCharm CE.app" e a aplicação acrescenta o resto do caminho para. O caminho completo para o executável por predefinição é `/Applications/PyCharm CE.app/Contents/MacOS/pycharm`. Também pode encontrar PyCharm na pasta bin, `/usr/local/bin/charm`

## <a name="open-project-in-ide"></a>Abrir projeto no IDE 
Depois da configuração estiver concluída, pode abrir um projeto do Azure Machine Learning, abrindo o **arquivo** menu no Azure Machine Learning Workbench, em seguida, clique em **Open Project (< IDE_Name >)**. Esta ação abre o projeto ativo atual no IDE configurado. _Nota: Se não estiver num projeto, o **Open Project (< IDE_Name >)** será desativada._

## <a name="configuring-the-integrated-terminal-in-visual-studio-code"></a>Configurar o terminal integrado no Visual Studio Code

### <a name="windows"></a>Windows 
Podemos ter substituído a shell predefinida ser cmd em vez do PowerShell. Clicando em **Open Project (< IDE_Name >)**, verá uma linha de comandos: 

_Permitir o shell: `C:\windows\System32\cmd.exe` (definida como uma definição de área de trabalho) para ser iniciado no terminal?_

Resposta `yes` para permitir a configurar o shell para trabalhar de forma totalmente integrada com interface de linha de comandos do Azure ML Workbench.

### <a name="mac"></a>Mac
Para executar uma `az` integrada de comando com o Visual Studio Code terminal no Mac, terá de definir manualmente a `PATH` para ser o mesmo valor que `PATH` do projeto `.vscode/settings.json` ficheiro, sob a chave `terminal.integrated.env.osx`. Pode fazê-lo ao executar o comando seguinte no terminal: `PATH=<PATH in .vscode/settings>`
