---
title: Rede Virtual do Azure para contentores | Microsoft Docs
description: Saiba mais sobre CNI Plug-in para Kubernetes clusters, que permite contentores comunicar com entre si e outros recursos, numa rede virtual.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: jdial
ms.openlocfilehash: f70afa8ff69b6c79363313c0f2df3b6785da8d81
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="container-networking"></a>Redes de contentor

O Azure oferece uma [Plug-in de Interface de rede de contentor (CNI)](https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md) que lhe permite implementar e gerir o seu próprio cluster Kubernetes, com capacidade de rede do Azure nativa. O plug-in estiver ativado, por predefinição, quando implementar Kubernetes clusters com o [motor do serviço de contentor do Azure](https://github.com/Azure/acs-engine) (ou o motor de ACS).

## <a name="networking-capabilities"></a>Capacidades de rede

Contentores podem utilizar ao conjunto avançado de capacidades que oferece uma rede virtual, tais como:
-   Pode criar uma rede virtual separada para o seu cluster ou implementar o cluster de uma rede virtual existente. 
-   Cada pod no cluster recebe um endereço IP a partir de dentro da rede virtual e pode comunicar diretamente com outros pods no cluster e qualquer máquina virtual na rede virtual. 
-   Um pod pode ligar outros pods e máquinas virtuais em redes virtuais em modo de peering e redes no local, através de ligações de VPN de site a site e ExpressRoute. Recursos no local podem comunicar com pods. 
-   Pode expor um serviço Kubernetes à Internet através do Balanceador de carga do Azure.  
-   Pods numa sub-rede que tem pontos finais de serviço ativados podem ligar de forma segura aos serviços do Azure (armazenamento e a SQL Database, por exemplo).
-   Pode utilizar rotas definidas pelo utilizador para encaminhar o tráfego da pods para uma aplicação virtual de rede. 
-   Pods podem aceder a recursos públicos na Internet.
-   Pode atribuir um pod um endereço IP público, que pode ser associado com um nome DNS.
 
## <a name="limits"></a>Limites
Pode implementar até 4 000 nós num Kubernetes cluster e até 250 pods por nó, com um limite geral de 16,000 pods por cluster, ao utilizar o plug-in.

## <a name="constraints"></a>Restrições
- O plug-in não está ativado quando implementar um cluster de Kubernetes com o [serviço de contentor do Azure (AKS)](../aks/intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [ACS](../container-service/kubernetes/container-service-intro-kubernetes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cluster com Kubernetes.
- Não há nenhum suporte nativo para políticas de rede Kubernetes, incluindo políticas de acesso ou DNS.
- O plug-in não suporta as políticas de rede por pod.

## <a name="pricing"></a>Preços
Não há sem qualquer encargo para utilizar o plug-in CNI.

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar um cluster de Kubernetes](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/deploy.md) no seu [própria rede virtual](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/features.md#using-azure-integrated-networking-cni).
