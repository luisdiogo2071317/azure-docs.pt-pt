---
title: Depurar c# módulos do Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para desenvolver, criar e depurar um módulo c# para o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/04/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: b1c6209c4d589093d7a29cd8a883d3e5d4ca12f9
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782307"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depurar c# módulos do Azure IoT Edge

Pode transformar sua lógica de negócio em módulos do Azure IoT Edge. Este artigo mostra-lhe como utilizar o Visual Studio Code (código de VS) como a principal ferramenta para desenvolver e depurar os módulos do c#.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que utilize um computador ou máquina virtual com o Windows, macOS ou Linux como o computador de desenvolvimento. Dispositivo IoT Edge pode ser outro dispositivo físico.

> [!NOTE]
> Este artigo depuração demonstra duas maneiras típicas para depurar seu módulo de c# no VS Code. Uma forma é anexar um processo num contêiner de módulo, enquanto a outra é lanuch o código de módulo no modo de depuração. Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging).

Uma vez que este artigo usa código do Visual Studio como a ferramenta de desenvolvimento principal, instale o VS Code. Em seguida, adicione as extensões necessárias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão c#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de .NET para criar a pasta de projeto, o Docker para criar a imagem do módulo e um registo de contentores para armazenar a imagem de módulo:
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) no computador de desenvolvimento. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Pode utilizar um registo do Docker local para o protótipo e fins de testes em vez de um registo de cloud. 

