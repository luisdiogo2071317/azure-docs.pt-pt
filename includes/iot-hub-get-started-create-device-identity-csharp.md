## <a name="create-a-device-identity"></a>Criar uma identidade de dispositivo

Nesta secção, vai criar uma aplicação de consola .NET que cria uma identidade de dispositivo no registo de identidade do seu Hub IoT. Não é possível ligar um dispositivo ao hub IoT, exceto se tiver uma entrada no registo de identidade. Para obter mais informações, veja a secção "Identity registry" (Registo de identidades) do [Hub IoT developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity]. Ao executar esta aplicação de consola, será gerado um ID de dispositivo exclusivo e uma chave. O dispositivo utiliza estes valores para se identificar quando envia mensagens do dispositivo para cloud para o Hub IoT. Os IDs dos dispositivos são sensíveis às maiúsculas e minúsculas.

1. No Visual Studio, adicione um projeto Visual C# no Ambiente de Trabalho Clássico do Windows a uma solução nova, utilizando o modelo de projeto **Aplicação de Consola (.NET Framework)**. Certifique-se de ter a versão 4.5.1 ou superior do .NET Framework. Atribua ao projeto o nome **CreateDeviceIdentity** e atribua à solução o nome **IoTHubGetStarted**.

    ![Novo projeto do Visual C# no Ambiente de Trabalho Clássico do Windows][10]

1. No Explorador de Soluções, clique com o botão direito do rato no projeto **CreateDeviceIdentity** e, em seguida, clique em **Gerir Pacotes de NuGet**.

1. Na janela **Gestor de Pacote NuGet**, selecione **Procurar**, pesquise **microsoft.azure.devices**, selecione **Instalar** para instalar o pacote **Microsoft.Azure.Devices** e aceite os termos de utilização. Este procedimento transfere, instala e adiciona uma referência ao pacote NuGet do [SDK do serviço do Azure IoT][lnk-nuget-service-sdk] e às respetivas dependências.

    ![Janela Gestor de Pacote NuGet][11]

1. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:

    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Common.Exceptions;
    ```

1. Adicione os seguintes campos à classe **Programa**. Substitua o valor do marcador de posição pela cadeia de ligação do Hub IoT para o hub que criou na secção anterior.

    ```csharp
    static RegistryManager registryManager;
    static string connectionString = "{iot hub connection string}";
    ```

1. Adicione o seguinte método à classe **Programa**:

    ```csharp
    private static async Task AddDeviceAsync()
    {
      string deviceId = "myFirstDevice";
      Device device;
      try
      {
          device = await registryManager.AddDeviceAsync(new Device(deviceId));
      }
      catch (DeviceAlreadyExistsException)
      {
          device = await registryManager.GetDeviceAsync(deviceId);
      }
      Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
    }
    ```

    Este método cria uma identidade de dispositivo com o ID **myFirstDevice**. (Se o ID desse dispositivo já existir no registo de identidade, o código apenas obtém as informações do dispositivo existente.) De seguida, a aplicação irá apresentar a chave primária para essa identidade. Esta chave vai ser utilizada na aplicação do dispositivo simulado para ligar ao seu hub IoT.
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. Por fim, adicione as seguintes linhas ao método **Main**:

    ```csharp
    registryManager = RegistryManager.CreateFromConnectionString(connectionString);
    AddDeviceAsync().Wait();
    Console.ReadLine();
    ```

1. Execute esta aplicação e tome nota da chave do dispositivo.

    ![Chave do dispositivo gerada pela aplicação][12]

> [!NOTE]
> O registo de identidade do Hub IoT apenas armazena identidades de dispositivos para permitir um acesso seguro ao Hub IoT. O registo de identidades armazena os IDs de dispositivo e as chaves para utilizar como credenciais de segurança. O registo de identidades também armazena um sinalizador ativado/desativado para cada dispositivo que pode utilizar para desativar o acesso a esse dispositivo. Se a sua aplicação tiver de armazenar outros metadados específicos do dispositivo, deverá utilizar um armazenamento específico da aplicação.  Para obter mais informações, veja o [IoT Hub developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity].

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png

<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
