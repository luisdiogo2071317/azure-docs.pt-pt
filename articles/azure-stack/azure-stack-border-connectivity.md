---
title: "Considerações de integração de rede de conectividade para sistemas de pilha do Azure integrado do limite | Microsoft Docs"
description: "Saiba o que pode fazer para planear a conectividade de rede de limite de centro de dados com vários nós do Azure pilha."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 93dd609df90adac2c84ba8c62cf0d18f55a317bb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="border-connectivity"></a>Conectividade de limite 
Planeamento de integração de rede é um pré-requisito importante para implementação de sistemas de pilha do Azure integrado com êxito, operação e gestão. Planeamento de conectividade de limite começa por escolher se deve ou não utilizar encaminhamento dinâmico com protocolo border gateway protocol (BGP). É necessário atribuir um número de sistema autónomo 16 bits BGP (público ou privado) ou utilizar o encaminhamento estático, onde uma rota predefinida estático é atribuída aos dispositivos de limite.

> [!IMPORTANT]
> Na parte superior do comutadores (TOR rack) necessitam de camada 3 uplinks com IPs-to-Point (/ 30 redes) configurado nas interfaces físicas. Não é suportado para utilizar uplinks de camada 2 com comutadores TOR suporte operações de pilha do Azure. 

## <a name="bgp-routing"></a>O encaminhamento de BGP
Utilizar um protocolo de encaminhamento dinâmico como BGP garante que o sistema é sempre conhecimento das alterações de rede e facilita a administração. 

Como é mostrado no diagrama seguinte, publicidade do IP privado espaço no comutador TOR é restringido através de uma lista de prefixo. As listas de prefixo nega as sub-redes IP privadas e aplicá-la como um mapa de rota na ligação entre o TOR e o limite.

O Balanceador de carga de Software (SLB) em execução dentro da solução de pilha do Azure elementos de rede para os dispositivos de TOR, de modo que pode anunciar dinamicamente os endereços VIP.

Para garantir que o tráfego do utilizador imediatamente e transparente recupera a partir da falha, o VPC ou MLAG configurada entre os dispositivos de TOR permite a utilização de ligação de chassi multi de agregação para anfitriões e HSRP ou VRRP que fornece rede redundância para as redes IP.

![O encaminhamento de BGP](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>encaminhamento estático
Utilizar rotas estáticas adiciona mais fixa de configuração para o limite e os dispositivos de TOR. Requer análise de detalhado antes de qualquer alteração. Problemas causados por um erro de configuração podem demorar mais tempo a reversão, consoante as alterações efetuadas. Não é o método de encaminhamento recomendado, mas é suportada.

Para integrar a pilha do Azure no seu ambiente de rede ao utilizar este método, o dispositivo de limite deve ser configurado de com rotas estáticas que apontam para os dispositivos de TOR para o tráfego destinado à rede externa, VIPs públicos.

Os dispositivos de TOR tem de ser configurados com uma rota predefinida estático enviar todo o tráfego para os dispositivos de limite. A única exceção de tráfego para esta regra de espaço para privada que serão bloqueados utilizando uma lista de controlo de acesso aplicadas no TOR à ligação de limite.

Tudo o resto deve ser o mesmo que o primeiro método. O BGP dinâmica encaminhamento irá continuar a ser utilizado dentro de bastidor porque é uma ferramenta essencial para o SLB e outros componentes e não pode ser desativado ou removido.

![encaminhamento estático](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Proxy transparente
Se o seu centro de dados requer que todo o tráfego para utilizar um proxy, tem de configurar um *proxy transparente* processar todo o tráfego de bastidor para processá-lo de acordo com a política, separar o tráfego entre as zonas na sua rede.

> [!IMPORTANT]
> A solução de pilha do Azure não suporta proxies normal web.  

Um proxy transparente (também conhecido como um intercetar, inline ou proxy forçada) interceta a comunicação normal na camada de rede sem necessidade de qualquer configuração de cliente especiais. Os clientes precisam não a ter em consideração a existência de proxy.

![Proxy transparente](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Passos Seguintes
[Integração do DNS](azure-stack-integrate-dns.md)