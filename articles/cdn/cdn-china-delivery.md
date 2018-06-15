---
title: Entrega de conteúdos China com o Azure CDN | Microsoft Docs
description: Saiba como utilizar a rede de entrega de conteúdos (CDN) do Azure para fornecer conteúdos aos utilizadores China.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 59788f301bb501103babd55a2ac37102932f4dcf
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260229"
---
# <a name="china-content-delivery-with-azure-cdn"></a>Entrega de conteúdos China com CDN do Azure

Azure entrega rede conteúdos (CDN) global pode servir conteúdo aos utilizadores China com as localizações de ponto de presença (POP) junto da China ou qualquer POP que fornece o melhor desempenho para pedidos provenientes China. No entanto, se China é um mercado significativo para os seus clientes e precisam de desempenho rápido, considere utilizar o Azure CDN China em vez disso.

Azure CDN China difere do Azure CDN global em que oferece conteúdo de POPs dentro China por partnering com um número de fornecedores locais. Devido à conformidade chinês e regulamento, tem de registar uma subscrição separada para utilizar o Azure CDN China e os Web sites tem de ter uma licença do ICP. O portal e a API de experiência para ativar e gerir conteúdo entrega é idêntica entre global a CDN do Azure e Azure CDN China.

## <a name="comparison-of-azure-cdn-global-and-azure-cdn-china"></a>Comparação do Azure CDN global e o Azure CDN China

Azure CDN global e o Azure CDN China têm as seguintes funcionalidades:

- CDN do Azure é global:

     - Portal: https://portal.azure.com  

     - Efetua a entrega de conteúdos fora da China

     - Os escalões de preços quatro: padrão da Microsoft, da Verizon standard, premium da Verizon e standard da Akamai

     - [Documentação](https://docs.microsoft.com/en-us/azure/cdn/)

- China CDN do Azure:

     - Portal: https://portal.azure.cn

     - Efetua a entrega de conteúdos no interior da China

     - Dois escalões de preços: Standard e premium

     - [Documentação](https://docs.azure.cn/en-us/cdn/)
 

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o Azure CDN China, consulte:

- [Funcionalidades de rede de entrega de conteúdo](https://www.azure.cn/en-us/home/features/cdn/)

- [Descrição geral da rede de entrega de conteúdos do Azure](https://docs.azure.cn/en-us/cdn/cdn-overview)

- [Utilizar a rede de entrega de conteúdos do Azure](https://docs.azure.cn/en-us/cdn/cdn-how-to-use)

- [Disponibilidade do serviço do Azure na China](https://docs.microsoft.com/en-us/azure/china/china-get-started-service-availability)



