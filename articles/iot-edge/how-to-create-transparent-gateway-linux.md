---
title: Criar um gateway transparente com o Azure IoT Edge - Linux | Microsoft Docs
description: Utilize o limite de IoT do Azure para criar um gateway transparente, que pode processar as informações de vários dispositivos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5a78d6fb8ee52f0daba80a77cc8a5e75c2e5248d
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036521"
---
# <a name="create-a-linux-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Criar um dispositivo de limite de IoT Linux que atua como um gateway transparente

Este artigo fornece instruções detalhadas para a utilização de um dispositivo de limite de IoT como um gateway transparente. Para o resto deste artigo, o termo *gateway de IoT* refere-se a um dispositivo de limite de IoT utilizado como um gateway transparente. Para obter mais informações, consulte [de limite de IoT como um dispositivo pode ser utilizado como um gateway][lnk-edge-as-gateway], que fornece uma descrição geral conceptual.

>[!NOTE]
>Atualmente:
> * Se o gateway está desligado do IoT Hub, os dispositivos a jusante não é possível autenticar com o gateway.
> * Não é possível ligar dispositivos de limite de IoT para gateways de periferia de IoT.
> * Dispositivos a jusante não podem utilizar o carregamento de ficheiros.

A parte do disco rígida sobre como criar um gateway transparente em segurança está a ligar o gateway para os dispositivos a jusante. Limite de IoT do Azure permite-lhe utilizar a infraestrutura PKI para configurar ligações TLS seguras entre estes dispositivos. Neste caso, iremos está a permitir que um dispositivo a jusante ligar a um dispositivo de limite de IoT a funcionar como gateway transparente.  Para manter a segurança razoável, o dispositivo a jusante deve confirmar a identidade do dispositivo de limite vez que apenas pretende que os dispositivos que se ligam aos seus gateways e não é um gateway potencialmente malicioso.

Pode criar qualquer infraestrutura de certificados que permite a fidedignidade necessária para a topologia de gateway do dispositivo. Neste artigo, partimos do pressuposto a mesmo certificado a configuração que pretende utilizar para ativar [segurança de AC de x. 509] [ lnk-iothub-x509] no IoT Hub, que envolve um certificado de AC de x. 509 associado a um IoT hub específico (o IoT hub proprietário da AC ) e uma série de certificados, assinada com esta AC e uma AC para o dispositivo de limite.

![Configuração do gateway][1]

O gateway apresenta o respetivo certificado da AC dispositivo Edge no dispositivo a jusante durante o início da ligação. Verifica o dispositivo a jusante para se certificar de que o certificado de AC do dispositivo de limite que está assinado pelo certificado de AC de proprietário. Este processo permite ao dispositivo a jusante confirmar que o gateway é proveniente de uma origem fidedigna.

Os seguintes passos guiá-lo durante o processo de criação dos certificados e instalá-los nos locais corretos.

## <a name="prerequisites"></a>Pré-requisitos
1.  Instale o runtime do Azure IoT Edge num dispositivo Linux que pretende utilizar como o gateway transparente.
   * [Linux x64][lnk-install-linux-x64]
   * [ARM32 do Linux][lnk-install-linux-arm]

