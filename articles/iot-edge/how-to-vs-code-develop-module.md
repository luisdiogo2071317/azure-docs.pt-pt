---
title: Desenvolver e depurar os módulos do Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para desenvolver, criar e depurar um módulo para utilizar o Azure IoT Edge C#, Python, node. js, Java ou C
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fb3d19d35a15d5476594948b035a39ae703f1c3a
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53551588"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depurar os módulos do Azure IoT Edge

Pode transformar sua lógica de negócio em módulos do Azure IoT Edge. Este artigo mostra-lhe como utilizar o Visual Studio Code como a principal ferramenta para desenvolver e depurar módulos.

## <a name="prerequisites"></a>Pré-requisitos

Pode utilizar um computador ou numa máquina virtual com o Windows, macOS ou Linux da máquina de desenvolvimento. Um dispositivo IoT Edge pode ser outro dispositivo físico.

Para módulos de escrita em C#, node. js ou Java, existem duas formas para depurar seu módulo no Visual Studio Code: Pode anexar um processo num contêiner de módulo ou iniciar o código de módulo no modo de depuração. Para módulos de escrita em Python ou C, eles só podem ser depurados ao anexação a processo nos contentores do Linux amd64.

> [!TIP]
> Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging).

Instale [Visual Studio Code](https://code.visualstudio.com/) primeiro e, em seguida, adicione as seguintes extensões:

- [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Visual Studio extensão ou extensões específicas para o idioma que está desenvolvendo em:
  - C#, incluindo as funções do Azure: [Extensão c#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Extensão de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Pacote de extensão do Java para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [Extensão de C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Também precisará instalar algumas ferramentas adicionais, específicos de linguagem para desenvolver seu módulo:

- C#, incluindo as funções do Azure: [SDK 2.1 do .NET Core](https://www.microsoft.com/net/download)

- Python: [Python](https://www.python.org/downloads/) e [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar pacotes de Python (normalmente, incluídos com a instalação do Python). Após a instalação do Pip, instalar o **Cookiecutteru** pacote com o seguinte comando:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- NODE. js: [Node.js](https://nodejs.org). Também vai querer instalar [Yeoman](https://www.npmjs.com/package/yo) e o [gerador de módulo de node. js do Azure IoT Edge](https://www.npmjs.com/package/generator-azure-iot-edge-module).

- Java: [Kit de desenvolvimento Sudeste do Java 10](https://aka.ms/azure-jdks) e [Maven](https://maven.apache.org/). Precisará [defina o `JAVA_HOME` variável de ambiente](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) para apontar para a instalação do JDK.

Para criar e implementar a imagem do módulo, precisa Docker para criar a imagem de módulo e um registo de contentores para armazenar a imagem de módulo:

- [Docker Community Edition](https://docs.docker.com/install/) no computador de desenvolvimento.

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > Pode utilizar um registo do Docker local para o protótipo e fins de testes em vez de um registo de cloud.

A menos que esteja a desenvolver o seu módulo em C, terá também de baseados em Python [ferramenta de desenvolvimento do Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/) para configurar o ambiente de desenvolvimento local para depurar, executar e testar a sua solução de IoT Edge. Se ainda não o fez, instale [Python (2.7/3.6) e o Pip](https://www.python.org/) e, em seguida, instale **iotedgehubdev** ao executar este comando no seu terminal.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Para testar seu módulo num dispositivo, terá um hub IoT Active Directory com, pelo menos, um dispositivo do IoT Edge. Para utilizar o seu computador como um dispositivo IoT Edge, siga os passos no guia de introdução para [Linux](quickstart-linux.md) ou [Windows](quickstart.md). Se estiver a executar o daemon de IoT Edge no computador de desenvolvimento, poderá ter de parar EdgeHub e EdgeAgent antes de prosseguir para o passo seguinte.

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Os passos seguintes mostram como criar um módulo do IoT Edge no seu idioma preferencial de desenvolvimento (incluindo funções do Azure, escritas em C#) com o Visual Studio Code e a extensão Azure IoT Edge. Comece por criar uma solução e, em seguida, gerar primeiro módulo nessa solução. Cada solução pode conter vários módulos.

1. Selecione **View** > **paleta de comandos**.

1. Na paleta de comandos, introduza e execute o comando **Azure IoT Edge: Nova solução de IoT Edge**.

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

1. Navegue até à pasta onde pretende criar a nova solução e, em seguida, selecione **selecionar pasta**.

1. Introduza um nome para a sua solução.

1. Selecione um modelo de módulo da sua linguagem de desenvolvimento preferido ser o primeiro módulo na solução.

1. Introduza um nome para seu módulo. Escolha um nome que é exclusivo no seu registo de contentor.

1. Forneça o nome do repositório de imagens do módulo. Visual Studio Code autopopulates o módulo nome com **localhost:5000 / < nome do seu módulo\>**. Substitua-o com as suas próprias informações de registo. Se usar um registo do Docker local para fins de teste, em seguida, **localhost** tudo bem. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão se parece com ***\<nome do registo\>*. azurecr.io**. Apenas substituir a **localhost:5000** parte da cadeia de caracteres para que o resultado final se parece com **\<* nome do registo*\>.azurecr.io/* \<seu nome de módulo\>***.

   ![Fornecer repositório de imagens do Docker](./media/how-to-develop-csharp-module/repository.png)

Código do Visual Studio usa as informações fornecidas, cria uma solução de IoT Edge e, em seguida, carrega-o numa nova janela.

Existem quatro itens dentro da solução:

- R **.vscode** pasta contém as configurações de depuração.

- R **módulos** pasta contém subpastas para cada módulo. Neste momento, tem apenas um. Mas pode adicionar mais na paleta de comandos com o comando **Azure IoT Edge: Adicionar módulo do IoT Edge**.

- Uma **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo.

  > [!NOTE]
  > Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente.

- R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [Aprenda a usar manifestos de implantação para implementar módulos e rotas](module-composition.md).

## <a name="add-additional-modules"></a>Adicionar módulos adicionais

Para adicionar módulos adicionais à sua solução, execute o comando **Azure IoT Edge: Adicionar módulo do IoT Edge** da paleta de comandos. Pode também com o botão direito a **módulos** pasta ou o `deployment.template.json` do ficheiro na vista do Explorador de código do Visual Studio e, em seguida, selecione **Adicionar módulo do IoT Edge**.

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O código de módulo de predefinição que vem com a solução está localizado na seguinte localização:

- Função do Azure (C#): **módulos >  *&lt;seu nome de módulo&gt;* > *&lt;seu nome de módulo&gt;*. CS**
- C#: **módulos > *&lt;seu nome de módulo&gt;* > Program.cs**
- Python: **módulos > *&lt;seu nome de módulo&gt;* > main.py**
- NODE. js: **módulos > *&lt;seu nome de módulo&gt;* > App. js**
- Java: **módulos > *&lt;seu nome de módulo&gt;* > src > principal > java > com > edgemodulemodules > App**
- C: **módulos > *&lt;seu nome de módulo&gt;* > main**

O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub.

Quando estiver pronto para personalizar o modelo com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Depurar um módulo sem um contentor (C#, node. js, Java)

Se estiver desenvolvendo em C#, node. js ou Java, seu módulo requer a utilização de um **ModuleClient** objeto no código do módulo de padrão para que ele pode iniciar, executar e rotear as mensagens. Também vai utilizar o canal de entrada padrão **input1** para agir quando o módulo recebe mensagens.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Configurar o simulador do IoT Edge para a solução de IoT Edge

No computador de desenvolvimento, pode iniciar um simulador de IoT Edge em vez de instalar o daemon de segurança de IoT Edge, para que possam executar sua solução de IoT Edge.

1. No Explorador de dispositivos no lado esquerdo, faça duplo clique no seu ID de dispositivo do IoT Edge e, em seguida, selecione **configuração do IoT Edge simulador** para iniciar o simulador com a cadeia de ligação do dispositivo.
1. Pode ver que o simulador do IoT Edge tem foi configurado com êxito ao ler os detalhes de progresso no terminal integrado.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>Configurar o simulador do IoT Edge para a aplicação de módulo único

Para configurar e iniciar o simulador, execute o comando **Azure IoT Edge: Iniciar o simulador de Hub do IoT Edge para o mesmo módulo** da paleta de comandos do Visual Studio Code. Quando lhe for pedido, utilize o valor **input1** do código do módulo de padrão (ou o valor equivalente a partir do código) como o nome de entrada para a sua aplicação. Os acionadores de comando a **iotedgehubdev** CLI e, em seguida, inicia o simulador do IoT Edge e um contentor de módulo de utilitário teste. Pode ver os resultados abaixo no terminal integrado se o simulator foi iniciado no modo de módulo único com êxito. Também pode ver um `curl` comando para o ajudar a enviar mensagem por meio de. Irá utilizá-lo mais tarde.

   ![Configurar o simulador do IoT Edge para a aplicação de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Pode usar a exibição do Gerenciador de Docker no Visual Studio Code para ver o estado de execução do módulo.

   ![Estado do módulo de simulador](media/how-to-develop-csharp-module/simulator-status.png)

   O **edgeHubDev** contentor é o núcleo do simulador do IoT Edge local. Ele pode ser executado no computador de desenvolvimento sem o daemon de segurança de IoT Edge e fornece definições de ambiente para a sua aplicação de módulo nativo ou contentores de módulo. O **entrada** contentor expõe as APIs REST para ajudar a mensagens de ponte para o canal de entrada de destino no seu módulo.

### <a name="debug-module-in-launch-mode"></a>Depurar o módulo no modo de inicialização

1. Preparar o ambiente para depuração, de acordo com os requisitos da sua linguagem de desenvolvimento, defina um ponto de interrupção em seu módulo e selecione a configuração de depuração para utilizar:
   - **C#**
     - No terminal integrado do Visual Studio Code, altere o diretório para o ***&lt;seu nome de módulo&gt;*** pasta e execute o seguinte comando para criar o .net Core a aplicação.

       ```cmd
       dotnet build
       ```

     - Abra o ficheiro `program.cs` e adicione um ponto de interrupção.

     - Navegue para a vista de depuração e código do Visual Studio selecionando **vista > depurar**. Selecione a configuração de depuração  ***&lt;seu nome de módulo&gt;* (.NET Core) de depurar Local** na lista pendente.

        > [!NOTE]
        > Se o .net Core `TargetFramework` não é consistente com o seu caminho de programa na `launch.json`, terá de atualizar manualmente o caminho de programa na `launch.json` de acordo com o `TargetFramework` em seu arquivo. csproj para que esse Visual Studio Code pode iniciar isso programa.

   - **Node.js**
     - No terminal integrado do Visual Studio Code, altere o diretório para o ***&lt;seu nome de módulo&gt;*** pasta e execute o seguinte comando para instalar pacotes de nó

       ```cmd
       npm install
       ```

     - Abra o ficheiro `app.js` e adicione um ponto de interrupção.

     - Navegue para a vista de depuração e código do Visual Studio selecionando **vista > depurar**. Selecione a configuração de depuração  ***&lt;seu nome de módulo&gt;* (node. js) de depurar Local** na lista pendente.
   - **Java**
     - Abra o ficheiro `App.java` e adicione um ponto de interrupção.

     - Navegue para a vista de depuração e código do Visual Studio selecionando **vista > depurar**. Selecione a configuração de depuração  ***&lt;seu nome de módulo&gt;* (Java) de depurar Local** na lista pendente.

1. Clique em **iniciar depuração** ou prima **F5** para iniciar a sessão de depuração.

1. No terminal integrado do Visual Studio Code, execute o seguinte comando para enviar um **Olá, mundo** mensagem ao módulo. Este é o comando mostrado nos passos anteriores, quando configurou o simulador do IoT Edge.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Se estiver a utilizar o Windows, certificar-se de que o shell do seu terminal integrado do Visual Studio Code é **Git Bash** ou **WSL Bash**. Não é possível executar o `curl` comando a partir de um PowerShell ou a linha de comandos.
   > [!TIP]
   > Também pode utilizar [PostMan](https://www.getpostman.com/) ou outras ferramentas de API para enviar mensagens através de, em vez de `curl`.

1. Na vista de depuração e código do Visual Studio, verá as variáveis no painel esquerdo.

1. Para parar o sessão de depuração, selecione o botão Parar ou prima **Shift + F5**e, em seguida, execute **Azure IoT Edge: Parar o simulador do IoT Edge** na paleta de comandos para parar o simulador e limpar.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>Depuração no modo de anexar com o simulador do IoT Edge (C#, as funções do node. js, Java, do Azure)

A solução predefinida contém dois módulos, um é um módulo de sensor de temperatura simulada e o outro é o módulo de pipe. O sensor de temperatura simulada envia mensagens para o módulo de pipe e, em seguida, as mensagens são direcionadas para o IoT Hub. Na pasta de módulo que criou, existem vários arquivos de Docker para tipos de contentor diferente. Utilizar qualquer um dos ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste.

Atualmente, anexar a depuração no modo é suportado apenas da seguinte forma:

- C#módulos, incluindo aqueles para as funções do Azure, suportam a depuração em contentores do Linux amd64
- Depuração no amd64 de Linux e contentores de arm32v7 e contentores do Windows amd64 de suporte de módulos node. js
- Módulos de Java suportam a depuração em contentores de Linux amd64 e arm32v7

> [!TIP]
> Pode alternar entre opções para a plataforma padrão para a sua solução de IoT Edge ao clicar no item na barra de status de Visual Studio Code.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>Configurar o simulador do IoT Edge para a solução de IoT Edge

No computador de desenvolvimento, pode iniciar um simulador de IoT Edge em vez de instalar o daemon de segurança de IoT Edge, para que possam executar sua solução de IoT Edge.

1. No Explorador de dispositivos no lado esquerdo, faça duplo clique no seu ID de dispositivo do IoT Edge e, em seguida, selecione **configuração do IoT Edge simulador** para iniciar o simulador com a cadeia de ligação do dispositivo.

1. Pode ver que o simulador do IoT Edge tem foi configurado com êxito ao ler os detalhes de progresso no terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Criar e executar o contentor para depuração e de depuração no modo de anexar

1. A abrir o ficheiro do módulo (`program.cs`, `app.js`, `App.java`, ou `<your module name>.cs`) e adicione um ponto de interrupção.

1. Na vista de Explorador de código do Visual Studio, clique com botão direito a `deployment.debug.template.json` de ficheiros para a sua solução e, em seguida, selecione **solução de compilação e executar o IoT Edge no simulador**. Pode ver que registos de todos os contentores de módulo na janela da mesma. Também pode navegar para a vista de Docker para ver o estado do contentor.

   ![Variáveis de inspeção](media/how-to-develop-csharp-module/view-log.png)

1. Navegue para a vista de depuração e código do Visual Studio e selecione o ficheiro de configuração de depuração para seu módulo. O nome de opção de depuração deve ser semelhante à  ***&lt;seu nome de módulo&gt;* de depuração remota**

1. Selecione **iniciar depuração** ou prima **F5**. Selecione o processo para anexar a.

1. Na vista de depuração e código do Visual Studio, verá as variáveis no painel esquerdo.

1. Para parar a sessão de depuração, primeiro selecione o botão Parar ou prima **Shift + F5**e, em seguida, selecione **Azure IoT Edge: Parar o simulador do IoT Edge** da paleta de comandos.

> [!NOTE]
> O exemplo anterior mostra como depurar os módulos do IoT Edge em contentores. Ele adicionado portas expostas ao contentor do seu módulo `createOptions` definições. Depois de concluir os módulos de depuração, recomendamos que remova estas portas expostas de módulos do IoT Edge prontos para produção.
>
> Para módulos de escrita em C#, incluindo as funções do Azure, este exemplo se baseia a versão de depuração `Dockerfile.amd64.debug`, que inclui o depurador de linha de comandos do .NET Core (VSDBG) na sua imagem de contentor ao criá-lo. Depois que Depure seu C# módulos, recomendamos que utilize diretamente o Dockerfile sem VSDBG de módulos do IoT Edge prontos para produção.

## <a name="debug-a-module-with-iot-edge-runtime-python-c"></a>Depurar um módulo com o runtime do IoT Edge (Python, C)

Em cada pasta de módulo, há vários arquivos de Docker para tipos de contentor diferente. Utilizar qualquer um dos ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste.

Atualmente, a depuração de suporte para módulos de Python e C está disponível apenas em contentores do Linux amd64.

### <a name="build-and-deploy-your-module"></a>Crie e implemente o seu módulo

1. No Visual Studio Code, abra a `deployment.debug.template.json` arquivo, que contém a versão de depuração das suas imagens de módulo com o elemento adequado `createOptions` conjunto de valores.

1. Se estiver a desenvolver o seu módulo em Python, siga estes passos antes de continuar:
   - Abra o ficheiro `main.py` e adicione esse código depois da secção de importação:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - Adicione a seguinte linha única de código para o retorno de chamada que deseja depurar:

      ```python
      ptvsd.break_into_debugger()
      ```

     Por exemplo, se deseja depurar o `receive_message_callback` método, teria que inserir essa linha de código, conforme mostrado abaixo:

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. Na paleta de comandos de código do Visual Studio:
   1. Execute o comando **Azure IoT Edge: Solução de compilação e de Push IoT Edge**.

   1. Selecione o `deployment.debug.template.json` ficheiro para a sua solução.

1. Na **dispositivos do Azure IoT Hub** secção da vista de Explorador de código do Visual Studio:
   1. Um ID de dispositivo do IoT Edge com o botão direito e, em seguida, selecione **criar a implementação de único dispositivo**.

   1. Navegue até à sua solução **config** pasta, selecione a `deployment.debug.amd64.json` do ficheiro e, em seguida, selecione **selecione o manifesto de implantação do Edge**.

Verá a implementação foi criada com êxito com um ID de implementação no terminal integrado.

Pode verificar o estado do contentor na vista de Docker de código do Visual Studio ou executando o `docker ps` comando no terminal.

### <a name="debug-your-module"></a>Depurar o seu módulo

Visual Studio Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isso `launch.json` arquivo foi gerado quando criou uma nova solução de IoT Edge. Ele atualiza cada vez que adiciona um novo módulo que oferece suporte à depuração.

1. Na vista de depuração e código do Visual Studio, selecione o ficheiro de configuração de depuração para seu módulo. O nome de opção de depuração deve ser semelhante à  ***&lt;seu nome de módulo&gt;* de depuração remota**

1. Abra o ficheiro do módulo da sua linguagem de desenvolvimento e adicione um ponto de interrupção:
   - **Python**: Open `main.py` e adicione um ponto de interrupção no método de retorno de chamada onde adicionou o `ptvsd.break_into_debugger()` linha.
   - **C**: Abra o ficheiro `main.c` e adicione um ponto de interrupção.

1. Selecione **iniciar depuração** ou selecione **F5**. Selecione o processo para anexar a.

1. Na vista de depuração e código do Visual Studio, verá as variáveis no painel esquerdo.

> [!NOTE]
> O exemplo anterior mostra como depurar os módulos do IoT Edge em contentores. Ele adicionado portas expostas ao contentor do seu módulo `createOptions` definições. Depois de concluir os módulos de depuração, recomendamos que remova estas portas expostas de módulos do IoT Edge prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Depois de criar seu módulo, saiba como [implementar módulos do Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
