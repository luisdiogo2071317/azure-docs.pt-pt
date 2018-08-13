---
title: Simular o Azure IoT Edge no Windows | Microsoft Docs
description: Instalar o runtime do Azure IoT Edge num dispositivo simulado no Windows e implementar o seu primeiro módulo
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 06/08/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1a45841564b0c985662e6d2db320111fa27d1e92
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578182"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Início rápido: Implementar o primeiro módulo IoT Edge do portal do Azure para um dispositivo Windows – pré-visualização

O Azure IoT Edge permite-lhe realizar o processamento de dados e de análise nos seus dispositivos, em vez de ter de solicitar todos os dados para a cloud. Os tutoriais do IoT Edge demonstram como implementar diferentes tipos de módulos, mas primeiro precisa de um dispositivo para testar. 

Neste início rápido, vai aprender a:

1. Criar um Hub IoT
2. Registar um dispositivo do IoT Edge
3. Iniciar o runtime do IoT Edge
4. Implementar um módulo

![Arquitetura do tutorial][2]

O módulo que vai implementar neste início rápido é um sensor simulado que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados simulados de informações empresariais. 

>[!NOTE]
>O runtime do IoT Edge no Windows está em [pré-visualização pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se não tiver uma subscrição do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido parte do princípio de que está a utilizar um computador ou uma máquina virtual que executa o Windows para simular um dispositivo de IoT. Se estiver a executar o Windows numa máquina virtual, ative a [virtualização aninhada][lnk-nested] e aloque, pelo menos, 2 GB de memória. 

Tenha os seguintes pré-requisitos prontos no computador que está a utilizar para um dispositivo IoT Edge:

1. Certifique-se de que está a utilizar uma versão suportada do Windows:
   * Windows 10 ou mais recente
   * Windows Server 2016 ou mais recente
2. Instale o [Docker para Windows][lnk-docker] e certifique-se de que está a ser executado.
3. Configurar o Docker para utilizar [contentores do Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers)

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o início rápido ao criar o seu Hub IoT no portal do Azure.
![Criar Hub IoT][3]

Crie o seu Hub IoT num grupo de recursos que pode utilizar para manter e gerir todos os recursos que criou neste início rápido. Atribua-lhe um nome que seja fácil de recordar, como **IoTEdgeResources**. Ao colocar todos os recursos dos inícios rápidos e dos tutoriais num grupo, pode geri-los em conjunto e removê-los facilmente quando concluir os testes. 

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu Hub IoT recentemente criado.
![Registar um dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

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

  Mantenha a variável de ambiente entre reinícios.

  ```powershell
  SETX /M IOTEDGE_HOST "http://<ip_address>:15580"
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

8. Localize a secção **Definições do Moby Container Runtime** e verifique se o valor para **rede** está por comentar e definido como **azure-iot-edge**

   ```yaml
   moby_runtime:
     docker_uri: "npipe://./pipe/docker_engine"
     network: "azure-iot-edge"
   ```
   
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
    sort-object @{Expression="TimeCreated";Descending=$false} |
    format-table -autosize -wrap
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

Também pode ver as mensagens recebidas pelo hub IoT com a [extensão Azure IoT Toolkit para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser avançar para os tutoriais do IoT Edge, pode utilizar o dispositivo que registou e configurou neste início rápido. Caso contrário, pode eliminar os recursos do Azure que criou e remover o runtime do IoT Edge do seu dispositivo. 

### <a name="delete-azure-resources"></a>Eliminar recursos do Azure

Se tiver criado a sua máquina virtual e o hub IoT num novo grupo de recursos, pode eliminar esse grupo e todos os recursos associados. Se tiver alguma coisa nesse grupo de recursos que pretende manter, então basta eliminar os recursos individuais que quer limpar. 

Para remover um grupo de recursos, siga estes passos: 

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.
2. Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos que contém o seu Hub IoT. 
3. À direita do seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.
4. É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos novamente para confirmar e, em seguida, clique em **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.

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
[2]: ./media/quickstart/install-edge-full.png
[3]: ./media/quickstart/create-iot-hub.png
[4]: ./media/quickstart/register-device.png
[5]: ./media/quickstart/start-runtime.png
[6]: ./media/quickstart/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-install-iotcore]: how-to-install-iot-core.md
[lnk-account]: https://azure.microsoft.com/free
