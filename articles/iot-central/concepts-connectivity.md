---
title: Conectividade do dispositivo no Azure IoT Central | Documentos da Microsoft
description: Este artigo apresenta os principais conceitos relacionados com a conectividade do dispositivo no Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: d2ed07be829e48cc4fc0538c08fd498dea99e71e
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985180"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Conectividade do dispositivo no Azure IoT Central

Este artigo apresenta os principais conceitos relacionados com a conectividade do dispositivo no Microsoft Azure IoT Central.

Utiliza o Azure IoT Central [serviço aprovisionamento de dispositivos do Azure IoT Hub (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps), ativar o Centro de IoT suportar a integração e a ligação dos dispositivos à escala.

-   Os clientes podem agora gerar credenciais de dispositivos e configurar os dispositivos offline sem ter de registar primeiro os dispositivos no IoT Central
-   Centro de IoT suporta a ligação de dispositivo com o setor recomendado X509 cert com base em conectividade, enquanto continua a suportar e melhorar a conectividade de assinaturas de acesso partilhado (SAS)
-   Os clientes do Centro de IoT agora podem levar seus próprios identificações de dispositivo para registar dispositivos no IoT Central, ativar a integração simple com os sistemas de back office existentes
-   Há uma maneira consistente para ligar dispositivos ao IoT Central 

>[!NOTE]
>Centro de IoT utiliza o serviço de aprovisionamento de dispositivos de IoT do Azure (DPS) abaixo para todos os registo de dispositivos e a ligação, [Saiba mais sobre o DPS](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Com base no seu siga de casos de utilização, as instruções para ligar dispositivos ao IoT Central
1. [Ligue-se de um único dispositivo rapidamente (utilizar assinaturas de acesso partilhado)](#connect-a-single-device)
1. [Ligar dispositivos em escala com assinaturas de acesso partilhado (SAS)](#connect-devices-at-scale-using-shared-access-signatures)
1. [Ligar dispositivos em escala com X509 certificados](#connect-devices-using-x509-certificates) **recomendado para cargas de trabalho de produção**
1. [Ligar-se sem o registo de dispositivos primeiro](#connect-without-first-registering-devices) 


>[!NOTE]
>Aqui está o ponto final global para os dispositivos para se ligar e aprovisionar **global.azure-dispositivos-provisioning.net**.

## <a name="connect-a-single-device"></a>Ligar um dispositivo único
Ligar um único dispositivo ao IoT Central através da SAS é fácil e demora apenas alguns passos 
1. Adicionar uma **dispositivo real** do Device Explorer, clique em **+ novo > Real** para adicionar um dispositivo real.
    * Introduza o Id de dispositivo **<span style="color:Red">(deve ser minúscula)</span>** ou utilize o ID de dispositivo sugerido.
    * Introduza o nome do dispositivo ou utilize o nome sugerido   
    ![Adicionar Dispositivo](media\concepts-connectivity\add-device.png)
1. Obter os detalhes de ligação, tal como **chave de ID de âmbito, o ID de dispositivo e o primário** para o dispositivo foi adicionado ao clicar em **Connect** na página do dispositivo.
    * **[Definir âmbito de ID](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**  é por aplicações de IoT Central e é gerado por pontos de distribuição, usados para garantir que o ID de dispositivo exclusivo dentro de uma aplicação.
    * **Id de dispositivo** é o ID por aplicação, o dispositivo tem de enviar o Id do dispositivo como parte da chamada de registo de dispositivo exclusivo.   
    * **Chave primária** é um token SAS gerado pelo centro de IoT para esse dispositivo específico. 
    ![Detalhes da ligação](media\concepts-connectivity\device-connect.PNG)
1. Utilize estes detalhes de ligação **identidade de dispositivo, o nome do dispositivo e a chave primária do dispositivo** em seu código de dispositivo para aprovisionar e ligar o seu dispositivo e começar a ver os dados fluem instantaneamente. Se estiver a utilizar a siga de dispositivo de MxChip [aqui as instruções passo a passo](howto-connect-devkit.md#add-a-real-device), começar na secção **preparar o dispositivo DevKit**.   

    Seguem-se as referências para outros idiomas, que pode querer utilizar.

    *   **Linguagem C:** se estiver a utilizar o C, siga [este cliente de dispositivo de exemplo do C](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) para ligar um dispositivo de exemplo. Utilize as seguintes definições no exemplo.   

         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
         
         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";

         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```

    *   **NODE. js:** se pretender utilizar o node. js [utilizar as instruções passo a passo aqui](tutorial-add-device.md#prepare-the-client-code), iniciar a partir da secção **preparar o código de cliente**.



## <a name="connect-devices-at-scale-using-shared-access-signatures"></a>Ligar dispositivos em escala com assinaturas de acesso partilhado

Para ligar dispositivos à escala com o IoT Central através da SAS, há duas etapas envolvidas 
1. **Registar dispositivos** importando-os no Centro de IoT através de um CSV de ficheiros e exportar os dispositivos com detalhes de ligação do dispositivo para utilizar para ligar os seus dispositivos
1. **Configuração do dispositivo** o dispositivo é programado com os detalhes de ligação ( **chave de ID de âmbito, o ID de dispositivo e o principal**), permitindo que ele chama o serviço de aprovisionamento para obter a sua ligação informações/IoT atribuição de aplicações Central quando é ativado.

>[!NOTE]
>Uma opção avançada também está disponível em que pode ligar dispositivos sem ter primeiro registrar dispositivos no IoT Central, [Saiba mais aqui](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

**Registar dispositivos**

Para ligar o grande número de dispositivos à sua aplicação, o Azure IoT Central ofertas em massa importando dispositivos através de um ficheiro CSV. 

Requisitos de ficheiro CSV: ficheiro CSV o deve ter as seguintes colunas (e cabeçalhos)
1.  IOTC_DeviceID  **<span style="color:Red">(deve ser em minúsculas)</span>**
1.  IOTC_DeviceName (opcional)



Importar dispositivos para registá-los na sua aplicação
1.  Escolher **Explorer** no menu de navegação esquerdo.
1.  No painel esquerdo, selecione o modelo de dispositivo para o qual pretende em massa criar os dispositivos. 
1.  Clique em **importação**, selecione o ficheiro CSV que tem a lista de identificações de dispositivo para ser importado.
O ficheiro CSV deve ter as seguintes colunas (e cabeçalhos)
    *   IOTC_DeviceID  **<span style="color:Red">(deve ser em minúsculas)</span>**
    *   IOTC_DeviceName (opcional)
1.  Assim que a importação estiver concluída, é apresentada uma mensagem de êxito na grade de dispositivo.

Exportar dispositivos para obter os detalhes de ligação, a exportação cria um ficheiro CSV com o Id de dispositivo, nome do dispositivo e a chave de dispositivo. Utilize estes detalhes para ligar o dispositivo ao IoT Central.
Em massa dispositivos de exportação da sua aplicação:
1.  Escolher **Explorer** no menu de navegação esquerdo.
1.  Selecione os dispositivos que pretende exportar e, em seguida, clique nas **exportar** ação.
1.  Quando a exportação for concluída, é apresentada uma mensagem de êxito, juntamente com uma ligação para transferir o ficheiro gerado.
1.  Clique na mensagem de sucesso para transferir o ficheiro numa pasta local no disco.
1.  O ficheiro CSV exportado terá as seguintes informações de colunas: **thumbrpints de certificado do Id de dispositivo, nome do dispositivo, as chaves de Priamary/Secondar do dispositivo e primária/secundária**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY


**Configuração do dispositivo**

Utilize estes detalhes de ligação **identidade de dispositivo (IOTC_DEVICEID), a chave primária de dispositivo (IOTC_SASKEY_PRIMARY) e o ID de âmbito** em seu código de dispositivo para aprovisionar e ligar o seu dispositivo. Se ainda não tiver, obter o **Id de âmbito** da sua aplicação de Central de IoT **administração > ligação do dispositivo > ID de âmbito**.
Se estiver a utilizar o **MxChip** dispositivo ligar-se siga [aqui as instruções passo a passo](howto-connect-devkit.md#add-a-real-device), começar na secção **preparar o dispositivo DevKit**.   

Seguem-se as referências para outros idiomas, que pode querer utilizar.

   *   **Linguagem C:** se estiver a utilizar o C siga [este cliente de dispositivo de exemplo do C](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) para ligar um dispositivo de exemplo. Utilize as seguintes definições no exemplo.   
         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";
         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```
    * **NODE. js:** se pretender utilizar o node. js [utilizar as instruções passo a passo aqui](tutorial-add-device.md#prepare-the-client-code), iniciar a partir da secção **preparar o código de cliente**.


## <a name="connect-devices-using-x509-certificates"></a>Ligar dispositivos através do X509 certificados

Usar X.509 certificados como um mecanismo de atestado é uma excelente forma de dimensionar **produção** e simplificar o aprovisionamento de dispositivos. Certificados X.509, normalmente, são organizados numa cadeia de certificados de confiança na qual cada certificado na cadeia está assinado pela chave privada do certificado superior seguinte e assim por diante, terminar num certificado de raiz autoassinado. Isto estabelece uma cadeia de delegados de confiança a partir do certificado de raiz gerado por uma raiz fidedigna autoridade de certificação (AC) para baixo por meio de cada AC intermediária para o certificado de "folha" de entidade final instalado num dispositivo. Para obter mais informações, consulte [autenticação de dispositivo usando certificados de AC X.509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Para ligar dispositivos ao IoT Central usando X509 certificados, aqui estão as três etapas principais envolvidas 
1. **Configurar as definições de ligação** na aplicação Centro de IoT ao adicionar/verificar o certificado de raiz/intermédio utilizado para gerar os certificados de dispositivo de X509.  Existem dois passos para configurar definições de ligação para X509 certificados.  

    *   **Adicionar X509 certificado raiz ou intermediário** estiver a utilizar para gerar os certificados de dispositivos de folha. Aceda a administração > ligação do dispositivo > certificados. 
    
        ![Definições de ligação](media\concepts-connectivity\connection-settings.PNG)
    *   **Verificação de certificado:** verificar a propriedade do certificado garante que o carregador do certificado está na posse da chave privada do certificado. Para verificar o certificado
        *  Gerar código de verificação, clique no botão junto ao campo de código de verificação para gerar o código de verificação. 
        *  Crie um certificado de verificação de X.509 com o código de verificação, guarde o certificado como um ficheiro. cer. 
        *  Carregue o certificado de verificação assinado e clique em verificar.

        ![Definições de ligação](media\concepts-connectivity\verify-cert.png)
    *   **Certificado secundário:** durante o ciclo de vida da sua solução de IoT, terá de implementar certificados. Duas das principais razões para implementar certificados seria uma violação de segurança e expirações de certificado. Certificados secundários são usados para reduzir o período de indisponibilidade para dispositivos tentar aprovisionar enquanto está a atualizar o certificado primário.

    **PARA FINS DE TESTE APENAS** 
    
    Seguem-se algumas ferramentas de linha de comandos do utilitário que pode utilizar para gerar certificados de AC e certificados de dispositivo.

    * Se estiver a utilizar o MxChip aqui é um [ferramenta de linha de comandos](http://aka.ms/iotcentral-docs-dicetool) para gerar a AC certificados adicioná-lo à sua aplicação do Centro de IoT e verifique se os certificados. 

    *   Utilize esta opção [ferramenta de linha de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para
        * Crie a cadeia de certificados (siga passo 2 em documentos do GitHub). 
         Guardar os certificados como ficheiros. cer e carregar para o IoT Central (primário).   
        * Obter o código de verificação a partir da aplicação de Central de IoT e gerar o certificado (siga passo 3 em documentos do GitHub) para carregar para verificar. 
        * Crie certificados de folha com o seu Id de dispositivo como um parâmetro para a ferramenta (siga o passo 4). Guarde o certificado e utilizá-lo no seu dispositivo.     

1. **Registar dispositivos** importando-os no Centro de IoT através de um ficheiro CSV.

1. **Configuração do dispositivo** : gerar os certificados de folha a utilizar o certificado de raiz carregado. Certifique-se de que utiliza o **ID do dispositivo** como o CNAME na folha de certificados e está a ser **minúsculas**. Aqui está uma [ferramenta de linha de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para gerar certificados de folha/dispositivo para **fins de teste apenas**.

    O dispositivo com informações de serviço ativá-la obter detalhes de ligação e o IoT Central quando ligado na atribuição de aplicações de aprovisionamento do programa.    

    **Referene adicional** 
    *   Implementação de exemplo para [RaspberryPi.](http://aka.ms/iotcentral-docs-Raspi-releases)  

    *   [Cliente de dispositivo de exemplo em C.](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

>[!NOTE]
>Utilize o **ID de dispositivo** como um cname ao gerar os certificados de folha para dispositivos.

>[!NOTE]
>O **ID de dispositivo** deve estar em minúsculas 
 
## <a name="connect-without-first-registering-devices"></a>Ligar-se sem o registo de dispositivos primeiro
Um dos principais cenários que IOT Central permite é para OEMs para dispositivos em massa de fabrico, gerar as credenciais e configurá-las na fábrica de sem a necessidade de primeiro registá-los no Centro de IoT. Depois dos dispositivos estão ativados e estabelecer ligação ao IoT Central o operador aprova ao dispositivo ligar-se para a aplicação do Centro de IoT.

Segue-se o fluxo para ligar os dispositivos com esta funcionalidade

![Definições de ligação](media\concepts-connectivity\device-connection-flow.PNG)


Siga os passos com base na sua escolha de esquema de autenticação do dispositivo (X509/SAS)

1. **Definições de ligação** 
    * **X509 certificados:** [adicionar e verificar o certificado de raiz/intermédio](#connect-devices-using-x509-certificates) e utilizá-lo para gerar os certificados de dispositivo no próximo passo.
    * **SAS:** copie a chave primária (esta chave é a chave SAS de grupo para esta aplicação do Centro de IoT) e utilizá-lo para gerar as chaves SAS do dispositivo no próximo passo. 
![Definições de ligação SAS](media\concepts-connectivity\connection-settings-sas.png)

1. **Gerar credenciais de dispositivo** 
    *   **Certificados X509:** gerar os certificados de folha para os seus dispositivos com o certificado de raiz/intermédio adicionou a esta aplicação. Certifique-se de que utiliza a **ID do dispositivo** como um cname nos certificados de folha e  **<span style="color:Red">(deve ser minúscula)</span>**. Aqui está uma [ferramenta de linha de comandos](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) para gerar certificados de folha/dispositivo para fins de teste.
    *   **SAS** chaves SAS de dispositivo podem ser geradas através desta [ferramenta de linha de comando](https://www.npmjs.com/package/dps-keygen). Utilize a chave de SAS primário (chave SAS de grupo) do passo anterior. Certificar-se de que o ID do dispositivo  **<span style="color:Red">está em minúsculas</span>**.

        Utilize as instruções para gerar a chave SAS de dispositivo abaixo           

        ```
        npm i -g dps-keygen
        ```
    
        **Utilização**
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Configuração do dispositivo** 
    
     Flash o dispositivo com o **ID de âmbito, ID de dispositivo, a chave SAS/certificado do dispositivo** e, em seguida, ativar o dispositivo para ligar à aplicação do Centro de IoT.

1. **Ligar dispositivo ao IoT Central:** uma vez ativado os dispositivos se ligam a pontos de distribuição/IoT Central para o registo.

1. **Associar o dispositivo a um modelo:** no dispositivo ligado serão apresentados no **dispositivos não associados** no **Device Explorer**. O estado de aprovisionamento de dispositivos é **registado**. **Associar** o dispositivo para o modelo de dispositivo apropriado e aprovar o dispositivo para ligar à aplicação IoT Central. O dispositivo obtém os detalhes de ligações para a aplicação do Centro de IoT, estabelece ligação e começa a enviar dados. Dispositivo provioning está agora concluída e o *estado de aprovisionamento* passa a **aprovisionado**.

## <a name="device-provisioning-status"></a>Estado de aprovisionamento do dispositivo
Há uma série de etapas envolvidas quando um dispositivo real está ligado ao Azure IoT Central 
1. **Registado**: O dispositivo é a primeiro **registada**, que significa que o dispositivo é criado no Centro de IoT e tem o ID de dispositivo para o dispositivo.
O dispositivo está Registeretd quando  
    *   Um dispositivo real novo for adicionado no **Explorer**
    *   Um conjunto de dispositivo é adicionado usando **importação** no **Explorer**
    *   Um dispositivo que não foi registado mas liga-se com credenciais válidas e está visível sob **não associados** dispositivos. 

    Em todos os casos acima a *estado de aprovisionamento* é **registado**

1. **Aprovisionado**: A próxima etapa é quando o dispositivo estabelece ligação com credenciais válidas IoT Central é concluído o passo de aprovisionamento (ao criar o dispositivo no IoT Hub). Em seguida, ele retorna a cadeia de ligação para o dispositivo para se ligar e começar a enviar dados. O dispositivo *estado de aprovisionamento* agora ativa a partir de **registado** para **aprovisionado**.

1.  **Bloqueado**: O operador pode bloquear um dispositivo, uma vez que um dispositivo está bloqueado, não é possível enviar dados para o IoT Central e terão de ser reposto. Dispositivos que estão bloqueados têm o *estado de aprovisionamento* dos **bloqueado**. O operador também pode desbloquear o dispositivo. Uma vez desbloqueado do dispositivo *estado de aprovisionamento* volte ao seu anterior *estado de aprovisionamento* (registado ou aprovisionado). 

## <a name="getting-device-connection-string"></a>Obter cadeia de ligação do dispositivo
Pode obter cadeia de ligação do Iot hub dispositivo hub IoT do Azure utilizando os passos seguintes 
1. Obter os detalhes de ligação, como **ID de âmbito, ID de dispositivo, chave primária do dispositivo** da página de dispositivo (tem de página do dispositivo > clicar em Connect) 

    ![Detalhes da ligação](media\concepts-connectivity\device-connect.PNG)

1. Obter a cadeia de ligação do dispositivo com a ferramenta de linha de commnd abaixo.
    Utilize as instruções para obter a cadeia de ligação do dispositivo abaixo  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Utilização**

    Para criar uma cadeia de ligação, localize o binário em bin / pasta
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Saiba mais sobre o [dps-keygen ferramenta aqui.](https://www.npmjs.com/package/dps-keygen)

## <a name="sdk-support"></a>Suporte do SDK

A oferta de SDKs de dispositivo do Azure a forma mais fácil para implementar o código nos seus dispositivos que se liga a sua aplicação do Azure IoT Central. Os SDKs do dispositivo seguintes estão disponíveis:

- [SDK do Azure IoT para C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT SDK para Python](https://github.com/azure/azure-iot-sdk-python)
- [SDK do Azure IoT para node. js](https://github.com/azure/azure-iot-sdk-node)
- [SDK do IoT do Azure para Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT SDK para .NET](https://github.com/azure/azure-iot-sdk-csharp)

Cada dispositivo liga-se através de uma cadeia de ligação exclusivo que identifica o dispositivo. Só pode ligar um dispositivo ao hub IoT em que está registado. Quando cria um dispositivo real na sua aplicação do Azure IoT Central, o aplicativo gera uma cadeia de ligação para que possa utilizar.

## <a name="sdk-features-and-iot-hub-connectivity"></a>Funcionalidades do SDK e conectividade do IoT Hub

Todas as comunicações de dispositivos com o IoT Hub utiliza as seguintes opções de conectividade do IoT Hub:

- [Mensagens do dispositivo para a cloud](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Dispositivos duplos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

A tabela seguinte resume como o mapeamento de funcionalidades do dispositivo Azure IoT Central, para funcionalidades do IoT Hub:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Medida: telemetria | Mensagens do dispositivo para a cloud |
| Propriedades do dispositivo | Propriedades comunicadas do dispositivo duplo |
| Definições | Dispositivo duplo desejado e propriedades comunicadas |

Para saber mais sobre como utilizar os SDKs de dispositivo, veja um dos seguintes artigos, por exemplo de código:

- [Ligar um cliente Node.js genérico à aplicação do Azure IoT Central](howto-connect-nodejs.md)
- [Ligar um dispositivo de Raspberry Pi a sua aplicação do Azure IoT Central](howto-connect-raspberry-pi-python.md)
- [Ligar um dispositivo de kit de representante à sua aplicação do Azure IoT Central](howto-connect-devkit.md).


## <a name="protocols"></a>Protocolos

Os SDKs de dispositivo suporta os seguintes protocolos de rede para ligar a um hub IoT:

- MQTT
- AMQP
- HTTPS

Para obter informações sobre esses protocolos de diferença e orientações sobre como escolher um, consulte [escolher um protocolo de comunicação](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Se o seu dispositivo não é possível utilizar qualquer um dos protocolos suportados, pode utilizar o Azure IoT Edge para a conversão de protocolo. IoT Edge suporta outros cenários de inteligência no edge para a descarga de processamento na periferia da aplicação do Azure IoT Central.

## <a name="security"></a>Segurança

Todos os dados trocados entre dispositivos e o Azure IoT Central são encriptados. IoT Hub autentica todos os pedidos de um dispositivo que se liga a qualquer um dos pontos finais com acesso de dispositivo IoT Hub. Para evitar a troca de credenciais durante a transmissão, um dispositivo utiliza tokens assinados para autenticar. Para obter mais informações, consulte, [controlar o acesso ao IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Atualmente, os dispositivos que estabelecem ligação ao Azure IoT Central tem de utilizar SAS tokens. Certificados X.509 não são suportados para dispositivos que estabelecem ligação ao Azure IoT Central.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu sobre a conectividade do dispositivo no Azure IoT Central, aqui estão os passos sugeridos seguintes:

- [Preparar e ligar um dispositivo de DevKit](howto-connect-devkit.md)
- [Preparar e ligar um Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Ligar um cliente Node.js genérico à aplicação do Azure IoT Central](howto-connect-nodejs.md)
