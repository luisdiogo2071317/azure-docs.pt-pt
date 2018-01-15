---
title: "Conceitos de segurança no serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs"
description: "Descreve conceitos específicos a dispositivos com o IoT Hub e o serviço de aprovisionamento de dispositivos de aprovisionamento de segurança"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: ab2bfff571af659552eef8117de041ca6367ce56
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Conceitos de segurança do serviço de aprovisionamento de dispositivos IoT Hub 

Aprovisionamento de serviço do dispositivos do Hub IoT é um serviço de programa auxiliar do IoT Hub que utiliza para configurar o dispositivo de zero touch aprovisionamento para um hub IoT especificado. Com o serviço de aprovisionamento de dispositivos pode aprovisionar milhões de dispositivos de forma segura e escalável. Este artigo fornece uma descrição geral sobre o *segurança* conceitos envolvidas no aprovisionamento de dispositivos. Este artigo é relevante para todas as pessoas fictícias envolvidas na obtenção de um dispositivo preparado para a implementação.

## <a name="attestation-mechanism"></a>Mecanismo de atestado

O mecanismo de atestado é o método utilizado para confirmar a identidade do dispositivo. O mecanismo de atestado também é relevante para a lista de inscrição, o que indica o serviço de aprovisionamento que método de atestado para utilizar com um dispositivo especificado.

> [!NOTE]
> IoT Hub utiliza "esquema de autenticação" para um conceito semelhante em que o serviço.

