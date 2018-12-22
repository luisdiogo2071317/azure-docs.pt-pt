---
title: 'Início rápido: Reconhecer voz em Python com o SDK do serviço de voz'
titleSuffix: Azure Cognitive Services
description: Aprender a reconhecer a voz em Python com o SDK do serviço de voz
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: chlandsi
ms.openlocfilehash: 7610b12b351b2652df7ade603711d4d92e587292
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53723914"
---
# <a name="quickstart-using-the-speech-service-from-python"></a>Início rápido: Com o serviço de voz do Python

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Este artigo mostra como utilizar o serviço de voz através do SDK de voz para Python. Ela ilustra como reconhecer voz da entrada do microfone.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, eis uma lista de pré-requisitos:

* Uma chave de subscrição do Azure para o serviço de voz. [Obter uma gratuitamente](get-started.md).
* [Python 3.5 (64-bit)](https://www.python.org/downloads/) ou posterior.
* O pacote do Python SDK de voz está disponível para Windows (x64), Mac (versão X do macOS 10.12 ou posterior) e o Linux (Ubuntu 16.04 ou 18.04 em x64).
* No Ubuntu, execute os seguintes comandos para a instalação de pacotes necessários:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

* No Windows, também terá do [Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) para a sua plataforma.

## <a name="get-the-speech-sdk-python-package"></a>Obter o pacote do Python SDK de voz

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

O pacote do Python do SDK de voz de serviços cognitivos pode ser instalado a partir [PyPI](https://pypi.org/) utilizando este comando na linha de comando:

```sh
pip install azure-cognitiveservices-speech
```

A versão atual do SDK de Voz dos Serviços Cognitivos é `1.2.0`.

## <a name="support-and-updates"></a>Atualizações e suporte

Atualizações para o pacote do Python SDK de voz serão distribuídas por meio de PyPI e comunicadas no [notas de versão](./releasenotes.md) página.
Se uma nova versão estiver disponível, pode atualizar para o mesmo com o comando `pip install --upgrade azure-cognitiveservices-speech`.
Pode verificar a versão está atualmente instalada ao inspecionar o `azure.cognitiveservices.speech.__version__` variável.

Se tiver um problema ou estão em falta um recurso, dê uma olhada em nosso [página de suporte](./support.md).

## <a name="create-a-python-application-using-the-speech-sdk"></a>Criar uma aplicação Python utilizando o SDK de voz

### <a name="running-the-sample-in-a-terminal"></a>Execução do exemplo num terminal

Pode copie os [código](#quickstart-code) este início rápido para um ficheiro de origem `quickstart.py` e executá-la no seu IDE ou na consola do

```sh
python quickstart.py
```

ou pode baixar este tutorial de início rápido, como um [Jupyter](https://jupyter.org) bloco de notas do [repositório de exemplos de voz de serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) e executá-lo como um bloco de notas.

### <a name="quickstart-code"></a>Código de início rápido

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="installing-the-speech-sdk-python-package-and-running-the-sample-in-visual-studio-code"></a>Instalar o pacote do Python SDK de voz e executar o exemplo no Visual Studio Code

1. [Transferir](https://www.python.org/downloads/) e instalar uma versão de 64 bits (3.5 ou posterior) do Python no seu computador.
1. [Transferir](https://code.visualstudio.com/Download) e instalar o Visual Studio Code.
1. Abra o Visual Studio Code e instale a extensão do Python, selecionando **arquivo** > **preferências** > **extensões** no menu e Procurar por "Python".
   ![Instalar a extensão de Python](media/sdk/qs-python-vscode-python-extension.png)
1. Crie uma pasta para armazenar o projeto, por exemplo com o Explorador do Windows.
1. No Visual Studio Code, clique nas **ficheiro** ícone e, em seguida, abra a pasta que criou.
   ![Abrir pasta](media/sdk/qs-python-vscode-python-open-folder.png)
1. Criar um novo ficheiro de origem do Python `speechsdk.py`, ao clicar no ícone do novo arquivo.
   ![Criar ficheiro](media/sdk/qs-python-vscode-python-newfile.png)
1. Copiar, colar e guardar a [código de Python](#quickstart-code) para o ficheiro recentemente criado.
1. Insira as informações da sua subscrição do serviço de voz.
1. Se já foi selecionado um interpretador de Python, será apresentado no lado esquerdo da barra de status na parte inferior da janela.
   Caso contrário, pode colocar uma lista de disponíveis interprety de Python, abrindo o **paleta de comandos** (`Ctrl+Shift+P`) e digitando **Python: Selecione o interpretador**e escolha um método adequado.
1. Se o pacote do Python SDK de voz não estiver ainda instalado para o interpretador de Python que selecionou, isso pode ser facilmente feito de dentro do Visual Studio Code.
   Para instalar o pacote do SDK de voz, abra um terminal, novamente a visualização a paleta de comandos (`Ctrl+Shift+P`) e digitando **Terminal: Criar novo Terminal integrado**.
   No terminal que se abre, introduza o comando `python -m pip install azure-cognitiveservices-speech`, ou o comando adequado para o seu sistema.
1. Para executar o código de exemplo, com o botão direito em algum lugar dentro do editor e selecione **execute o ficheiro de Python no Terminal**.
   Uma vez solicitado dizer algumas palavras e texto transcrito deve ser apresentado em breve, posteriormente.
   ![Executar o exemplo](media/sdk/qs-python-vscode-python-run.png)

Se existirem problemas a seguir estas instruções, consulte a mais extensa [tutorial do Python de código do Visual Studio](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Explorar amostras de Python no GitHub](https://aka.ms/csspeech/samples)
