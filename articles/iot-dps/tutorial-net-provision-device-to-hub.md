---
title: "Aprovisionar um dispositivo utilizando o Azure IoT Hub dispositivo aprovisionamento de serviço (.NET) | Microsoft Docs"
description: "Aprovisionar o seu dispositivo para um único IoT hub com o Azure IoT Hub dispositivo aprovisionamento de serviço (.NET)"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6919f962d853faa572ee7ad5d0cb9aeacd3bd2b6
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="enroll-the-device-to-an-iot-hub-using-the-azure-iot-hub-provisioning-service-client-net"></a>Inscrever o dispositivo a um IoT hub, utilizando o Azure IoT Hub aprovisionamento de serviço de cliente (.NET)

O tutorial anterior, aprendeu a configurar um dispositivo para ligar ao seu serviço de aprovisionamento de dispositivos. Neste tutorial, irá aprender a utilizar este serviço para aprovisionar o seu dispositivo ao IoT hub único, utilizando ambos  **_inscrição individuais_**  e  **_inscrição grupos_**. Este tutorial mostrar-lhe como:

> [!div class="checklist"]
> * Inscrever o dispositivo
> * Iniciar o dispositivo
> * Certifique-se de que o dispositivo está registado

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, certifique-se configurar o seu dispositivo e a respetiva *módulo de Hardware de segurança* tal como explicado no tutorial [configurar um dispositivo para aprovisionar a utilizar o serviço de aprovisionamento de dispositivos do Azure IoT Hub](./tutorial-set-up-device.md).

* Visual Studio 2015 ou Visual Studio 2017