Serviço de aprovisionamento de dispositivos suporta duas formas de atestado:
* **Certificados x. 509** com base no fluxo de autenticação de certificado de x. 509 padrão.
* **Os tokens SAS** com base num desafio de nonce utilizando a norma TPM para chaves. Isto requer um TPM no dispositivo físico, mas o serviço espera atestem utilizando a chave de endossamento pelo [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Módulo de hardware de segurança

O módulo de hardware de segurança ou HSM, é utilizado para armazenamento seguro, baseada em hardware de segredos do dispositivo e é a forma mais segura de armazenamento secreta. Certificados x. 509 e SAS tokens podem ser armazenados no HSM. HSMs podem ser utilizadas com ambos os mecanismos de atestado suporta o aprovisionamento.

> [!TIP]
> Recomendamos vivamente que utilizar um HSM com dispositivos para armazenar segredos com segurança nos seus dispositivos.

Segredos do dispositivo também podem ser armazenados no software (memória), mas é um formulário menos seguro do armazenamento de um HSM.

## <a name="trusted-platform-module-tpm"></a>Trusted Platform Module (TPM)

TPM pode fazer referência a um padrão para armazenar as chaves utilizadas para autenticar a plataforma de forma segura, ou podem referir-se para a interface de e/s utilizada para interagir com os módulos implementar padrão. Podem existir TPMs como hardware discreta, integrada hardware, firmware, funções ou baseada em software. Saiba mais sobre [TPMs e TPM atestado](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Serviço de aprovisionamento de dispositivos só suporta TPM 2.0.

### <a name="endorsement-key"></a>Chave de endossamento

A chave de endossamento é uma chave assimétrica contida no interior do TPM que foi gerado internamente ou injetado no tempo de fabrico e é exclusivo para cada TPM. A chave de endossamento não pode ser alterada ou removida. A parte privada da chave de endossamento nunca é libertada fora do TPM, enquanto a parte pública da chave de endossamento é utilizada para reconhecer um TPM genuíno. Saiba mais sobre o [chave de endossamento](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>Chave de raiz de armazenamento

A chave de raiz de armazenamento é armazenada no TPM e é utilizada para proteger chaves TPM criadas por aplicações, para que estas chaves não podem ser utilizadas sem TPM. A chave de raiz de armazenamento é gerada quando a assumir a propriedade do TPM; Quando desmarcar o TPM para que um novo utilizador pode assumir a propriedade, é gerada uma nova chave de raiz de armazenamento. Saiba mais sobre o [chave de raiz de armazenamento](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>Certificados x. 509

A utilização de certificados x. 509 como um mecanismo de atestado é uma excelente forma de escala de produção e simplificar o aprovisionamento de dispositivos. Certificados x. 509 normalmente são dispostos numa cadeia de certificados de confiança no qual cada certificado na cadeia está assinado pela chave privada do certificado superior seguinte e assim sucessivamente, terminar num certificado de raiz autoassinado. Isto estabelece uma cadeia de fidedignidade do certificado de raiz gerado por uma raiz fidedigna autoridade de certificação (AC) para baixo através de cada AC intermediária para o certificado de entidade final instalado num dispositivo de delegado. Para obter mais informações, consulte [autenticação de dispositivo com certificados de AC de x. 509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

A cadeia de certificados representa com frequência algumas hierarquia lógica ou física associada a dispositivos. Por exemplo, um fabricante poderá emitir um certificado de AC de raiz autoassinado, utilizar esse certificado para gerar um certificado de AC intermediária exclusivo para cada fábrica, utilize o certificado de cada fábrica para gerar um certificado de AC intermediária exclusivo para cada de produção a maquinaria de linha e, finalmente, utilizar o certificado de linha de produção para gerar um certificado de dispositivo exclusivo (entidade final) para cada dispositivo fabricado na linha. Para obter mais informações, consulte [compreensão Conceptual a x. 509 de certificados da AC na indústria IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Certificado de raiz

Um certificado de raiz é um certificado x. 509 autoassinado que representa uma autoridade de certificação (AC). É o terminus ou âncora de confiança, da cadeia de certificados. Certificados de raiz podem ser personalizada emitidos por uma organização ou comprados numa autoridade de certificação de raiz. Para obter mais informações, consulte [certificados de AC de x. 509 obter](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). O certificado de raiz pode também ser referido como um certificado de AC de raiz.

### <a name="intermediate-certificate"></a>Intermediária

Um certificado intermediária é um certificado de x. 509 que assinado pelo certificado de raiz (ou outro certificado intermédio com o certificado de raiz na cadeia). O último certificado intermédio numa cadeia é utilizado para assinar o certificado de folha. Um certificado intermediária pode também ser referido como um certificado de AC intermediária.

### <a name="leaf-certificate"></a>Certificado de folha

O certificado de folha ou o certificado de entidade final, identifica o marcador de posição de certificado. Tem o certificado de raiz na respetiva cadeia de certificados, bem como zero ou mais certificados intermediários. O certificado de folha não é utilizado para assinar quaisquer outros certificados. Exclusivamente identifica o dispositivo para o serviço de aprovisionamento e é por vezes referido como o certificado do dispositivo. Durante a autenticação, o dispositivo utiliza a chave privada associada este certificado para responder a uma prova de desafio posse do serviço. Para obter mais informações, consulte [autenticar dispositivos assinados com certificados de AC de x. 509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>Controlar o acesso de dispositivo para o serviço de aprovisionamento com certificados x. 509

O serviço de aprovisionamento expõe dois tipos de entrada de registo que pode utilizar para controlar o acesso para dispositivos que utilizam o mecanismo de atestado de x. 509:  

- [Inscrição individuais](./concepts-service.md#individual-enrollment) entradas estão configuradas com o certificado de dispositivo associado a um dispositivo específico. Estas entradas controlam a inscrição para dispositivos específicos.
- [Grupo de inscrição](./concepts-service.md#enrollment-group) entradas associados a um intermédio específico ou um certificado de AC de raiz. Estas entradas controlam a inscrição para todos os dispositivos que tenham intermediária ou certificado na respetiva cadeia de certificados de raiz. 

Quando um dispositivo se liga ao serviço de aprovisionamento, o serviço dá prioridade à entradas de registo mais específicas sobre entradas de registo inferior específicas. Ou seja, se existir um registo individual para o dispositivo, o serviço de aprovisionamento aplica-se essa entrada. Se não houver nenhuma inscrição individuais para o dispositivo e existe um grupo de inscrição para o primeiro intermediária na cadeia de certificados do dispositivo, o serviço aplica-se essa entrada, e assim sucessivamente se a cadeia para a raiz. O serviço aplica-se a primeira entrada aplicável que encontra, para que:

- Se a entrada de inscrição de primeiro localizar estiver ativada, o serviço Aprovisiona o dispositivo.
- Se a entrada de inscrição de primeiro localizar estiver desativada, o serviço não Aprovisiona o dispositivo.  
- Não se for encontrada nenhuma entrada de inscrição para qualquer um dos certificados na cadeia de certificados do dispositivo, o serviço não Aprovisiona o dispositivo. 

Este mecanismo e a estrutura hierárquica de cadeias de certificados fornece uma poderosa flexibilidade na forma como pode controlar o acesso de dispositivos individuais, bem como para grupos de dispositivos. Por exemplo, imagine cinco dispositivos com o certificado encadeia seguintes: 

- *Dispositivo 1*: certificado de raiz-certificados um -> o certificado de dispositivo 1 >
- *O dispositivo 2*: certificado de raiz-certificados um -> o certificado do dispositivo 2 >
- *Dispositivo 3*: certificado de raiz -> certificados um -> o certificado de dispositivo 3
- *O dispositivo 4*: certificado de raiz -> B -> o certificado do dispositivo 4 do certificado
- *O dispositivo 5*: certificado de raiz -> B -> o certificado do dispositivo 5 do certificado

Inicialmente, pode criar uma entrada de registo único grupo ativada para o certificado de raiz ativar o acesso para todos os cinco dispositivos. Se o certificado B mais tarde ficar comprometido, pode criar uma entrada de grupo desativado inscrição para certificados B impedir que *dispositivo 4* e *dispositivo 5* da inscrição. Se ainda posterior *dispositivo 3* ficar comprometida, pode criar uma entrada de inscrição individuais desativado para o respetivo certificado. Revoga este acesso para *dispositivo 3*, mas ainda permite *dispositivo 1* e *dispositivo 2* inscrever.