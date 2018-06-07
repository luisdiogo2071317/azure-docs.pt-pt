---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 244d6be318662be794cac58aaa8350b433b6cb37
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825464"
---
Não associe uma tabela de rota que inclui uma rota com um destino 0.0.0.0/0 para a sub-rede do gateway. Se o fizer, impede que o gateway de funcionar corretamente.