> [!NOTE]
> Não é necessário o Visual Studio. A instalação do [.NET](https://www.microsoft.com/net) é suficiente e os programadores podem utilizar as respetivas preferencial editor no Windows ou Linux.  

Este tutorial simula o período durante ou imediatamente após o hardware de fabricantes de processo, quando as informações do dispositivo são adicionadas ao serviço de aprovisionamento. Este código, normalmente, é executado no PC ou um dispositivo de fábrica que pode executar código do .NET e não deve ser adicionado aos dispositivos.


## <a name="enroll-the-device"></a>Inscrever o dispositivo

Este passo envolve a adição de artefactos de segurança exclusiva do dispositivo para o serviço de aprovisionamento de dispositivos. Destes artefactos de segurança são os seguintes:

- Para dispositivos baseados em TPM:
    - O *chave de endossamento* que seja exclusivo para cada TPM chip ou simulação. Leia o [compreender chave de endossamento TPM](https://technet.microsoft.com/library/cc770443.aspx) para obter mais informações.
    - O *ID de registo* que é utilizado para identificar exclusivamente um dispositivo no espaço de nomes/âmbito. Esta pode ou não pode ser o mesmo que o ID de dispositivo. O ID é obrigatório para cada dispositivo. Para dispositivos baseados em TPM, o ID de registo pode ser derivado da TPM, por exemplo, um hash SHA-256 da chave de endossamento TPM.

- Para dispositivos de x. 509 com base em:
    - O [certificado x. 509 emitido para o dispositivo](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), na forma de um *. pem* ou um *. cer* ficheiro. Para a inscrição individuais, tem de utilizar o *certificado folha* para o seu sistema de x. 509, enquanto para os grupos de inscrição, tem de utilizar o *certificado de raiz* ou equivalente *: signatário inválido certificado*.
    - O *ID de registo* que é utilizado para identificar exclusivamente um dispositivo no espaço de nomes/âmbito. Esta pode ou não pode ser o mesmo que o ID de dispositivo. O ID é obrigatório para cada dispositivo. Para os x. 509 com base em dispositivos, o ID de registo é derivado de nome do certificado comum (CN). Para mais informações sobre estes requisitos, consulte [conceitos de dispositivo](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device).

Existem duas formas de inscrever o dispositivo para o serviço de aprovisionamento de dispositivos:

- **As inscrições individuais** representa uma entrada para um único dispositivo pode registar com o serviço de aprovisionamento de dispositivos. As inscrições individuais podem utilizar certificados x. 509 ou tokens SAS (por um TPM real ou virtual) como mecanismos de atestado. Recomendamos que utilize individuais inscrições de dispositivos, que requerem configurações de iniciais exclusivas, ou para dispositivos que só podem utilizar os tokens SAS através do TPM como o mecanismo de atestado. As inscrições individuais podem ter o ID de dispositivo do hub IoT pretendido especificado.

- **Grupos de inscrição** representa um grupo de dispositivos que partilham um mecanismo de atestado específico. Recomendamos que utilize um grupo de inscrição de um grande número de dispositivos, que partilham uma configuração desejada inicial, ou para dispositivos que todos os mesmo inquilino. Os grupos de inscrição são x. 509 apenas e todos os partilham um certificado de assinatura na respetiva cadeia de certificados x. 509.

### <a name="enroll-the-device-using-individual-enrollments"></a>Inscrever o dispositivo utilizando as inscrições individuais

1. No Visual Studio, crie um projeto do Visual c# aplicação da consola utilizando o **aplicação de consola** modelo de projeto. Nomeie o projeto **DeviceProvisioning**.
    
1. No Explorador de soluções, clique com botão direito do **DeviceProvisioning** projeto e, em seguida, clique em **gerir pacotes NuGet...** .

1. No **Gestor de pacotes NuGet** janela, selecione **procurar** e procure **microsoft.azure.devices.provisioning.service**. Selecione a entrada e clique em **instalar** para instalar o **Microsoft.Azure.Devices.Provisioning.Service** do pacote e aceite os termos de utilização. Este procedimento transfere, instala e adiciona uma referência para o [SDK do serviço de aprovisionamento de dispositivos de IoT do Azure](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet pacote e as respetivas dependências.

1. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:
   
    ```csharp
    using Microsoft.Azure.Devices.Provisioning.Service;
    ```

1. Adicione os seguintes campos à classe **Programa**. Substitua o valor do marcador de posição pela cadeia de ligação de DPS indicada na secção anterior.
   
    ```csharp
    static readonly string ServiceConnectionString = "{DPS connection string}";

    private const string SampleRegistrationId = "sample-individual-csharp";
    private const string SampleTpmEndorsementKey =
            "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
            "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
            "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
            "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
            "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
    private const string OptionalDeviceId = "myCSharpDevice";
    private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
    ```

1. Adicione o seguinte para implementar a inscrição do dispositivo:

    ```csharp
    static async Task SetRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetRegistrationData");

        Attestation attestation = new TpmAttestation(SampleTpmEndorsementKey);

        IndividualEnrollment individualEnrollment = new IndividualEnrollment(SampleRegistrationId, attestation);

        individualEnrollment.DeviceId = OptionalDeviceId;
        individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;

        Console.WriteLine("\nAdding new individualEnrollment...");
        var serviceClient = ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString);

        IndividualEnrollment individualEnrollmentResult =
            await serviceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);

        Console.WriteLine("\nIndividualEnrollment created with success.");
        Console.WriteLine(individualEnrollmentResult);
    }
    ```

1. Por fim, adicione o seguinte código para o **Main** método para abrir a ligação ao seu IoT hub e iniciar a inscrição:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Provisioning example");

        SetRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. No Visual Studio Explorador de soluções, faça duplo clique sua solução e, em seguida, clique em **definir projetos de arranque...** . Selecione **projeto de arranque único**e, em seguida, selecione o **DeviceProvisioning** projeto no menu pendente.  

1. Executar a aplicação de dispositivo .NET **DeviceProvisiong**. Deve definir a cópia de segurança de aprovisionamento para o dispositivo: 

    ![Registo individuais, executar](./media/tutorial-net-provision-device-to-hub/individual.png)

Quando o dispositivo é inscrito com êxito, deve ver apresentá-la no portal da seguinte forma:

   ![Inscrição concluída com êxito no portal](./media/tutorial-net-provision-device-to-hub/individual-portal.png)

### <a name="enroll-the-device-using-enrollment-groups"></a>Inscrever o dispositivo utilizar grupos de inscrição

> [!NOTE]
> O exemplo de grupo de inscrição requer um certificado x. 509.

1. No Explorador de soluções do Visual Studio, abra o **DeviceProvisioning** projeto criado acima. 

1. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:
    
    ```csharp
    using System.Security.Cryptography.X509Certificates;
    ```

1. Adicione os seguintes campos à classe **Programa**. Substitua o valor do marcador de posição de X509 localização do certificado.
   
    ```csharp
    private const string X509RootCertPathVar = "{X509 Certificate Location}";
    private const string SampleEnrollmentGroupId = "sample-group-csharp";
    ```

1. Adicione o seguinte ao **Program.cs** implementar a inscrição para o grupo:

    ```csharp
    public static async Task SetGroupRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetGroupRegistrationData");

        using (ProvisioningServiceClient provisioningServiceClient =
                ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString))
        {
            Console.WriteLine("\nCreating a new enrollmentGroup...");

            var certificate = new X509Certificate2(X509RootCertPathVar);

            Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);

            EnrollmentGroup enrollmentGroup = new EnrollmentGroup(SampleEnrollmentGroupId, attestation);

            Console.WriteLine(enrollmentGroup);
            Console.WriteLine("\nAdding new enrollmentGroup...");

            EnrollmentGroup enrollmentGroupResult =
                await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);

            Console.WriteLine("\nEnrollmentGroup created with success.");
            Console.WriteLine(enrollmentGroupResult);
        }
    }
    ```

1. Por fim, substitua o seguinte código para o **Main** método para abrir a ligação ao seu IoT hub e iniciar a inscrição de grupo:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Group Provisioning example");

        SetGroupRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

1. Executar a aplicação de dispositivo .NET **DeviceProvisiong**. Deve definir a segurança grupo aprovisionamento para o dispositivo: 

    ![Registo de grupo, executar](./media/tutorial-net-provision-device-to-hub/group.png)

    Quando o grupo de dispositivo está inscrito com êxito, deve ver apresentá-la no portal da seguinte forma:

   ![Inscrição de grupo com êxito no portal](./media/tutorial-net-provision-device-to-hub/group-portal.png)


## <a name="start-the-device"></a>Iniciar o dispositivo

Neste momento, o programa de configuração seguinte está preparado para o registo de dispositivos:

1. O dispositivo ou grupo de dispositivos são inscritos ao seu serviço de aprovisionamento de dispositivos, e 
2. O dispositivo está pronto com a configurado e acessível através da aplicação utilizando o SDK do cliente do serviço de aprovisionamento de dispositivos de segurança.

Inicie o dispositivo para permitir que a aplicação de cliente iniciar o registo com o serviço de aprovisionamento de dispositivos.  


## <a name="verify-the-device-is-registered"></a>Certifique-se de que o dispositivo está registado

Uma vez arranques seu dispositivo, as seguintes ações que devem ter lugar. Consulte a aplicação de exemplo do simulador TPM [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) para obter mais detalhes. 

1. O dispositivo envia um pedido de registo ao seu serviço de aprovisionamento de dispositivos.
2. Para dispositivos do TPM, o serviço de aprovisionamento de dispositivos envia um desafio de registo para o qual o dispositivo responde. 
3. No registo com êxito, o serviço de aprovisionamento de dispositivos envia que o URI do IoT hub, ID de dispositivo e a chave encriptada de volta para o dispositivo. 
4. A IoT Hub aplicação cliente no dispositivo, em seguida, liga ao seu hub. 
5. Em ligação com êxito para o hub, deverá ver o dispositivo aparecem do hub IoT **Explorador de dispositivo**. 

    ![Ligação com êxito ao hub no portal](./media/tutorial-net-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Inscrever o dispositivo
> * Iniciar o dispositivo
> * Certifique-se de que o dispositivo está registado

Avançar para o próximo tutorial para saber como aprovisionar vários dispositivos em hubs com balanceamento de carga. 

> [!div class="nextstepaction"]
> [Aprovisionar dispositivos entre os hubs IoT do balanceamento de carga](./tutorial-provision-multiple-hubs.md)
