---
title: Depurar os módulos de função do Azure para o Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para depurar funções do Azure c# com o Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bf2d1af66cc3ecc35dafe3bcd43bf10399d71641
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346720"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depurar funções do Azure para o Azure IoT Edge

Este artigo mostra-lhe como utilizar [Visual Studio Code (código de VS)](https://code.visualstudio.com/) para depurar as suas funções do Azure no Azure IoT Edge.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que utilize um computador ou máquina virtual a executar o Windows ou Linux da máquina de desenvolvimento. Dispositivo IoT Edge pode ser outro dispositivo físico. Ou pode simular o seu dispositivo IoT Edge no computador de desenvolvimento.

> [!NOTE]
> Este artigo depuração demonstra como anexar um processo num contêiner de módulo e depurá-lo com o VS Code. Só é possível depurar funções c# nos contentores do Linux amd64. Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging). 

Este artigo utiliza o Visual Studio Code como a ferramenta de desenvolvimento principal. Instale o VS Code. Em seguida, adicione as extensões necessárias: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão c#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de .NET para criar a pasta de projeto, o Docker para criar a imagem do módulo e um registo de contentores para armazenar a imagem de módulo:
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download)
* [Docker Community Edition](https://docs.docker.com/install/) no computador de desenvolvimento 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Pode utilizar um registo do Docker local para o protótipo e fins de testes em vez de um registo de cloud. 

Para testar seu módulo num dispositivo, precisa de um hub IoT Active Directory com, pelo menos, um dispositivo do IoT Edge. Para utilizar o seu computador como um dispositivo IoT Edge, siga os passos no guia de introdução para [Windows](quickstart.md) ou [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Siga estes passos para criar uma solução de IoT Edge que tenha o módulo de função um c#. Cada solução pode ter vários módulos.

1. No Visual Studio Code, selecione **View** > **Terminal integrado**.
3. Selecione **View** > **paleta de comandos**.
4. Na paleta de comandos, introduza e execute o comando **do Azure IoT Edge: nova solução do IoT Edge**. 

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Procure a pasta onde pretende criar a nova solução. Escolher **selecionar pasta**. 
6. Introduza um nome para a sua solução. 
7. Escolher **as funções do Azure - c#** como o modelo para o primeiro módulo na solução.
8. Introduza um nome para seu módulo. Escolha um nome que é exclusivo no seu registo de contentor. 
9. Indique o repositório de imagens para o módulo. Código de VS autopopulates o módulo nome com **localhost:5000**. Substitua-o com as suas próprias informações de registo. Se usar um registo do Docker local para fins de teste, em seguida, **localhost** tudo bem. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **\<nome do registo\>. azurecr.io**.

Código VS usa as informações fornecidas, cria uma solução de IoT Edge com um projeto de funções do Azure e, em seguida, carrega-o numa nova janela.

Existem quatro itens dentro da solução: 

* R **.vscode** pasta contém as configurações de depuração.
* R **módulos** pasta contém subpastas para cada módulo. Neste momento, tem apenas um. Mas pode adicionar mais por meio da paleta de comandos com o comando **do Azure IoT Edge: Adicionar módulo do IoT Edge**.
* Uma **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo. 

   >[!NOTE]
   >Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente. 

* R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [Aprenda a usar manifestos de implantação para implementar módulos e rotas](module-composition.md).

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O código de função do Azure de predefinição que vem com a solução está localizado em **módulos** > **\<seu nome de módulo\>**   >   **EdgeHubTrigger-Csharp** > **csx**. O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo de função do Azure com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

## <a name="build-your-module-for-debugging"></a>Criar o módulo para depuração
1. Para iniciar a depuração, utilize **Dockerfile.amd64.debug** para reconstruir a imagem do docker e implemente a sua solução de Edge novamente. No Explorador do VS Code, navegue para o `deployment.template.json` ficheiro. Atualize o seu URL de imagem de função ao adicionar `.debug` ao fim.

    ![Criar imagem de depuração](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Reconstrua a sua solução. Na paleta de comandos VS Code, introduza e execute o comando **do Azure IoT Edge: solução de IoT Edge criar**.
3. No Explorador de dispositivos de Hub IoT do Azure, um ID de dispositivo do IoT Edge com o botão direito e, em seguida, selecione **criar implementação para o dispositivo de limite**. Selecione o `deployment.json` de ficheiros a `config` pasta. Verá a implementação foi criada com êxito com um ID de implementação num terminal integrado do VS Code.

Verificar o estado do contentor no Explorador de Docker de código do VS ou ao executar o `docker images` comando no terminal.

## <a name="start-debugging-c-functions-in-vs-code"></a>Iniciar a depuração de funções c# no VS Code
1. Mantém depuração de código de VS informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isso `launch.json` arquivo foi gerado quando criou uma nova solução de IoT Edge. Ele atualiza cada vez que adiciona um novo módulo que oferece suporte à depuração. Navegue para a vista de depuração. Selecione o ficheiro de configuração de depuração correspondente. O nome de opção de depuração deve ser semelhante à **ModuleName depuração remota (.NET Core)**.

   ![Configuração de depuração Select](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.
3. Selecione **iniciar depuração** ou selecione **F5**. Selecione o processo para anexar a.
4. Na vista de depuração do VS Code, verá as variáveis no painel esquerdo. 


> [!NOTE]
> Este exemplo mostra como depurar .net funções de núcleo IoT Edge em contentores. Baseia-se a versão de depuração a `Dockerfile.amd64.debug`, que inclui o depurador de linha de comandos do .NET Core VSDBG em sua imagem de contentor ao criá-lo. Recomendamos que diretamente utilizar ou personalizar o `Dockerfile` sem VSDBG para prontos para produção do IoT Edge funções depois que Depure suas funções c#.

## <a name="next-steps"></a>Passos Seguintes

Depois de criar seu módulo, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
