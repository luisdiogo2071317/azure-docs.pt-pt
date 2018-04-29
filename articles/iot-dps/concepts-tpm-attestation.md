---
title: Serviço - atestado TPM de aprovisionamento de dispositivos de IoT Hub do Azure
description: Este artigo fornece uma descrição geral conceptual do fluxo de atestado TPM utilizando o serviço de aprovisionamento de dispositivos de IoT.
services: iot-dps
keywords: ''
author: nberdy
ms.author: nberdy
ms.date: 04/23/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: dec024c5c23bf8c628457127af57b8d18800660e
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tpm-attestation"></a>Atestado de TPM

Aprovisionamento de serviço do dispositivos do Hub IoT é um serviço de programa auxiliar do IoT Hub que utiliza para configurar o dispositivo de zero touch aprovisionamento para um hub IoT especificado. Com o serviço de aprovisionamento de dispositivos, pode aprovisionar milhões de dispositivos de forma segura.

Este artigo descreve o processo de atestado de identidade ao utilizar um [TPM](./concepts-device.md). TPM representa Trusted Platform Module e é um tipo de módulo de hardware de segurança (HSM). Este artigo pressupõe que está a utilizar um firmware discreto, ou integrada TPM. Software TPMs emulados são ideais para fazer o protótipo ou de teste, mas não fornecer o mesmo nível de segurança como discreta, firmware TPMs integradas fazer. Não é recomendada a utilização de software TPMs na produção. [Saiba mais](http://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf) sobre os tipos de TPMs.

Este artigo só é relevante para dispositivos com o TPM 2.0 com suporte de chave HMAC e as respetivas chaves de endossamento. Não é para dispositivos com certificados x. 509 para autenticação. TPM é um toda a indústria, a norma ISO do grupo de computação fidedigna e pode ler mais sobre TPM no [concluída TPM 2.0 spec](https://trustedcomputinggroup.org/tpm-library-specification/) ou [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html). Este artigo também assume que está familiarizado com os pares de chaves públicos e privados e como são utilizados para a encriptação.

O dispositivo de serviço de aprovisionamento de dispositivos SDKs processam tudo o que é descrito neste artigo para si. Não é necessário para que possa implementar nada adicional, se estiver a utilizar os SDKs nos seus dispositivos. Este artigo ajuda-o a compreender, essencialmente, que se passa com o chip de segurança TPM quando Aprovisiona o dispositivo e por que motivo é, para proteger.

## <a name="overview"></a>Descrição geral

TPMs utilizam algo chamado a chave de endossamento (EK) como a raiz de confiança segura. O EK é exclusivo no TPM e alterá-lo, essencialmente, altera o dispositivo para um novo.

Não há outro tipo de chave que TPMs tiverem, denominada a chave de raiz de armazenamento (SRK). Pode ser gerado um SRK pelo proprietário do TPM após demora a propriedade do TPM. A propriedade de TPM é a forma de TPM específicos da indicar "alguém define uma palavra-passe no HSM." Se um dispositivo TPM é vendido a um novo proprietário, o novo proprietário pode assumir a propriedade do TPM para gerar um novo SRK. A nova geração de SRK assegura que o proprietário anterior não é possível utilizar o TPM. Porque o SRK é exclusivo para o proprietário do TPM, o SRK pode ser utilizado para selar dados no TPM para esse proprietário. O SRK fornece uma sandbox para o proprietário armazenar as respetivas chaves e fornece acesso revocability se o dispositivo ou TPM é vendido. É como mover para uma novo próxima: a propriedade é alterar os bloqueios nas portas e destroying furniture todos os restantes pelos proprietários anteriores (SRK), mas não é possível alterar o endereço da próxima (EK).

Depois de um dispositivo foi configurado e pronto a utilizar, pode ter um EK e um SRK disponível para utilização.

![A propriedade de um TPM](./media/concepts-tpm-attestation/tpm-ownership.png)

Uma nota na propriedade do TPM: A propriedade de um TPM depende inúmeros aspetos, incluindo o fabricante TPM, o conjunto de ferramentas TPM a ser utilizado e o SO do dispositivo. Siga as instruções relevantes para o sistema para obter a propriedade.

O serviço de aprovisionamento de dispositivos utiliza a parte pública do EK (EK_pub) para identificar e inscrever dispositivos. Fabricante do dispositivo pode ler o EK_pub durante fabrico ou teste final e carregar o EK_pub para o serviço de aprovisionamento para que o dispositivo será possível reconhecer quando este se liga a aprovisionar. O serviço de aprovisionamento de dispositivos não verifique o SRK ou proprietário, pelo que "Limpar" o TPM apague dados de cliente, mas o EK (e outros dados de fornecedor) são preservados e o dispositivo ainda ser reconhecido pelo serviço de aprovisionamento de dispositivos para estabelecer a ligação para aprovisionar.

## <a name="detailed-attestation-process"></a>Processo de atestado de detalhado

Quando um dispositivo com um TPM é ligado pela primeira vez para o serviço de aprovisionamento de dispositivos, o serviço verifica primeiro a EK_pub fornecido contra EK_pub armazenados na lista de inscrição. Se o EK_pubs não corresponderem, o dispositivo não é permitido para aprovisionar. Se o EK_pubs correspondem, o serviço, em seguida, requer que o dispositivo para provar a propriedade da parte privada EK através de um desafio de nonce, que é um desafio seguro utilizado para provar a identidade. O serviço de aprovisionamento de dispositivos gera um nonce e, em seguida, encripta-a com a SRK e, em seguida, EK_pub, que são fornecidos pelo dispositivo durante a chamada de registo inicial. O TPM sempre mantém a parte privada o EK segura. Isto impede a counterfeiting e assegura a tokens SAS de forma segura são aprovisionados para dispositivos autorizados.

Vamos guiá-lo durante o processo de atestado em detalhe.

### <a name="device-requests-an-iot-hub-assignment"></a>Dispositivo solicitar uma atribuição de IoT Hub

Primeiro o dispositivo estabelece ligação com o serviço de aprovisionamento de dispositivos e os pedidos para aprovisionar. Ao fazê-lo, o dispositivo fornece o serviço com o ID de registo, um âmbito de ID e a EK_pub e SRK_pub do TPM. O serviço transmite o nonce encriptado novamente para o dispositivo e pede-lhe o dispositivo para desencriptar o nonce e utilize-a para assinar um token SAS para estabelecer ligação novamente e concluir o aprovisionamento.

![Pedidos de aprovisionamento de dispositivos](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Desafio nonce

O dispositivo demora o nonce e utiliza as partes do EK e SRK privadas para desencriptar o nonce no TPM; a ordem de fidedignidade de delegados de encriptação de nonce da EK, que é imutável, para SRK, que pode alterar se um novo proprietário é proprietários do TPM.

![Desencriptar o nonce](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Validar o nonce e receber credenciais

O dispositivo, em seguida, pode iniciar um token SAS, utilizando o nonce desencriptado e restabelecer uma ligação para o serviço de aprovisionamento de dispositivos utilizando o token SAS assinado. Com o desafio de Nonce concluído, o serviço permite ao dispositivo para aprovisionar.

![Dispositivo reestablishes ligação DPS para validar a propriedade EK](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Passos Seguintes

Agora, o dispositivo estabelece ligação ao IoT Hub e rest Proteja o conhecimento de que as chaves dos seus dispositivos estão armazenadas em segurança. Agora que já sabe como o serviço de aprovisionamento de dispositivos verifica em segurança a identidade de um dispositivo utilizar TPM, consulte os seguintes artigos para saber mais:

* [Saiba mais sobre todos os conceitos no aprovisionamento automático](./concepts-auto-provisioning.md)
* [Começar a utilizar o aprovisionamento automático](./quick-setup-auto-provision.md) utilizar os SDKs para asseguramos o fluxo.
