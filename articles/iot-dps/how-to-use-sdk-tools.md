---
title: "Utilizar ferramentas fornecidas no Azure dispositivo aprovisionamento serviço SDKs do IoT Hub para simplificar o desenvolvimento"
description: "Este documento revê as ferramentas fornecidas no SDKs de serviço aprovisionamento a dispositivo do Azure IoT Hub para o desenvolvimento"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 01/18/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 76c6f64dea202f661691fafaa78a6d77b4a40f14
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-use-tools-provided-in-the-sdks-to-simplify-development-for-provisioning"></a>Como utilizar ferramentas fornecidas nos SDKs para simplificar o desenvolvimento para o aprovisionamento
O serviço de aprovisionamento de dispositivos do IoT Hub simplifica o processo de aprovisionamento com zero touch, just-in-time o aprovisionamento de forma segura e escalável.  É necessário o atestado de segurança no formato de certificado x. 509 ou Trusted Platform Module (TPM).  Microsoft também é partnering com [restantes parceiros de hardware de segurança](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) para melhorar a confiança na proteger a implementação de IoT. Noções sobre o requisito de segurança de hardware pode ser bastante difícil para os programadores. Um conjunto de SDKs Service de aprovisionamento do IoT do Azure são fornecidos para que os programadores podem utilizar uma camada de conveniência para clientes de escrita que comuniquem com o serviço de aprovisionamento. Os SDKs também fornecem exemplos para cenários comuns, bem como um conjunto de ferramentas para simplificar o atestado de segurança de desenvolvimento.

## <a name="trusted-platform-module-tpm-simulator"></a>Fidedigna simulador de Platform Module (TPM)
[TPM](https://docs.microsoft.com/azure/iot-dps/concepts-security#trusted-platform-module-tpm) pode fazer referência a um padrão para armazenar as chaves para a plataforma de autenticar de forma segura, ou podem referir-se para a interface de e/s utilizada para interagir com os módulos implementar padrão. Podem existir TPMs como hardware discreta, integrada hardware, firmware, funções ou baseada em software.  Na produção, TPM está localizado no dispositivo, como hardware discreta, hardware integrada, baseado no firmware ou. Na fase de teste, é fornecido um simulador TPM com base em software para os programadores.  Este simulador só está disponível para o desenvolvimento na plataforma do Windows por agora.

Passos para utilizar o simulador TPM são:
1. [Preparar o ambiente de desenvolvimento](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) e clonar o repositório do GitHub:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Navegue para a pasta do simulador TPM em ```azure-iot-sdk-java/provisioning/provisioning-tool/tpm-simulator/```.
3. Execute Simulator.exe antes de executar qualquer aplicação cliente do dispositivo de aprovisionamento.
4. Permitir que o simulador executado em segundo plano em todo o processo de aprovisionamento para obter o ID de registo e a chave de endossamento.  Ambos os valores só são válidos para uma instância de execução.

## <a name="x509-certificate-generator"></a>Gerador de certificado x. 509
[Certificados x. 509](https://docs.microsoft.com/azure/iot-dps/concepts-security#x509-certificates) pode ser utilizado como um mecanismo de atestado para produção de escala e simplificar o aprovisionamento de dispositivos.  Existem [várias formas](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) para obter um certificado x. 509:
* Para o ambiente de produção, recomendamos a compra de um certificado de AC de x. 509 de uma autoridade de certificação de raiz pública.
* Para o ambiente de teste, pode gerar um certificado de raiz de x. 509 ou a cadeia de certificados de x. 509 através de:
    * OpenSSL: Isto [como guia](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-create-certificates) explica scripts do PowerShell de exemplo que utilizam [OpenSSL](https://www.openssl.org/) para criar e assinar certificados x. 509.  Além disso, também pode utilizar scripts noutros idiomas para a geração de certificado:
        * [Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools)
        * [PowerShell](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md)
        
    * Emulador de motor de composição de identidade (REPARTIR) do dispositivo: REPARTIR pode ser utilizado para a identidade de dispositivo de criptografia e atestado com base no protocolo TLS e x. 509 certificados de cliente.  [Saiba](https://www.microsoft.com/research/publication/device-identity-dice-riot-keys-certificates/) mais informações sobre a identidade de dispositivo com REPARTIR.

### <a name="using-x509-certificate-generator-with-dice-emulator"></a>Utilizar o gerador de certificado x. 509 com o emulador de REPARTIR
Os SDKs fornecem um gerador de certificado x. 509 com o emulador de REPARTIR, localizado no [Java SDK](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-tools/provisioning-x509-cert-generator).  Este gerador funciona em diferentes plataformas.  O certificado gerado pode ser utilizado para o desenvolvimento em outros idiomas.

Atualmente, enquanto o emulador de REPARTIR produz um certificado de raiz, um certificado intermediária, um certificado de folha e chave privada associada.  No entanto, o certificado de raiz ou intermediária não pode ser utilizada para assinar um certificado de folha separado.  Se pretender testar o cenário de inscrição de grupo em que um certificado de assinatura é utilizado para assinar os certificados de folha de vários dispositivos, pode utilizar OpenSSL para produzir uma cadeia de certificados.

Para gerar o certificado x. 509 utilizando este gerador:
1. [Preparar o ambiente de desenvolvimento](https://docs.microsoft.com/azure/iot-dps/quick-enroll-device-x509-java#prepare-the-development-environment) e clonar o repositório do GitHub:
```
git clone https://github.com/Azure/azure-iot-sdk-java.git
```
2. Altere a raiz para o azure-iot-sdk-java.
3. Executar ```mvn install -DskipTests=true``` transferir pacotes todos requeridos e compile o SDK
4. Navegue para a raiz para o gerador de certificado x. 509 no ```azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator```.
5. Criar com o```mvn clean install```
6. Execute a ferramenta com os comandos seguintes:
```
cd target
java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
```
7. Quando lhe for pedido, pode introduzir opcionalmente um _Nome Comum_ para os certificados.
8. A ferramenta localmente gera um **certificados de cliente**, a **chave privada do certificado de cliente**, **certificados intermédios**e o **certificados de raiz**.

**Certificado de cliente** é o certificado de folha no dispositivo.  **Certificado de cliente** e associada **chave privada do certificado de cliente** são necessárias no cliente do dispositivo. Consoante o idioma que escolher, o mecanismo de coloque isto na aplicação cliente poderão ser diferente.  Para obter mais informações, consulte o [inícios Rápidos](https://docs.microsoft.com/azure/iot-dps/quick-create-simulated-device-x509) criar no dispositivo simulado com x. 509 para obter mais informações.

O certificado de raiz ou intermédio pode ser utilizado para criar um grupo de inscrição ou inscrição individuais [programaticamente](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments-sdks) ou utilizando o [portal](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

## <a name="next-steps"></a>Passos Seguintes
* Desenvolver com o [SDK do Azure IoT]( https://github.com/Azure/azure-iot-sdks) para o IoT Hub do Azure e o serviço de aprovisionamento de dispositivos do Azure IoT Hub