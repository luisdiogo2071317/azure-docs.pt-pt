---
title: Início rápido do Azure IoT Edge + Windows | Microsoft Docs
description: Experimentar o Azure IoT Edge executando análise num dispositivo Edge simulado
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/24/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 19fd671514da0dbfb1704c37d4347e870763d41b
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091818"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Início rápido: Implementar o primeiro módulo IoT Edge do portal do Azure para um dispositivo Windows – pré-visualização

Neste guia de início rápido, utilize a interface cloud do Azure IoT Edge para implementar código pré-construído remotamente num dispositivo IoT Edge. Para realizar esta tarefa, primeiro utilize o seu dispositivo do Windows para simular um dispositivo IoT Edge, em seguida, pode implementar um módulo no mesmo.

Neste início rápido, vai aprender a:

1. Criar um Hub IoT.
2. Registar um dispositivo IoT Edge no seu hub IoT.
3. Instalar e iniciar o runtime do IoT Edge no seu dispositivo.
4. Implementar remotamente um módulo num dispositivo IoT Edge e enviar telemetria para o Hub IoT.

![Arquitetura do tutorial][2]

O módulo que vai implementar neste início rápido é um sensor simulado que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados simulados de informações empresariais. 

>[!NOTE]
>O runtime do IoT Edge no Windows está em [pré-visualização pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se não tiver uma subscrição do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido transforma o seu computador Windows ou máquina virtual num dispositivo IoT Edge. Se estiver a executar o Windows numa máquina virtual, ative a [virtualização aninhada][lnk-nested] e aloque, pelo menos, 2 GB de memória. 

Tenha os seguintes pré-requisitos prontos no computador que está a utilizar para um dispositivo IoT Edge:

1. Certifique-se de que está a utilizar uma versão suportada do Windows:
   * Windows 10 ou mais recente
   * Windows Server 2016 ou mais recente
2. Instale o [Docker para Windows][lnk-docker] e certifique-se de que está a ser executado.
3. Configurar o Docker para utilizar [contentores do Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Vai utilizar a CLI do Azure para concluir muitos dos passos neste início rápido e o IoT do Azure tem uma extensão para ativar funcionalidades adicionais. 

Adicione a extensão do IoT do Azure à instância da shell da cloud.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o início rápido ao criar o seu Hub IoT no portal do Azure.
![Criar Hub IoT][3]

O nível gratuito do Hub IoT funciona para este início rápido. Se tiver utilizado o Hub IoT anteriormente e já tiver um hub gratuito criado, pode utilizar esse hub IoT. Cada subscrição só pode ter um Hub IoT gratuito. 

1. No Azure Cloud Shell, crie um grupo de recursos. O código seguinte cria um grupo de recursos denominado **IoTEdgeResources** na região **E.U.A. Oeste**. Ao colocar todos os recursos dos inícios rápidos e os tutoriais num grupo, pode geri-los em conjunto. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

1. Crie um Hub IoT no seu novo grupo de recursos. O código seguinte cria um hub **F1** gratuito no grupo de recursos **IoTEdgeResources**. Substitua *{hub_name}* por um nome exclusivo para o hub IoT.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu Hub IoT recentemente criado.
![Registar um dispositivo][4]

Crie uma identidade de dispositivo para o seu dispositivo simulado para que este consiga comunicar com o seu hub IoT. Uma vez que os dispositivos do IoT Edge se comportam e podem ser geridos de forma diferente dos dispositivos do IoT típicos, declare esta opção como um dispositivo do IoT Edge desde o início. 

1. No Azure Cloud Shell, introduza o comando seguinte para criar um dispositivo com o nome **myEdgeDevice** no seu hub.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Obtenha a cadeia de ligação para o seu dispositivo, que liga o dispositivo físico à respetiva identidade do Hub IoT. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Copie a cadeia de ligação e guarde-a. Irá utilizar este valor para configurar o runtime do IoT Edge na secção seguinte. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Instalar e iniciar o runtime do IoT Edge

Instalar e iniciar o runtime do Azure IoT Edge no seu dispositivo IoT Edge. 
![Registar um dispositivo][5]

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança do IoT Edge** é iniciado sempre que um dispositivo Edge arranca e arranca o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e a monitorização de módulos no dispositivo IoT Edge, incluindo o hub do IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT. 

>[!NOTE]
>Os passos de instalação nesta secção são manuais por agora enquanto está a ser desenvolvido um script de instalação. 

As instruções nesta secção configuram o runtime do IoT Edge com contentores do Linux. Se quiser utilizar contentores do Windows, veja [Instalar o runtime do Azure IoT Edge no Windows para utilizar com contentores do Windows](how-to-install-iot-edge-windows-with-windows.md).

### <a name="download-and-install-the-iot-edge-service"></a>Transferir e instalar o serviço IoT Edge

1. No seu dispositivo IoT Edge, execute o PowerShell como administrador.

2. Transfira o pacote do serviço IoT Edge.

   ```powershell
   Invoke-WebRequest https://aka.ms/iotedged-windows-latest -o .\iotedged-windows.zip
   Expand-Archive .\iotedged-windows.zip C:\ProgramData\iotedge -f
   Move-Item c:\ProgramData\iotedge\iotedged-windows\* C:\ProgramData\iotedge\ -Force
   rmdir C:\ProgramData\iotedge\iotedged-windows
   $sysenv = "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment"
   $path = (Get-ItemProperty -Path $sysenv -Name Path).Path + ";C:\ProgramData\iotedge"
   Set-ItemProperty -Path $sysenv -Name Path -Value $path
   ```

3. Instale o vcruntime.

  ```powershell
  Invoke-WebRequest -useb https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe -o vc_redist.exe
  .\vc_redist.exe /quiet /norestart
  ```

4. Crie e inicie o serviço IoT Edge.

   ```powershell
   New-Service -Name "iotedge" -BinaryPathName "C:\ProgramData\iotedge\iotedged.exe -c C:\ProgramData\iotedge\config.yaml"
   Start-Service iotedge
   ```

5. Adicione exceções de firewall para as portas que o serviço IoT Edge utiliza.

   ```powershell
   New-NetFirewallRule -DisplayName "iotedged allow inbound 15580,15581" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 15580-15581 -Program "C:\programdata\iotedge\iotedged.exe" -InterfaceType Any
   ```

6. Crie um novo ficheiro chamado **iotedge.reg** e abra-o com um editor de texto. 

7. Adicione o código seguinte e guarde o ficheiro. 

   ```input
   Windows Registry Editor Version 5.00
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
   "CustomSource"=dword:00000001
   "EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
   "TypesSupported"=dword:00000007
   ```

8. Navegue para o ficheiro no Explorador de Ficheiros e faça duplo clique no mesmo para importar as alterações ao Registo do Windows. 

### <a name="configure-the-iot-edge-runtime"></a>Configurar o runtime do IoT Edge 

Configure o runtime com a cadeia de ligação do dispositivo IoT Edge que copiou quando registou o seu novo dispositivo. Em seguida, configure a rede de runtime. 

1. Abra o ficheiro de configuração do IoT Edge, localizado em `C:\ProgramData\iotedge\config.yaml`. Este ficheiro está protegido, por isso, execute um editor de texto como o Bloco de Notas como administrador e, em seguida, utilize o editor para abrir o ficheiro. 

2. Localize a secção **aprovisionamento** e atualize o valor de **device_connection_string** com a cadeia que copiou dos detalhes do dispositivo IoT Edge. 

3. Na janela do PowerShell de administrador, obtenha o nome de anfitrião do seu dispositivo IoT Edge e copie o resultado. 

   ```powershell
   hostname
   ```

4. No ficheiro de configuração, localize a secção **Nome de anfitrião do dispositivo Edge**. Atualize o valor de **hostname** com o nome de anfitrião que copiou do PowerShell.

3. Na janela do PowerShell de administrador, obtenha o endereço IP do seu dispositivo IoT Edge. 

   ```powershell
   ipconfig
   ```

4. Copie o valor para **Endereço IPv4** na secção **vEthernet (DockerNAT)** do resultado. 

5. Crie uma variável de ambiente denominada **IOTEDGE_HOST** e substitua *\<ip_address\>* pelo endereço IP do seu dispositivo IoT Edge. 

   ```powershell
   [Environment]::SetEnvironmentVariable("IOTEDGE_HOST", "http://<ip_address>:15580")
   ```

6. No ficheiro `config.yaml`, localize a secção **Definições de ligação**. Atualize os valores **management_uri** e **workload_uri** com o endereço IP e as portas que abriu na secção anterior. Substitua **\<GATEWAY_ADDRESS\>** pelo endereço IP de DockerNAT que copiou. 

   ```yaml
   connect: 
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

7. Localize a secção **Definições de escuta** e adicione os mesmos valores para **management_uri** e **workload_uri**. 

   ```yaml
   listen:
     management_uri: "http://<GATEWAY_ADDRESS>:15580"
     workload_uri: "http://<GATEWAY_ADDRESS>:15581"
   ```

8. Localize a secção **Definições do Moby Container Runtime** e verifique se o valor para **rede** está definido como `nat`.

9. Guarde o ficheiro de configuração. 

10. No PowerShell, reinicie o serviço IoT Edge.

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

### <a name="view-the-iot-edge-runtime-status"></a>Ver o estado de runtime do IoT Edge

Verifique se o runtime foi instalado e configurado corretamente.

1. Verifique o estado do serviço IoT Edge.

   ```powershell
   Get-Service iotedge
   ```

2. Se precisar de resolver problemas relacionados com o serviço, obtenha os registos do serviço. 

   ```powershell
   # Displays logs from today, newest at the bottom.

   Get-WinEvent -ea SilentlyContinue `
    -FilterHashtable @{ProviderName= "iotedged";
      LogName = "application"; StartTime = [datetime]::Today} |
    select TimeCreated, Message |
    sort-object @{Expression="TimeCreated";Descending=$false}
   ```

3. Veja todos os módulos em execução no seu dispositivo IoT Edge. Uma vez que o serviço foi iniciado pela primeira vez, deverá ver apenas o módulo **edgeAgent** em execução. O módulo edgeAgent é executado por predefinição e ajuda a instalar e iniciar quaisquer módulos adicionais que implementar no seu dispositivo. 

   ```powershell
   iotedge list
   ```

   ![Ver um módulo no seu dispositivo](./media/quickstart/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implementar um módulo

Gira o seu dispositivo Azure IoT Edge a partir da cloud para implementar um módulo que irá enviar dados telemétricos para o Hub IoT.
![Registar um dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Ver os dados gerados

Neste início rápido, criou um novo dispositivo IoT Edge e instalou o runtime do IoT Edge no mesmo. Depois, utilizou o portal do Azure para emitir um módulo do IoT Edge para ser executado no dispositivo, sem ter de realizar alterações no dispositivo propriamente dito. Neste caso, o módulo que emitiu cria dados ambientais que pode utilizar para os tutoriais. 

Certifique-se de que o módulo implementado a partir da cloud está em execução no seu dispositivo IoT Edge. 

```powershell
iotedge list
```

   ![Ver três módulos no seu dispositivo](./media/quickstart/iotedge-list-2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor para a cloud. 

```powershell
iotedge logs tempSensor -f
```

  ![Ver os dados a partir do seu módulo](./media/quickstart/iotedge-logs.png)

Também pode ver as mensagens recebidas pelo hub IoT através da ferramenta [Explorador do Hub IoT][lnk-iothub-explorer] ou da [extensão do Toolkit do Azure IoT para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser avançar para os tutoriais do IoT Edge, pode utilizar o dispositivo que registou e configurou neste início rápido. Caso contrário, pode eliminar os recursos do Azure que criou e remover o runtime do IoT Edge do seu dispositivo. 

### <a name="delete-azure-resources"></a>Eliminar recursos do Azure

Se tiver criado a sua máquina virtual e o hub IoT num novo grupo de recursos, pode eliminar esse grupo e todos os recursos associados. Se tiver alguma coisa nesse grupo de recursos que pretende manter, então basta eliminar os recursos individuais que quer limpar. 

Remova o grupo de **IoTEdgeResources**. 

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

### <a name="remove-the-iot-edge-runtime"></a>Remover o runtime do IoT Edge

Se planeia utilizar o dispositivo do IoT Edge para testes futuros, mas pretende impedir o módulo tempSensor de enviar dados para o seu hub IoT enquanto não está a ser utilizado, utilize o comando seguinte para parar o serviço IoT Edge. 

   ```powershell
   Stop-Service iotedge -NoWait
   ```

Pode reiniciar o serviço quando estiver preparado para começar novamente os testes

   ```powershell
   Start-Service iotedge
   ```

Se quiser remover as instalações do seu dispositivo, utilize os comandos seguintes.  

Remova o runtime do IoT Edge.

   ```powershell
   cmd /c sc delete iotedge
   rm -r c:\programdata\iotedge
   ```

Quando o runtime do IoT Edge é removido, os contentores que ele criou são parados, mas continuam no seu dispositivo. Veja todos os contentores.

   ```powershell
   docker ps -a
   ```

Elimine os contentores criados no seu dispositivo pelo runtime do IoT Edge. Altere o nome do contentor tempSensor, se o tiver chamado de outra forma. 

   ```powershell
   docker rm -f tempSensor
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```
   
## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um novo dispositivo IoT Edge e utilizou a interface da cloud do Azure IoT Edge para implementar código no dispositivo. Agora tem um dispositivo de teste a gerar dados não processados sobre o seu ambiente. 

Pode continuar para qualquer um dos outros tutoriais para saber mais sobre como o Azure IoT Edge o pode ajudar a transformar estes dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Filtrar os dados de sensores com uma Função do Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png


<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
