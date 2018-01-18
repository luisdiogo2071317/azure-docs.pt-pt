---
title: "Proteger a implementação de Internet das coisas | Microsoft Docs"
description: "Este artigo fornece detalhes sobre como proteger a implementação de IoT"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: 3c2c5c045ad4786ed656fbaf99373644de244570
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-suite-cipher-suites"></a>Conjuntos de cifras do IoT Suite

Azure IoT Suite suporta os seguintes conjuntos de cifras, por esta ordem.

| Conjunto de cifras | Duração |
| --- | --- |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. 7680 bits RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. 3072 bits RSA) FS |128 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. 7680 bits RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. 3072 bits RSA) FS |128 |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Consulte também
Também pode explorar algumas das outras funcionalidades e capacidades das soluções pré-configuradas do IoT Suite:

* [Descrição geral de solução pré-configurada de manutenção preditiva][lnk-predictive-overview]
* [Perguntas mais frequentes sobre o IoT Suite][lnk-faq]

Pode ler sobre a segurança de IoT Hub na [controlar o acesso ao IoT Hub] [ lnk-devguide-security] no guia de programadores do IoT Hub.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md