2.  Obter os scripts para gerar os certificados necessários de não produção com o seguinte comando. Estes scripts ajudam a criar os certificados necessários para configurar um gateway transparente. 

   ```cmd
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Estes scripts utilizam OpenSSL para gerar os certificados necessários e OpenSSL requer alguns programa de configuração.
   
   1. Navegue para o diretório no qual pretende trabalhar. A partir daqui em irá denominamos isto $WRKDIR.  Serão criados todos os ficheiros neste diretório.

      CD $WRKDIR
   
   1. Copie os ficheiros de configuração e de script para o diretório de trabalho.

      ```cmd
      cp azure-iot-sdk-c/tools/CACertificates/*.cnf .
      cp azure-iot-sdk-c/tools/CACertificates/certGen.sh .
      chmod 700 certGen.sh 
      ```

## <a name="certificate-creation"></a>Criação do certificado
1.  Crie o certificado de AC de proprietário e um certificado intermédio. Estes são todos colocados na `$WRKDIR`.

   ```cmd
   ./certGen.sh create_root_and_intermediate
   ```

   As saídas da execução de script são os seguintes certificados e chaves:
   * Certificados
      * `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
      * `$WRKDIR/certs/azure-iot-test-only.intermediate.cert.pem`
   * Chaves
      * `$WRKDIR/private/azure-iot-test-only.root.ca.key.pem`
      * `$WRKDIR/private/azure-iot-test-only.intermediate.key.pem`

2.  Crie o certificado de AC de dispositivo de limite e a chave privada com o comando abaixo.

   >[!NOTE]
   > **NÃO** utilizar um nome que é o mesmo nome de anfitrião DNS do gateway. Se o fizer, irá causar a certificação do cliente contra estes certificados falhar.

      ```cmd
      ./certGen.sh create_edge_device_certificate "<gateway device name>"
      ```

   As saídas da execução de script são os seguintes certificados e chaves:
   * `$WRKDIR/certs/new-edge-device.*`
   * `$WRKDIR/private/new-edge-device.key.pem`

## <a name="certificate-chain-creation"></a>Criação de cadeia do certificado
Crie uma cadeia de certificados do certificado de AC de proprietário, intermediária e certificado de AC de dispositivo de limite com o comando abaixo. Colocá-la num ficheiro cadeia permite-lhe facilmente instalá-lo no seu dispositivo de limite a funcionar como gateway transparente.

   ```cmd
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="installation-on-the-gateway"></a>Instalação do gateway
1.  Copie os seguintes ficheiros do $WRKDIR em qualquer lugar no seu dispositivo de limite, irá denominamos que $CERTDIR. Se gerou os certificados no seu dispositivo de limite, ignore este passo.

   * Certificado de AC do dispositivo-  `$WRKDIR/certs/new-edge-device-full-chain.cert.pem`
   * Chave privada de AC de dispositivo- `$WRKDIR/private/new-edge-device.key.pem`
   * Proprietário AC- `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`

2.  Definir o `certificate` propriedades no segurança Daemon ficheiro de configuração yaml para o caminho onde colocou os ficheiros de certificado e chave.

```yaml
certificates:
  device_ca_cert: "$CERTDIR/certs/new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR/private/new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Implementar EdgeHub ao gateway
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Esta secção tem de criar uma implementação seemingly vazia; No entanto Edge Hub é automatcially adicionada a todas as implementações, mesmo que existem não existem outros módulos presentes. Hub de limite é o módulo só que precisa de um dispositivo de limite para atuar como um gateway transparente, para que criar uma implementação vazia é suficiente. 
1. No portal do Azure, navegue para o seu hub IoT.
2. Aceda a **IoT Edge** e selecione o seu dispositivo de limite de IoT que pretende utilizar como um gateway.
3. Selecione **Definir Módulos**.
4. Selecione **Seguinte**.
5. No **especificar rotas** passo, deve ter uma rota predefinida que envia todas as mensagens de todos os módulos ao IoT Hub. Se não estiver, adicione o código seguinte, em seguida, selecione **seguinte**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. O passo de modelo de revisão, selecione **submeter**.

## <a name="installation-on-the-downstream-device"></a>Instalação no dispositivo a jusante
Um dispositivo a jusante pode ser qualquer aplicação utilizando o [dispositivos IoT do Azure SDK][lnk-devicesdk], tal como aquele simple descrito em [ligar o seu dispositivo ao seu IoT hub através do .NET] [ lnk-iothub-getstarted]. Uma aplicação de dispositivo a jusante tem de confiar a **proprietário AC** certificado para validar as ligações de TLS para os dispositivos de gateway. Normalmente, é possível executar este passo de duas formas: ao nível do SO ou (para alguns idiomas) ao nível da aplicação.

### <a name="os-level"></a>Nível de SO
Instalar este certificado no arquivo de certificados de SO irá permitir todas as aplicações a utilizar o proprietário do certificado de AC como um certificado fidedigno.

* Ubuntu - Eis um exemplo de como instalar um certificado de AC num anfitrião Ubuntu.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    Deverá ver uma mensagem a indicar "certificados de atualização no /etc/ssl/certs... 1 adicionadas, removidas 0; concluído."

* Windows - [isto](https://msdn.microsoft.com/en-us/library/cc750534.aspx) artigo fornece detalhes sobre como fazê-lo num dispositivo Windows utilizando o wizzard de importação de certificados. 

### <a name="application-level"></a>Nível de aplicação
Para aplicações de .NET, pode adicionar o fragmento seguinte para confiar um certificado no formato PEM. Inicializar a variável `certPath` com `$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Ligue o dispositivo a jusante para o gateway
Tem de inicializar o sdk do dispositivo IoT Hub com uma cadeia de ligação que faça referência a nome de anfitrião do dispositivo de gateway. Isto é feito, acrescentando o `GatewayHostName` propriedade para a cadeia de ligação do dispositivo. Por exemplo, aqui está uma cadeia de ligação do dispositivo de exemplo para um dispositivo, é acrescentado a `GatewayHostName` propriedade:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Este é um comando de exemplo que os testes que tudo foi configurado corretamente. A sohuld uma mensagem a indicar "verificada OK".
   >
   >OpenSSL s_client-ligar mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts

## <a name="routing-messages-from-downstream-devices"></a>Encaminhamento de mensagens de dispositivos a jusante
O tempo de execução do limite de IoT pode encaminhar mensagens enviadas a partir de dispositivos a jusante, tal como as mensagens enviadas por módulos. Isto permite-lhe efetuar análises de um módulo em execução no gateway antes de enviar quaisquer dados para a nuvem. O abaixo rota seria utilizada para enviar mensagens a partir de um dispositivo a jusante com o nome `sensor` para um nome de módulo `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Consulte o [artigo de composição do módulo] [lnk--composição do módulo] para obter mais detalhes sobre o encaminhamento de mensagens.

## <a name="next-steps"></a>Passos Seguintes
[Compreender os requisitos e ferramentas para o desenvolvimento de módulos de limite de IoT][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-linux-x64]: ./how-to-install-iot-edge-linux.md
[lnk-install-linux-arm]: ./how-to-install-iot-edge-linux-arm.md
[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
