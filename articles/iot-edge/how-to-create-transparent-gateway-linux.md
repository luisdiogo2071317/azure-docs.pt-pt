---
title: Criar um gateway transparente com o Azure IoT Edge - Linux | Documentos da Microsoft
description: Utilizar o Azure IoT Edge para criar um gateway transparente, que pode processar informações para vários dispositivos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 45179f8f1f46be764144bdc22d5bab3548e9401d
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346064"
---
# <a name="create-a-linux-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Criar um dispositivo do IoT Edge do Linux que atua como um gateway transparente

Este artigo fornece instruções detalhadas para a utilização de um dispositivo IoT Edge como gateway transparente. Para o restante deste artigo, o termo *gateway do IoT Edge* refere-se a um dispositivo IoT Edge utilizado como um gateway transparente. Para obter mais informações, consulte [como um dispositivo do IoT Edge pode ser utilizado como um gateway][lnk-edge-as-gateway], que fornece uma descrição geral conceptual.

>[!NOTE]
>Atualmente:
> * Se o gateway estiver desconectado do IoT Hub, os dispositivos jusante não é possível autenticar com o gateway.
> * Dispositivos IoT Edge não é possível ligar aos gateways de IoT Edge.
> * Dispositivos jusante não podem utilizar o carregamento de ficheiros.

A parte difícil sobre a criação de um gateway transparente é ligar com segurança o gateway a jusante dispositivos. O Azure IoT Edge permite-lhe utilizar a infraestrutura PKI para configurar ligações seguras de TLS entre estes dispositivos. Neste caso, estamos está a permitir que um dispositivo downstream ligar a um dispositivo IoT Edge que atua como um gateway transparente.  Para manter a segurança razoável, o dispositivo de downstream deve confirmar a identidade do dispositivo de limite, uma vez que apenas pretende que os dispositivos se liguem ao seus gateways e não um gateway potencialmente malicioso.

Pode criar qualquer infraestrutura de certificado que permite a confiança necessária para a sua topologia de gateway de dispositivo. Neste artigo, partimos do princípio a mesma configuração de certificado que pretende utilizar para ativar [segurança de AC X.509] [ lnk-iothub-x509] no IoT Hub, que envolve um certificado X.509 de AC associado a um hub de IoT específico (o IoT hub proprietário da AC ) e uma série de certificados, assinados com esta AC e uma autoridade de certificação para o dispositivo de limite.

![Configuração do gateway][1]

O gateway apresenta o respetivo certificado de AC de dispositivo do Edge no dispositivo jusante durante a inicialização da ligação. O dispositivo de downstream verifica para se certificar de que o certificado de AC de dispositivo do Edge está assinado pelo certificado de AC de proprietário. Este processo permite ao dispositivo downstream confirmar se que o gateway é proveniente de uma origem fidedigna.

Os seguintes passos guiá-lo pelo processo de criação dos certificados e instalá-los nos locais corretos.

## <a name="prerequisites"></a>Pré-requisitos
1.  Instale o runtime do Azure IoT Edge num dispositivo Linux que pretende utilizar como o gateway transparente.
   * [Linux x64][lnk-install-linux-x64]
   * [ARM32 do Linux][lnk-install-linux-arm]

