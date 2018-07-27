---
title: Considerações de integração de rede de conectividade para os sistemas integrados do Azure Stack dos limites | Documentos da Microsoft
description: Saiba o que pode fazer para planear a conectividade de rede de limite de centro de dados com vários nós do Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 260c58ad9099a4532c8a6558cfcf5c13f0fc8d52
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282013"
---
# <a name="border-connectivity"></a>Conectividade de limite 
Planeamento de integração de rede é um pré-requisito importante para a implementação de sistemas integrados do Azure Stack, operação e gerenciamento com êxito. O planejamento de conectividade de limite começa escolhendo se deve ou não utilizam o encaminhamento dinâmico com o protocolo BGP (BGP). Isso requer a atribuição de um número de sistema autónomo de BGP 16 bits (público ou privado) ou utilizar o encaminhamento estático, onde uma rota predefinida estático é atribuída para os dispositivos de limite.

> [!IMPORTANT]
> Parte superior do comutadores (TOR rack) requerem uplinks de camada 3 com IPs de ponto a ponto (/ 30 redes) configurado nas interfaces físicas. Não é suportado para utilizar a camada 2 uplinks com comutadores TOR suporte a operações do Azure Stack. 

## <a name="bgp-routing"></a>O encaminhamento de BGP
Utilizar um protocolo de encaminhamento dinâmico com BGP garante que seu sistema estará sempre ciente das alterações de rede e facilita a administração. 

Conforme mostrado no diagrama seguinte, de publicidade de IP privado espaço no comutador TOR é limitado por uma lista de prefixos. As listas de prefixo nega as sub-redes IP privadas e aplicá-lo como um mapa de rota na ligação entre o TOR e a borda.

O Balanceador de carga de Software (SLB) em execução dentro da solução do Azure Stack elementos para os dispositivos de TOR, para que ele dinamicamente pode anunciar os endereços VIP.

Para garantir que o tráfego de utilizador imediatamente e transparente recupera de uma falha, o VPC ou MLAG configurada entre os dispositivos de TOR permite a utilização da ligação de chassi multi de agregação para os anfitriões e HSRP ou VRRP que fornece a redundância de rede para as redes IP.

![O encaminhamento de BGP](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>Encaminhamento estático
Encaminhamento estático requer configuração adicional para os dispositivos de limite. Ele requer intervenção manual mais e gestão, bem como a análise detalhada antes de qualquer alteração e problemas causados por um erro de configuração pode demorar mais tempo para reversão dependendo das alterações feitas. Não é o método de encaminhamento recomendado, mas é suportada.

Para integrar o Azure Stack no seu ambiente de rede com o encaminhamento estático, todas as ligações físicas quatro entre o limite e o dispositivo de TOR tem de estar ligadas e elevada disponibilidade não pode ser garantida devido a estática como funciona o roteamento.

O dispositivo de limite deve ser configurado com rotas estáticas que aponta para os dispositivos de TOR P2P para o tráfego destinado à rede externa ou VIPs públicas e a rede de infraestrutura. Ele requer rotas estáticas para a rede BMC para a implementação. Os clientes podem optar por deixar o rotas estáticas na borda para acessar alguns recursos que residem na rede BMC.  Adicionar rotas estáticas para *infraestrutura comutador* e *comutador gestão* redes é opcional.

Os dispositivos de TOR são configurados com uma rota padrão estática enviar todo o tráfego para os dispositivos de limite. É a única exceção de tráfego para a regra predefinida para o espaço privado, o que for bloqueado com uma lista de controlo de acesso aplicadas no TOR a ligação de limite.

Encaminhamento estático só se aplica a uplinks entre os comutadores TOR e o limite. Encaminhamento de BGP dinâmico é utilizado dentro de rack porque ele é uma ferramenta essencial para o SLB e outros componentes e não pode ser desativado ou removido.

![Encaminhamento estático](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Proxy transparente
Se o seu datacenter requer que todo o tráfego para utilizar um proxy, tem de configurar uma *proxy transparente* para processar todo o tráfego de rack para manipulá-lo de acordo com a política, separar o tráfego entre as zonas na sua rede.

> [!IMPORTANT]
> A solução do Azure Stack não suporta proxies normal web.  

Um proxy transparente (também conhecido como uma interceptação, inline ou proxy forçado) intercepta comunicação normal na camada da rede sem a necessidade de qualquer configuração de cliente especial. Os clientes não precisam estar atento a existência do proxy.

![Proxy transparente](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Passos Seguintes
[Integração do DNS](azure-stack-integrate-dns.md)