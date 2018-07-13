---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1c3103889bd11df45710cc0d6afaeeba022c9ace
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38763010"
---
Pode verificar se a sua ligação foi concluída com êxito com o comando [az network vpn-connection show](/cli/azure/network/vpn-connection#show). No exemplo, '--name' refere-se ao nome da ligação que pretende testar. Quando a ligação ainda estiver a ser estabelecida, o respetivo estado de ligação mostra "A ligar". Quando a ligação estiver estabelecido, o estado muda para "Ligado".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```