Para configurar o local ambiente de desenvolvimento para depurar, executar e testar a sua solução de IoT Edge, precisa [ferramenta de desenvolvimento do Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) e o Pip](https://www.python.org/). Em seguida, instale **iotedgehubdev** executando o comando no seu terminal abaixo.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Para testar seu módulo num dispositivo, precisa de um hub IoT Active Directory com, pelo menos, um dispositivo do IoT Edge. Para utilizar o seu computador como um dispositivo IoT Edge, siga os passos no guia de introdução para [Windows](quickstart.md) ou [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-with-c-module"></a>Criar uma nova solução com o módulo de c#

Siga estes passos para criar um módulo do IoT Edge com base no .NET Core 2.0 usando o Visual Studio Code e a extensão Azure IoT Edge. Em primeiro lugar é criar uma solução e, em seguida, gerar primeiro módulo nessa solução. Cada solução pode conter mais de um módulo. 

1. No Visual Studio Code, selecione **View** > **Terminal integrado**.
3. Selecione **View** > **paleta de comandos**. 
4. Na paleta de comandos, introduza e execute o comando **do Azure IoT Edge: nova solução do IoT Edge**.

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Procure a pasta onde pretende criar a nova solução. Escolher **selecionar pasta**. 
6. Introduza um nome para a sua solução. 
7. Selecione **módulo de c#** como o modelo para o primeiro módulo na solução.
8. Introduza um nome para seu módulo. Escolha um nome que é exclusivo no seu registo de contentor. 
9. Forneça o nome do repositório de imagens do módulo. Código de VS autopopulates o módulo nome com **localhost:5000**. Substitua-o com as suas próprias informações de registo. Se usar um registo do Docker local para fins de teste, em seguida, **localhost** tudo bem. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **\<nome do registo\>. azurecr.io**. Substitua apenas a parte localhost da cadeia de carateres, não elimine o nome do módulo.

   ![Fornecer repositório de imagens do Docker](./media/how-to-develop-csharp-module/repository.png)

Código VS usa as informações fornecidas, cria uma solução de IoT Edge e, em seguida, carrega-o numa nova janela.

   ![Ver a solução de IoT Edge](./media/how-to-develop-csharp-module/view-solution.png)

Existem quatro itens dentro da solução: 
* R **.vscode** pasta contém as configurações de depuração.
* R **módulos** pasta contém subpastas para cada módulo. Neste momento, tem apenas um. Mas pode adicionar mais na paleta de comandos com o comando **do Azure IoT Edge: Adicionar módulo do IoT Edge**. 
* Uma **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo. 

   >[!NOTE]
   >Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente. 

* R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [Aprenda a usar manifestos de implantação para implementar módulos e rotas](module-composition.md). 

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O padrão módulo código c# que vem com a solução está localizado em **módulos** > * * [nome da sua módulo] * * > **Program.cs**. O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo do c# com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

O suporte do c# no VS Code é otimizado para o desenvolvimento entre plataformas .NET Core. Saiba mais sobre [como trabalhar com c# no VS Code](https://code.visualstudio.com/docs/languages/csharp).

## <a name="launch-and-debug-module-code-without-container"></a>Inicie e depurar o código de módulo sem contentor
O módulo do IoT Edge c# é um .net Core a aplicação. E isso depende do Azure c# SDK de dispositivo IoT. No código do módulo padrão, inicializar uma **ModuleClient** com definições de ambiente e o nome de entrada, que significa que o módulo do IoT Edge c# requer as definições de ambiente iniciar e executar e, também terá de enviar ou encaminhar mensagens para o canais de entrada. O módulo c# predefinido contém apenas um canal de entrada e o nome é **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Simulador de IoT Edge do programa de configuração para a aplicação de módulo único

1. Para configurar e iniciar o simulador, na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: iniciar o IoT Edge Hub simulador de módulo único**. Também tem de especificar os nomes de entrada para a sua aplicação de módulo único, tipo **input1** e prima Enter. O comando irá disparar **iotedgehubdev** CLI e iniciar o simulador do IoT Edge e um contentor de módulo de utilitário teste. Pode ver os resultados abaixo no terminal integrado se o simulator foi iniciado no modo de módulo único com êxito. Também pode ver um `curl` comando para o ajudar a enviar mensagem por meio de. Irá utilizá-lo mais tarde.

   ![Simulador de IoT Edge do programa de configuração para a aplicação de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Pode mover para o Docker Explorer e ver o módulo de estado de execução.

   ![Estado do módulo de simulador](media/how-to-develop-csharp-module/simulator-status.png)

   O **edgeHubDev** contentor é o núcleo do simulador do IoT Edge local. Ele pode ser executado no computador de desenvolvimento sem o daemon de segurança de IoT Edge e fornecer definições de ambiente para a sua aplicação de módulo nativo ou contentores de módulo. O **entrada** contentor exposta restAPIs para ajudar a mensagens de bridge ao canal de entrada em seu módulo de destino.

2. Na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: Definir credenciais de módulo em configurações do usuário** para configurar o módulo de definições de ambiente em `azure-iot-edge.EdgeHubConnectionString` e `azure-iot-edge.EdgeModuleCACertificateFile` nas definições de utilizador. Pode encontrar estas definições de ambiente são referenciadas numa **.vscode** > **Launch** e [definições de utilizador do VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="build-module-app-and-debug-in-launch-mode"></a>Criar a aplicação de módulo e depurar no modo de inicialização

1. No terminal integrado, altere o diretório para **CSharpModule** pasta, execute o seguinte comando para criar o .net Core a aplicação.

    ```cmd
    dotnet build
    ```

   > [!TIP]
   > Também pode utilizar [PostMan](https://www.getpostman.com/) ou outras ferramentas de API para enviar mensagens através de, em vez de `curl`.

2. Navegue para `program.cs`. Adicione um ponto de interrupção nesse arquivo.

3. Navegue para a vista de depuração do VS Code. Selecione a configuração de depuração **ModuleName depurar Local (.NET Core)**. 

4. Clique em **iniciar depuração** ou prima **F5**. Começa a sessão de depuração.

5. No terminal integrado do VS Code, execute o seguinte comando para enviar um **Olá, mundo** mensagem ao módulo. Este é o comando mostrado nos passos anteriores quando configurado com êxito o simulador do IoT Edge.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Se estiver a utilizar o Windows, certificar-se de que o shell do seu terminal integrado do VS Code está **Git Bash** ou **WSL Bash**. Não é possível executar `curl` comando no PowerShell ou a linha de comandos. 
   
   > [!TIP]
   > Também pode utilizar [PostMan](https://www.getpostman.com/) ou outras ferramentas de API para enviar mensagens através de, em vez de `curl`.

6. Na vista de depuração de código do VS, verá as variáveis no painel esquerdo. 

    ![Variáveis de inspeção](media/how-to-develop-csharp-module/single-module-variables.png)

7. Para parar a sessão de depuração, clique no botão Parar ou o press **Shift + F5**. E na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: parar simulador do IoT Edge**.

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Criar contentor de módulo para depuração e de depuração no modo de anexar

A solução predefinida contém dois módulos, um é um módulo de sensor de temperatura simulada e o outro é o módulo de pipe do c#. O sensor de temperatura simulada envie mensagens ao módulo de pipe do c# e, em seguida, as mensagens são direcionadas para o IoT Hub. Na pasta de módulo que criou, existem vários arquivos de Docker para tipos de contentor diferente. Utilizar qualquer um destes ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste. Atualmente, o c# módulos suporte depuração apenas em contentores do Linux amd64 na anexar modo. 

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Simulador de IoT Edge do programa de configuração para a solução de IoT Edge

No computador de desenvolvimento, pode iniciar o simulador do IoT Edge em vez de instalar o daemon de segurança de IoT Edge para executar a sua solução de IoT Edge. 

1. No Explorador de dispositivos no lado esquerdo, faça duplo clique no seu ID de dispositivo do IoT Edge, selecione **configuração do IoT Edge simulador** para iniciar o simulador com a cadeia de ligação do dispositivo.

2. Pode ver que o simulador do IoT Edge tem sido com êxito a configuração no terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Criar e executar o contentor para depuração e de depuração no modo de anexar

1. No VS Code, navegue para o `deployment.template.json` ficheiro. Atualizar o seu URL da imagem do módulo c# adicionando **.debug** ao fim.

   ![Adicionar *** .debug para o seu nome de imagem](./media/how-to-develop-csharp-module/image-debug.png)

2. Navegue para `program.cs`. Adicione um ponto de interrupção nesse arquivo.
3. No Explorador de ficheiros VS Code, selecione o `deployment.template.json` ficheiros para a sua solução, no menu de contexto, clique em **solução de compilação e executar o IoT Edge no simulador**. Pode ver que registos de todos os contentores de módulo na janela da mesma. Também pode navegar para o Docker Explorer para ver o estado do contentor.

   ![Variáveis de inspeção](media/how-to-develop-csharp-module/view-log.png)

5. Navegue para a vista de depuração do VS Code. Selecione o ficheiro de configuração de depuração para seu módulo. O nome da opção de depuração deve ser semelhante a **ModuleName depuração remota (.NET Core)**

   ![Selecione a configuração](media/how-to-develop-csharp-module/debug-config.png)

6. Selecione **iniciar depuração** ou selecione **F5**. Selecione o processo para anexar a.

7. Na vista de depuração de código do VS, verá as variáveis no painel esquerdo.

8. Para parar a sessão de depuração, clique no botão Parar ou o press **Shift + F5**. E na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: parar simulador do IoT Edge**.

> [!NOTE]
> Este exemplo mostra como depurar os módulos do IoT Edge do .NET Core em contentores. Baseia-se a versão de depuração `Dockerfile.debug`, que inclui o depurador de linha de comandos do .NET Core VSDBG em sua imagem de contentor ao criá-lo. Depois de depurar seus módulos do c#, recomendamos que diretamente utilizar ou personalizar `Dockerfile` sem VSDBG de módulos do IoT Edge prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Depois de criar seu módulo, saiba como [implementar módulos do Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
