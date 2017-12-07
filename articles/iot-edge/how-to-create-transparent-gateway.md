---
title: Criar um dispositivo de gateway transparente com limite de IoT do Azure | Microsoft Docs
description: "Utilize o limite de IoT do Azure para criar um dispositivo de gateway transparente, que pode processar as informações de vários dispositivos"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/04/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 25f4cea1908a0f9bdf387ddfed5f29e6d19bdd20
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="create-an-iot-edge-device-that-acts-as-a-transparent-gateway---preview"></a>Criar um dispositivo de limite de IoT que atua como um gateway transparente - pré-visualização

Este artigo fornece instruções detalhadas para a utilização de um dispositivo de limite de IoT como um gateway transparente. Para o resto deste artigo, o termo *gateway de IoT* refere-se a um dispositivo de limite de IoT utilizado como um gateway transparente. Para obter mais informações, consulte [de limite de IoT como um dispositivo pode ser utilizado como um gateway][lnk-edge-as-gateway], que fornece uma descrição geral conceptual. 

>[!NOTE]
>Atualmente:
> * Se o gateway está desligado do IoT Hub, os dispositivos a jusante não é possível autenticar com o gateway.
> * Não é possível ligar dispositivos de limite de IoT para gateways de periferia de IoT.

## <a name="understand-the-azure-iot-device-sdk"></a>Compreender os dispositivos IoT do Azure SDK


O hub de limite que está instalado em todos os dispositivos de IoT Edge expõe as primitivas seguintes para dispositivos a jusante:

* mensagens do dispositivo para nuvem e da nuvem para o dispositivo
* métodos diretos
* operações do dispositivo duplo

Atualmente, os dispositivos a jusante não são capazes de utilizar o carregamento de ficheiros ao ligar através de um gateway de IoT.

Quando ligar dispositivos a um gateway de IoT Edge utilizando o SDK de dispositivo do IoT do Azure, tem de:

* Configurar o dispositivo a jusante com uma cadeia de ligação que faz referência ao nome do anfitrião do dispositivo de gateway; e
* Certifique-se de que o dispositivo a jusante confianças o certificado utilizado para aceitar a ligação ao dispositivo de gateway.

Quando instala o tempo de execução de limite de IoT do Azure utilizando o script de controlo, é criar um certificado para o hub de limite, como fez no tutorial [instalar IoT Edge num dispositivo simulado no Windows] [ lnk-tutorial1-win] e [Linux][lnk-tutorial1-lin]. Este certificado é utilizado pelo Edge hub para aceitar ligações a receber TLS e tem de ser fidedigno para o dispositivo a jusante ao ligar ao dispositivo de gateway.

Pode criar qualquer infraestrutura de certificados que permite a fidedignidade necessária para a topologia de gateway do dispositivo. Neste artigo, partimos do pressuposto a mesmo certificado a configuração que pretende utilizar para ativar [segurança de AC de x. 509] [ lnk-iothub-x509] no IoT Hub, que envolve um certificado de AC de x. 509 associado a um IoT hub específico (o *Proprietário do hub IoT AC*) e uma série de certificados, assinada com esta AC, instalado nos dispositivos de limite de IoT.

>[!IMPORTANT]
>Atualmente, os dispositivos de IoT Edge e a jusante só pode utilizar [SAS tokens] [ lnk-iothub-tokens] para autenticar com o IoT Hub. Os certificados serão utilizados apenas para validar a ligação de TLS entre o dispositivo de gateway e de folha.

A configuração utiliza **proprietário do hub IoT AC** como ambos:
* Um certificado de assinatura para o programa de configuração do runtime IoT Edge em todos os dispositivos de IoT Edge; e
* um certificado de chave pública instalado nos dispositivos a jusante.

Isto resulta numa solução que permite que todos os dispositivos utilizar qualquer dispositivo de limite de IoT como um gateway, desde que estão ligados ao mesmo IoT hub.

## <a name="create-the-certificates-for-test-scenarios"></a>Criar os certificados para cenários de teste

Pode utilizar o exemplo do Powershell e scripts de Bash descritas [gerir exemplo de certificado de AC] [ lnk-ca-scripts] para gerar um autoassinado **proprietário do hub IoT AC** e certificados de dispositivo sessão iniciada com o mesmo.

>[!IMPORTANT]
>Este exemplo destinam-se apenas a fins de teste. Para cenários de produção, consulte [proteger a implementação de IoT] [ lnk-iothub-secure-deployment] para as Azure IoT diretrizes sobre como proteger a sua solução de IoT e aprovisionar o certificado em conformidade.


1. Clone [SDKs do IoT do Microsoft Azure e bibliotecas para C] a partir do GitHub:

   ```
   git clone -b modules-preview https://github.com/Azure/azure-iot-sdk-c.git 
   ```

