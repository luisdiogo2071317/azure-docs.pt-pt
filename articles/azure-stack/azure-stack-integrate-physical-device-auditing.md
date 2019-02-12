---
title: Auditoria e dispositivo físico do Azure Stack
description: Saiba como integrar a auditoria de acesso de dispositivo físico no Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 02/11/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 02/11/2019
keywords: ''
ms.openlocfilehash: c04ec1213d48179e203a8bef47f304fd23203afe
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098984"
---
# <a name="azure-stack-datacenter-integration---physical-device-auditing"></a>Integração de datacenter do Azure Stack - a auditoria do dispositivo físico

Todos os dispositivos físicos no Azure Stack, como os controladores de gestão da placa base (BMCs) e comutadores de rede, emitem os registos de auditoria. Pode integrar os registos de auditoria a solução global de auditoria. Uma vez que os dispositivos variam entre os diferentes fornecedores de hardware de OEM de pilha do Azure, contacte o fornecedor para a documentação sobre a integração de auditoria.
As seções abaixo fornecem algumas informações gerais para auditoria do dispositivo físico no Azure Stack.  

## <a name="physical-device-access-auditing"></a>Auditoria de acesso do dispositivo físico

Todos os dispositivos físicos no Azure Stack suportam a utilização de TACACS ou RADIUS. O suporte inclui acesso ao controlador de gestão da placa base (BMC) e comutadores de rede.

Soluções de pilha do Azure não são enviados com RADIUS ou TACACS incorporado. No entanto, as soluções foram validadas para suportar a utilização de RADIUS ou TACACS soluções existentes disponíveis no mercado.

Para RADIUS, MSCHAPv2 foi validada. Isso representa a implementação mais segura com o RADIUS.
Consulte o fornecedor de hardware de OEM para ativar o TACAS ou RADIUS nos dispositivos incluídos com a sua solução do Azure Stack.

## <a name="syslog-forwarding-for-network-devices"></a>Reencaminhamento do syslog para dispositivos de rede

Todos os dispositivos de rede físicos no Azure Stack suportam mensagens syslog. Soluções de pilha do Azure não são enviados com um servidor syslog. No entanto, os dispositivos foram validados para suportar o envio de mensagens para soluções de syslog existentes disponíveis no mercado.

O endereço de destino do syslog é um parâmetro opcional recolhido para a implementação, mas ele também pode ser adicionado após a implementação. Consulte o fornecedor de hardware de OEM para configurar o reencaminhamento nos seus dispositivos de rede do syslog.

## <a name="next-steps"></a>Passos Seguintes

[Política de manutenção](azure-stack-servicing-policy.md)
