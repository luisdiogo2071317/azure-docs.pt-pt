---
title: Connnect um Raspberry Pi à sua aplicação do Azure IoT Central (c#) | Microsoft Docs
description: Como um programador de dispositivo, como ligar um Raspberry Pi a sua aplicação Central de IoT do Azure com c#.
author: dominicbetts
ms.author: dobett
ms.date: 01/22/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 58f363c522f3e5abe6bf49a2aebafe4e953e00df
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628594"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-c"></a>Ligar um Raspberry Pi à sua aplicação do Azure IoT Central (c#)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Este artigo descreve como, como um programador de dispositivo, para ligar um Raspberry Pi a sua aplicação do Microsoft Azure IoT Central com a c# linguagem de programação.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

* [.NET core 2](https://www.microsoft.com/net) instalado no computador de desenvolvimento. Também deverá ter um editor de código adequado, tais como [Visual Studio Code](https://code.visualstudio.com/).
* Uma aplicação do Azure IoT Central criada a partir de **exemplo Devkits** modelo de aplicação. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
* Um dispositivo de Raspberry Pi com o sistema de operativo Raspbian.

Uma aplicação criada a partir de **Devkits de exemplo** modelo de aplicação inclui um **Raspberry Pi** modelo de dispositivo com as seguintes características:

### <a name="telemetry-measurements"></a>Medidas de telemetria

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| humidade       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | grupo de gestão     | -2000   | 2000    | 0              |
| accelerometerY | grupo de gestão     | -2000   | 2000    | 0              |
| accelerometerZ | grupo de gestão     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>Definições

Definições de um valor numérico

| Nome a apresentar | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Tensão      | setVoltage | Volts | 0              | 0       | 240     | 0       |
| Atual      | setCurrent | AMPS  | 0              | 0       | 100     | 0       |
| Ventoinha velocidade    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Definições de ativar/desativar

| Nome a apresentar | Nome do campo | No texto | Desativar texto | Inicial |
| ------------ | ---------- | ------- | -------- | ------- |
| RESPOSTA A INCIDENTES           | activateIR | ON      | OFF      | Desativado     |

### <a name="properties"></a>Propriedades

| Tipo            | Nome a apresentar | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade do dispositivo | Die número   | dieNumber  | número    |
| Texto            | Localização     | localização   | N/A       |

### <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real do **Raspberry Pi** modelo do dispositivo e tome nota da cadeia de ligação de dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

## <a name="create-your-net-application"></a>Criar a sua aplicação .NET

Pode criar e testar a aplicação de dispositivo no seu computador de secretária.

Para concluir os passos seguintes, pode utilizar o Visual Studio Code. Para obter mais informações, consulte [trabalhar com c#](https://code.visualstudio.com/docs/languages/csharp).

> [!NOTE]
> Se preferir, pode concluir os seguintes passos com um editor de código diferentes.

1. Para inicializar o projeto de .NET e adicionar os pacotes de NuGet necessários, execute os seguintes comandos:

  ```cmd/sh
  mkdir pisample
  cd pisample
  dotnet new console
  dotnet add package Microsoft.Azure.Devices.Client
  dotnet restore
  ```

1. Abra o `pisample` pasta no Visual Studio Code. Em seguida, abra o **pisample.csproj** ficheiro de projeto. Adicionar o `<RuntimeIdentifiers>` tag mostrado no seguinte fragmento:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifiers>win-arm;linux-arm</RuntimeIdentifiers>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices.Client" Version="1.5.2" />
      </ItemGroup>
    </Project>
    ```

    > [!NOTE]
    > O **Microsoft.Azure.Devices.Client** o número de versão do pacote pode ser superior ao mostradas.

1. Guardar **pisample.csproj**. Se o Visual Studio Code pede-lhe para executar o comando restore, escolha **restaurar**.

1. Abra **Program.cs** e substitua o conteúdo com o seguinte código:

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    using Newtonsoft.Json;

    namespace pisample
    {
      class Program
      {
        static string DeviceConnectionString = "{your device connection string}";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();
        static CancellationTokenSource cts;
        static double baseTemperature = 60;
        static double basePressure = 500;
        static double baseHumidity = 50;
        static void Main(string[] args)
        {
          Console.WriteLine("Raspberry Pi Azure IoT Central example");

          try
          {
            InitClient();
            SendDeviceProperties();

            cts = new CancellationTokenSource();
            SendTelemetryAsync(cts.Token);

            Console.WriteLine("Wait for settings update...");
            Client.SetDesiredPropertyUpdateCallbackAsync(HandleSettingChanged, null).Wait();
            Console.ReadKey();
            cts.Cancel();
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static void InitClient()
        {
          try
          {
            Console.WriteLine("Connecting to hub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        public static async void SendDeviceProperties()
        {
          try
          {
            Console.WriteLine("Sending device properties:");
            Random random = new Random();
            TwinCollection telemetryConfig = new TwinCollection();
            reportedProperties["dieNumber"] = random.Next(1, 6);
            Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static async void SendTelemetryAsync(CancellationToken token)
        {
          try
          {
            Random rand = new Random();

            while (true)
            {
              double currentTemperature = baseTemperature + rand.NextDouble() * 20;
              double currentPressure = basePressure + rand.NextDouble() * 100;
              double currentHumidity = baseHumidity + rand.NextDouble() * 20;

              var telemetryDataPoint = new
              {
                humidity = currentHumidity,
                pressure = currentPressure,
                temp = currentTemperature
              };
              var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
              var message = new Message(Encoding.ASCII.GetBytes(messageString));

              token.ThrowIfCancellationRequested();
              await Client.SendEventAsync(message);

              Console.WriteLine("{0} > Sending telemetry: {1}", DateTime.Now, messageString);

              await Task.Delay(1000);
            }
          }
          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Intentional shutdown: {0}", ex.Message);
          }
        }

        private static async Task HandleSettingChanged(TwinCollection desiredProperties, object userContext)
        {
          try
          {
            Console.WriteLine("Received settings change...");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            string setting = "fanSpeed";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setVoltage";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "setCurrent";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            setting = "activateIR";
            if (desiredProperties.Contains(setting))
            {
              // Act on setting change, then
              AcknowledgeSettingChange(desiredProperties, setting);
            }
            await Client.UpdateReportedPropertiesAsync(reportedProperties);
          }

          catch (Exception ex)
          {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
          }
        }

        private static void AcknowledgeSettingChange(TwinCollection desiredProperties, string setting)
        {
          reportedProperties[setting] = new
          {
            value = desiredProperties[setting]["value"],
            status = "completed",
            desiredVersion = desiredProperties["$version"],
            message = "Processed"
          };
        }
      }
    }
    ```

    > [!NOTE]
    > Atualizar o marcador de posição `{your device connection string}` no próximo passo.

## <a name="run-your-net-application"></a>Execute a sua aplicação .NET

Adicione a cadeia de ligação específica do dispositivo para o código para o dispositivo autenticar com o Azure IoT Central. Apontou da cadeia de ligação quando adicionou o seu dispositivo real à sua aplicação do Azure IoT Central.

1. Substitua `{your device connection string}` no **Program.cs** ficheiro com a cadeia de ligação que apontou anteriormente.

1. Execute o seguinte comando no seu ambiente de linha de comandos:

  ```cmd/sh
  dotnet restore
  dotnet publish -r linux-arm
  ```

1. Copiar o `pisample\bin\Debug\netcoreapp2.0\linux-arm\publish` pasta para o seu dispositivo Raspberry Pi. Pode utilizar o **scp** comandos para copiar os ficheiros, por exemplo:

    ```cmd/sh
    scp -r publish pi@192.168.0.40:publish
    ```

    Para obter mais informações, consulte [acesso remoto Raspberry Pi](https://www.raspberrypi.org/documentation/remote-access/).

1. Inicie sessão no seu dispositivo Raspberry Pi e execute os seguintes comandos numa shell:

    ```cmd/sh
    sudo apt-get update
    sudo apt-get install libc6 libcurl3 libgcc1 libgssapi-krb5-2 liblttng-ust0 libstdc++6 libunwind8 libuuid1 zlib1g
    ```

1. No seu Raspberry Pi, execute os seguintes comandos:

    ```cmd/sh
    cd publish
    chmod 777 pisample
    ./pisample
    ```

    ![Início do programa](./media/howto-connect-raspberry-pi-csharp/device_begin.png)

1. Na aplicação do Azure IoT Central, pode ver como o código em execução no Raspberry Pi interage com a aplicação:

    * No **medidas** página para o seu dispositivo real, pode ver a telemetria.
    * No **propriedades** página, pode ver o valor a que relatados **Die número** propriedade.
    * No **definições** página, pode alterar várias definições no Raspberry Pi, tais como velocidade tensão e ventoinha.

    A seguinte captura de ecrã mostra o Raspberry Pi receber a alteração da definição:

    ![Alteração da definição de recebe raspberry Pi](./media/howto-connect-raspberry-pi-csharp/device_switch.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um Raspberry Pi a sua aplicação do Azure IoT Central, Eis os passos sugeridos:

* [Ligar uma aplicação de cliente do Node.js genérica para o Azure IoT Central](howto-connect-nodejs.md)