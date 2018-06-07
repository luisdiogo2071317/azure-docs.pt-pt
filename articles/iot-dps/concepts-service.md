---
title: Serviço os conceitos do serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs
description: Descreve conceitos aprovisionamento do serviço específicos para dispositivos com DPS e o IoT Hub
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 2908e08e36f41ebb8a154e7c490e5c6719d911be
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628305"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Conceitos do serviço de aprovisionamento de dispositivos IoT Hub

Aprovisionamento de serviço do dispositivos do Hub IoT é um serviço de programa auxiliar do IoT Hub que utiliza para configurar o dispositivo de zero touch aprovisionamento para um hub IoT especificado. Com o serviço de aprovisionamento de dispositivos, pode [aprovisionamento automático](concepts-auto-provisioning.md) milhões de dispositivos de forma segura e escalável.

Aprovisionamento de dispositivos é um processo de dois parte. A primeira parte consiste em estabelecer a ligação inicial entre o dispositivo e a solução de IoT por *registar* o dispositivo. A segunda parte estiver a aplicar o adequado *configuração* para o dispositivo com base nos requisitos específicos da solução. Assim que os dois passos foram concluídos, o dispositivo tiver sido totalmente *aprovisionado*. O Serviço de Aprovisionamento de Dispositivos automatiza os dois passos para fornecer uma experiência totalmente integrada de aprovisionamento do dispositivo.

Este artigo fornece uma descrição geral dos conceitos aprovisionamento mais aplicáveis para gerir o *serviço*. Este artigo é mais relevante para pessoas fictícias envolvidas no [passo de configuração de nuvem](about-iot-dps.md#cloud-setup-step) de preparar para a implementação de um dispositivo.

## <a name="service-operations-endpoint"></a>Ponto final de operações de serviço

O ponto final de operações de serviço é o ponto final para gerir as definições de serviço e manter a lista de inscrição. Este ponto final só é utilizada pelo administrador do serviço; Não é utilizado pelos dispositivos.

## <a name="device-provisioning-endpoint"></a>Ponto final de aprovisionamento de dispositivos

O ponto de final de aprovisionamento de dispositivos é o único ponto final de todos os dispositivos utilizarem para aprovisionamento automático. O URL é igual para todas as instâncias serviço aprovisionamento, para eliminar a necessidade de reflash dispositivos com novas informações de ligação em cenários de cadeia de fornecimento. O [âmbito ID](#id-scope) garante o isolamento de inquilino.

## <a name="linked-iot-hubs"></a>Ligado a hubs IoT

Serviço de aprovisionamento de dispositivos apenas pode aprovisionar dispositivos para os hubs IoT que foram associados ao mesmo. Ligar um IoT hub para o serviço de aprovisionamento de dispositivos fornece as permissões de leitura/escrita do serviço para o registo do IoT hub dispositivo; com a ligação, o serviço de aprovisionamento de dispositivos pode registar um ID de dispositivo e definir a configuração inicial no dispositivo duplo. Podem ser ligados aos hubs IoT em qualquer região do Azure. Pode ligar os hubs de outras subscrições ao seu serviço de aprovisionamento.

## <a name="allocation-policy"></a>Política de alocação

O nível de serviço, a definição que determina a forma como o serviço de aprovisionamento de dispositivos atribui dispositivos a um IoT hub. Existem três políticas de alocação suportadas:
* **Uniformemente ponderado distribuição**: ligados aos hubs IoT são igualmente ter, provavelmente, os dispositivos aprovisionados aos mesmos. A predefinição. Se estiver a aprovisionar dispositivos apenas para um hub IoT, pode manter esta definição.
* **Latência mais baixa**: dispositivos são aprovisionados para um hub IoT com a latência mais baixa para o dispositivo. Se vários hubs IoT ligados iriam fornecer a mesma latência mais baixa, o serviço de aprovisionamento codifica dispositivos desses hubs
* **Configuração estático através da lista de inscrição**: especificação do hub IoT pretendida na lista de inscrição tem prioridade sobre a política de alocação de nível de serviço.

## <a name="enrollment"></a>Inscrição

Uma inscrição é o registo de dispositivos ou grupos de dispositivos que poderão registar através de aprovisionamento automático. O registo de inscrição contém informações sobre o dispositivo ou grupo de dispositivos, incluindo:
- o [mecanismo de atestado](concepts-security.md#attestation-mechanism) utilizado pelo dispositivo
- a configuração pretendida inicial opcional
- pretendida do IoT hub
- o ID de dispositivo pretendidos

Existem dois tipos de inscrições suportados pelo serviço de aprovisionamento de dispositivos:

### <a name="enrollment-group"></a>Grupo de inscrição

Um grupo de inscrição é um grupo de dispositivos que partilham um mecanismo de atestado específico. Todos os dispositivos no grupo de inscrição apresentam os certificados x. 509 que são assinados pela mesma raiz ou AC intermediária. Grupos de inscrição só podem utilizar o mecanismo de atestado de x. 509. O nome do grupo de inscrição e o nome do certificado tem de ser alfanumérico, minúsculas e podem conter hífenes.

> [!TIP]
> Recomendamos que utilize um grupo de inscrição para um grande número de dispositivos que partilham uma configuração desejada inicial ou para dispositivos que todos os mesmo inquilino.

### <a name="individual-enrollment"></a>Inscrição individuais

Uma inscrição individuais é uma entrada para um único dispositivo pode registar. As inscrições individuais podem utilizar certificados de folha de x. 509 ou tokens SAS (a partir de um TPM físico ou virtual) como mecanismos de atestado. O ID de registo de inscrição individuais é alfanuméricos, minúsculas e pode conter hífenes. As inscrições individuais podem ter o ID de dispositivo do hub IoT pretendido especificado.

> [!TIP]
> Recomendamos que utilize inscrições individuais para os dispositivos que requerem configurações iniciais exclusivas ou para dispositivos que só podem autenticar utilizando os tokens SAS através do atestado de TPM.

## <a name="registration"></a>Registo

Um registo é o registo de um dispositivo com êxito ao registar/aprovisionamento para um IoT Hub através do serviço de aprovisionamento de dispositivos. Registos de registo são criados automaticamente; também podem ser eliminadas, mas não podem ser atualizadas.

## <a name="operations"></a>Operações

As operações são a unidade de faturação do serviço de aprovisionamento de dispositivos. Uma operação é a conclusão com êxito de uma instrução para o serviço. As operações incluem o registo de dispositivos - e novos registos -, bem como alterações do lado do serviço, como adicionar e atualizar entradas de listas de inscrição.