2. Para instalar os scripts de certificado, siga as instruções em **passo 1 – configuração inicial** de [gerir exemplo de certificado de AC][lnk-ca-scripts]. 
3. Para gerar o **proprietário do hub IoT AC**, siga as instruções em **passo 2 - criar a cadeia de certificados**. Este ficheiro é utilizado pelos dispositivos a jusante para validar a ligação.
4. Para gerar um certificado para o dispositivo de gateway, utilize instruções Bash ou do PowerShell:

### <a name="bash"></a>Bash

Crie o novo certificado de dispositivo:

   ```
   ./certGen.sh create_edge_device_certificate myGateway
   ```

São criados novos ficheiros:.\certs\new-edge-device.* contém a chave pública e o PFX e.\private\new-edge-device.key.pem contém a chave privada do dispositivo.
 
No `certs` diretório, execute o seguinte comando para obter a cadeia completa de chave pública do dispositivo:

   ```
   cat ./new-edge-device.cert.pem ./azure-iot-test-only.intermediate.cert.pem ./azure-iot-test-only.root.ca.cert.pem > ./new-edge-device-full-chain.cert.pem
   ```

### <a name="powershell"></a>PowerShell

Crie o novo certificado de dispositivo: 
   ```
   New-CACertsEdgeDevice myGateway
   ```

São criados novos ficheiros de myEdgeDevice *, que contém a chave pública, a chave privada e o PFX deste certificado. 

Quando lhe for pedido para introduzir uma palavra-passe durante o processo de assinatura, introduza "1234".

## <a name="configure-a-gateway-device"></a>Configurar um dispositivo de gateway

Para configurar o seu dispositivo IoT Edge como um gateway apenas terá de configurar para utilizar o certificado de dispositivo que criou na secção anterior.

Partimos do pressuposto os seguintes nomes de ficheiro dos scripts de exemplo acima:

| Saída | Scripts de bash | PowerShell |
| ------ | ----------- | ---------- |
| Certificado do dispositivo | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Chave privada do dispositivo | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Cadeia de certificados de dispositivo | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| Proprietário do hub IoT AC | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

Forneça as informações de dispositivo e o certificado para o tempo de execução do limite de IoT. 
 
No Linux, utilizando a saída de Bash:

   ```
   sudo iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem
   ```

No Windows, utilizando a saída do PowerShell:

   ```
   iotedgectl setup --connection-string {device connection string}
        --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
        --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
        --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
        --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
        --owner-ca-cert-file {full path}/RootCA.pem
   ```

Por predefinição os scripts de exemplo não defina uma frase de acesso para a chave privada do dispositivo. Se definir uma frase de acesso, adicione o seguinte parâmetro:

   ```
   --device-ca-passphrase {passphrase}
   ```

O script pede-lhe definir uma frase de acesso para o certificado de agente de limite. Reinicie o tempo de execução do limite de IoT após este comando:

   ```
   iotedgectl restart
   ```

## <a name="configure-a-downstream-device"></a>Configurar um dispositivo a jusante

Um dispositivo a jusante pode ser qualquer aplicação utilizando o [dispositivos IoT do Azure SDK][lnk-devicesdk], tal como aquele simple descrito em [ligar o seu dispositivo ao seu IoT hub através do .NET] [ lnk-iothub-getstarted].

Em primeiro lugar, tem de confiança de uma aplicação de dispositivo a jusante a **proprietário do hub IoT AC** certificado para validar as ligações de TLS para os dispositivos de gateway. Normalmente, é possível executar este passo de duas formas: ao nível do SO ou (para alguns idiomas) ao nível da aplicação.

Por exemplo, para aplicações de .NET, pode adicionar o seguinte fragmento de um certificado no formato PEM armazenado no caminho de confiança `certPath`. Consoante a versão do script tiver utilizado, o caminho referencia `certs/azure-iot-test-only.root.ca.cert.pem` (Bash) ou `RootCA.pem` (Powershell).

   ```
   using System.Security.Cryptography.X509Certificates;
   
   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

Executar este passo ao nível do SO é diferente entre o Windows e entre as distribuições do Linux.

O segundo passo é ao inicializar o sdk do dispositivo IoT Hub com uma cadeia de ligação que faça referência a nome de anfitrião do dispositivo de gateway.
Isto é feito, acrescentando o `GatewayHostName` propriedade para a cadeia de ligação do dispositivo. Por exemplo, aqui está uma cadeia de ligação do dispositivo de exemplo para um dispositivo, é acrescentado a `GatewayHostName` propriedade:

   ```
   HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com
   ```

Estes dois passos ativam a aplicação de dispositivo para ligar ao dispositivo de gateway.

## <a name="next-steps"></a>Passos seguintes
[Compreender os requisitos e ferramentas para o desenvolvimento de módulos de limite de IoT][lnk-module-dev].

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
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/modules-preview/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus