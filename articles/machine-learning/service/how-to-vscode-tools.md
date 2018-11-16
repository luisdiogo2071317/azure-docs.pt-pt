---
title: Usando o Visual Studio Code Tools para a extensão de IA com o Azure Machine Learning
description: Saiba mais sobre o Visual Studio Code Tools para IA e como começar a formação e implementar modelos com o serviço Azure Machine Learning no VS Code de aprendizagem profunda e de aprendizagem automática.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: a298a49c5bdf28b30da05c1b6296fc211ec14c1d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713151"
---
# <a name="vs-code-tools-for-ai-get-started-with-azure-machine-learning-from-visual-studio-code"></a>VS Code Tools para IA: introdução ao Azure Machine Learning a partir do Visual Studio Code

Neste artigo, irá aprender sobre a extensão do Visual Studio Code (código de VS), **Tools para IA**e como começar a formação e implementar modelos com o serviço Azure Machine Learning no VS Code de aprendizagem profunda e de aprendizagem automática.

Utilize as ferramentas para a extensão de IA no Visual Studio code para utilizar o serviço do Azure Machine Learning para preparar seus dados, formação e teste ML modelos no local e remoto destinos de computação, implementar esses modelos e controlar métricas personalizadas e experimentações.

## <a name="prerequisite"></a>Pré-requisito

+ Deve ser instalado o Visual Studio Code. VS Code é um editor de código fonte simples, poderoso, mas que é executado no seu ambiente de trabalho. Ele vem com suporte incorporado para o Python e muito mais.  [Saiba como instalar o VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Instalar o Python 3.5 ou superior](https://www.anaconda.com/download/).

+ Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

## <a name="install-vs-code-tools-for-ai-extension"></a>Instalar as ferramentas de código do VS para a extensão de IA

Ao instalar o **Tools para IA** extensão, duas extensões mais são instaladas automaticamente (se tiver acesso à internet). Eles estão o [conta do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensão e o [Python do Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-python.python) extensão

Para trabalhar com o Azure Machine Learning, é necessário tornar o código VS num IDE de Python. Trabalhar com [Python no Visual Studio Code](https://code.visualstudio.com/docs/languages/python), requer a extensão de Python do Microsoft, que é instalada com as ferramentas automaticamente para IA. A extensão faz um excelente IDE com código VS e funciona em qualquer sistema operativo com uma variedade de interprety de Python. Tira partido de tudo de energia do VS Code para fornecer o preenchimento automático e IntelliSense, linting, depuração e teste de unidade, juntamente com a capacidade de alternar facilmente entre ambientes do Python, incluindo virtual e os ambientes de conda. Confira estas noções básicas de editar, executar e depuração de código de Python, veja o [Python Hello World Tutorial](https://code.visualstudio.com/docs/python/python-tutorial)

**Para instalar as ferramentas para a extensão de IA:**

1. Inicie o VS Code.

1. Num browser, visite: https://aka.ms/vscodetoolsforai. 

1. Na página da web, clique em **instalar**. 

1. No separador de extensão, clique em **instalar**.

1. Bem-vindo é aberto um separador no VS Code para a extensão e o símbolo do Azure é adicionado à barra de atividade.

   ![Ícone do Azure na barra de atividade do Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Na caixa de diálogo, clique em **sessão** e siga o pedido na tela para autenticar com o Azure. 
   
   A extensão da conta do Azure, que foi instalada, juntamente com o VS Code Tools para IA, ajuda-o a autenticar com a sua conta do Azure. Ver a lista de comandos do [extensão da conta do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) página.

> [!Tip] 
> Veja a [IntelliCode extensão para o VS Code (pré-visualização)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode fornece um conjunto de capacidades de IA auxiliado para IntelliSense em Python, tais como de inferir os auto-conclusões mais relevantes com base no contexto de código atual.

## <a name="install-the-sdk"></a>Instalar o SDK

1. Certifique-se de que o Python 3.5 ou superior está instalado e reconhecido pelo código VS. Se instalar a aplicação agora, em seguida, reinicie o VS Code e selecione um interpretador de Python com as instruções apresentadas em https://code.visualstudio.com/docs/python/python-tutorial.

1. No VS Code, abra a paleta de comandos **Ctrl + Shift + P**.

1. Tipo "instalar o SDK do Azure ML" para localizar o pip o comando de instalação para o SDK. É criado um ambiente de Python privado local que tem os pré-requisitos do Visual Studio Code para trabalhar com o Azure Machine Learning.
   ![Instalar](./media/vscode-tools-for-ai/install-sdk.png)

1. Na janela de terminal integrada, especifique o interpretador de Python a utilizar ou pode usar **Enter** para utilizar o interpretador de Python padrão.

   ![Instalar](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Introdução ao Azure Machine Learning

Antes de começar a formação e implementação de modelos de ML utilizar o VS Code, tem de criar uma [área de trabalho do Azure Machine Learning serviço](concept-azure-machine-learning-architecture.md#workspace) na cloud para conter os seus modelos e recursos. Saiba como criar uma e criar a sua primeira experiência nessa área de trabalho.

1. Clique no ícone do Azure na barra de atividade de código do Visual Studio. O Azure: A barra lateral do Machine Learning aparece.

   ![Instalar](./media/vscode-tools-for-ai/createworkspace.gif)

1. A subscrição do Azure com o botão direito e selecione **criar área de trabalho**. É apresentada uma lista. Na imagem animada, o nome da subscrição é 'OpenMind Studio' e a área de trabalho é 'MyWorkspace'. 

1. Selecione um grupo de recursos existente na lista ou crie um novo utilizando o assistente na paleta de comandos.

1. No campo, escreva um nome exclusivo e claro para sua nova área de trabalho. Nas capturas de ecrã, a área de trabalho com o nome 'MyWorkspace'.

1. Prima enter e a nova área de trabalho é criada. Ele aparece na árvore abaixo do nome de subscrição.

1. O nome de área de trabalho com o botão direito e escolher **experimentação criar** no menu de contexto.  Experimentações manter o controle de suas execuções com o Azure Machine Learning.

1. No campo, introduza um nome de sua experiência. Nas capturas de ecrã, a experimentação com o nome 'MNIST'.
 
1. Prima enter e é criada a nova experimentação. Ele aparece na árvore abaixo do nome de área de trabalho.

1. O nome de experimentação com o botão direito e escolher **anexar uma pasta local**. Esta pasta deve conter os seus scripts de Python locais. A pasta, em seguida, é vinculada à experimentação na cloud. 

   Agora cada da sua experimentação é executado com a sua experimentação, todas as métricas chave serão armazenadas no histórico de experimentação e os modelos de que preparar, irão obter automaticamente carregados para o Azure Machine Learning e armazenados com a sua experimentação métricas e registos.

   [![Anexar uma pasta no VS Code](./media/vscode-tools-for-ai/attachfolder.gif)](./media/vscode-tools-for-ai/attachfolder.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Utilizar atalhos de teclado

Como a maioria do VS Code, os recursos do Azure Machine Learning no VS Code são acessíveis a partir do teclado. A combinação de teclas mais importante saber é Ctrl + Shift + P, que abrirá a paleta de comandos. A partir daqui, tem acesso a todas as funcionalidades do VS Code, incluindo atalhos de teclado para as operações mais comuns.

[![Atalhos de teclado do VS Code Tools para IA](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Passos Seguintes

Agora, pode utilizar o Visual Studio Code para trabalhar com o Azure Machine Learning.

Saiba como [criar destinos de computação, preparar e implementar modelos no Visual Studio Code](how-to-vscode-train-deploy.md).