2.  Obter os scripts para gerar os certificados necessários de não produção com o seguinte comando. Estes scripts ajudar a criar os certificados necessários para configurar um gateway transparente. 

   ```cmd
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Estes scripts utilizam OpenSSL para gerar os certificados necessários e OpenSSL requer alguma configuração.
   
   1. Navegue para o diretório no qual pretende trabalhar. Daqui em faremos referência a este como $WRKDIR.  Todos os ficheiros serão criados neste diretório.

      CD $WRKDIR
   
   1. Copie ficheiros de configuração e script no seu diretório de trabalho.

      ```cmd
      cp azure-iot-sdk-c/tools/CACertificates/*.cnf .
      cp azure-iot-sdk-c/tools/CACertificates/certGen.sh .
      chmod 700 certGen.sh 
      ```

## <a name="certificate-creation"></a>Criação do certificado
1.  Crie o certificado de AC de proprietário e um certificado intermédio. Eles são todos colocados no `$WRKDIR`.

   ```cmd
   ./certGen.sh create_root_and_intermediate
   ```

   As saídas da execução do script são as seguintes chaves e certificados:
   * Certificados
      * `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
      * `$WRKDIR/certs/azure-iot-test-only.intermediate.cert.pem`
   * Chaves
      * `$WRKDIR/private/azure-iot-test-only.root.ca.key.pem`
      * `$WRKDIR/private/azure-iot-test-only.intermediate.key.pem`

2.  Crie o certificado de AC de dispositivo do Edge e a chave privada com o comando abaixo.

   >[!NOTE]
   > **Isso não é possível** utilizar um nome que é o mesmo nome de anfitrião DNS do gateway. Se o fizer, fará com que a certificação de cliente contra esses certificados para efetuar a ativação.

      ```cmd
      ./certGen.sh create_edge_device_certificate "<gateway device name>"
      ```

   As saídas da execução do script são os seguintes certificados e chaves:
   * `$WRKDIR/certs/new-edge-device.*`
   * `$WRKDIR/private/new-edge-device.key.pem`

## <a name="certificate-chain-creation"></a>Criação de cadeia de certificados
Crie uma cadeia de certificados a partir do certificado de AC de proprietário, certificado intermédio e certificado de AC de dispositivo de periferia com o comando abaixo. Colocá-lo num arquivo de cadeia permite que facilmente instale-o no seu dispositivo Edge atuar como gateway transparente.

   ```cmd
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="installation-on-the-gateway"></a>Instalação no gateway
1.  Copie os seguintes ficheiros de $WRKDIR em qualquer lugar no seu dispositivo Edge, faremos referência a ela como $CERTDIR. Se gerar os certificados no seu dispositivo Edge ignore este passo.

   * Certificado de acesso condicional de dispositivo-  `$WRKDIR/certs/new-edge-device-full-chain.cert.pem`
   * Chave privada de AC de dispositivo- `$WRKDIR/private/new-edge-device.key.pem`
   * Proprietário da Califórnia- `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`

2.  Definir o `certificate` propriedades no Daemon de segurança yaml ficheiro de configuração para o caminho onde colocou os ficheiros de certificado e chave.

```yaml
certificates:
  device_ca_cert: "$CERTDIR/certs/new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR/private/new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>Implementar EdgeHub para o gateway
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Esta secção tem que criar uma implementação vazia aparentemente; No entanto Hub do Edge é automatcially adicionada a todas as implementações, mesmo se existirem não existem outros módulos presentes. Hub do Edge é o único módulo, que precisa num dispositivo do Edge-lo a atuar como gateway transparente, para que criar uma implementação vazia é suficiente. 
1. No portal do Azure, navegue para o seu hub IoT.
2. Aceda a **IoT Edge** e selecione o seu dispositivo IoT Edge que pretende utilizar como um gateway.
3. Selecione **Definir Módulos**.
4. Selecione **Seguinte**.
5. Na **especificar rotas** passo, deve ter uma rota envia todas as mensagens de todos os módulos para o IoT Hub. Se não estiver, adicione o seguinte código, em seguida, selecione **seguinte**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. O passo de modelo de revisão, selecione **submeter**.

## <a name="installation-on-the-downstream-device"></a>Instalação do dispositivo a jusante
Um dispositivo de downstream pode ser de qualquer aplicação com o [do Azure IoT device SDK][lnk-devicesdk], como aquele simple descrito na [ligar o dispositivo ao seu hub IoT com .NET] [ lnk-iothub-getstarted]. Um aplicativo de dispositivo downstream deve confiar a **proprietário AC** certificado para validar as ligações de TLS para os dispositivos de gateway. Normalmente, é possível executar este passo de duas formas: ao nível do SO ou (para certos idiomas) ao nível da aplicação.

### <a name="os-level"></a>Nível do SO
Instalar este certificado no arquivo de certificados de sistema operacional irá permitir que todas as aplicações para utilizar o proprietário do certificado de AC como um certificado fidedigno.

* Ubuntu - aqui está um exemplo de como instalar um certificado de AC num host de Ubuntu.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    Deverá ver uma mensagem a indicar "certificados de atualização no /etc/ssl/certs... 1 adicionou, removeu a 0; concluído."

* Windows - aqui está um exemplo de como instalar um certificado de AC num host do Windows.
  * No menu Iniciar, escreva em "Gerir certificados de computador". Isso deve abrir um utilitário chamado `certlm`.
  * Navegue para certificados de computador Local--> certificados de raiz fidedigna--> certificados--> direito clique--> todas as tarefas--> importar para iniciar o Assistente de importação de certificado.
  * Siga os passos conforme indicado e importe o certificado ficheiro $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem.
  * Quando concluída, deverá ver uma mensagem "Importada com êxito".

### <a name="application-level"></a>Nível de aplicativo
Para aplicativos .NET, é possível adicionar o fragmento seguinte para confiar num certificado no formato PEM. Inicializar a variável `certPath` com `$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Se conectar a jusante para o gateway
É preciso inicializar o sdk de dispositivo do IoT Hub com uma cadeia de ligação de referência para o nome de anfitrião do dispositivo de gateway. Isso é feito ao acrescentar o `GatewayHostName` propriedade para a cadeia de ligação do dispositivo. Por exemplo, aqui está uma cadeia de ligação do dispositivo de exemplo para um dispositivo, ao qual podemos acrescentado o `GatewayHostName` propriedade:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Este é um comando de exemplo que testes que tudo o que foi configurado corretamente. Sohuld uma mensagem a indicar "verificada OK".
   >
   >OpenSSL s_client-ligar mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts

## <a name="routing-messages-from-downstream-devices"></a>Encaminhar mensagens a partir de dispositivos de downstream
O runtime do IoT Edge pode encaminhar mensagens enviadas a partir de dispositivos downstream, assim como as mensagens enviadas por módulos. Isto permite-lhe efetuar análises num módulo em execução no gateway antes de enviar quaisquer dados para a cloud. A seguir a rota seria usada para enviar mensagens a partir de um dispositivo de downstream chamado `sensor` para um nome de módulo `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Veja o [artigo de composição do módulo] [lnk--composição do módulo] para obter mais detalhes sobre o roteamento de mensagens.

## <a name="next-steps"></a>Passos seguintes
[Compreender os requisitos e as ferramentas para desenvolver módulos do IoT Edge][lnk-module-dev].

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
