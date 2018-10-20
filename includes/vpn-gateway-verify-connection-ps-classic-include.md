---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 03dc6d8bb95a952a77be31f79df36a2c1ddc8ffc
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458644"
---
Pode verificar se a ligação teve êxito com o cmdlet 'Get-AzureVNetConnection'.

1. Utilize o seguinte exemplo de cmdlet, configurando os valores para corresponder aos seus. O nome da rede virtual tem de ser aspas se contiver espaços.

  ```azurepowershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Quando o cmdlet terminar, veja os valores. No exemplo abaixo, o estado de conectividade é apresentado como "Ligado" e pode ver bytes de entrada e saída.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal