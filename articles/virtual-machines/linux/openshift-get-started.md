---
title: OpenShift na descrição geral do Azure | Microsoft Docs
description: Uma descrição geral de OpenShift no Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: c8e740a66271c88b3abb036867d1760cc9e77607
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944506"
---
# <a name="openshift-in-azure"></a>OpenShift no Azure

OpenShift é uma plataforma de aplicação do contentor aberta e extensível que coloca Docker e Kubernetes para a empresa.  

OpenShift inclui Kubernetes para a gestão e orquestração do contentor. Adiciona as ferramentas de programador e operações-centrada que permitem:

- Desenvolvimento da aplicação rápida.
- Facilitar a implementação e dimensionamento.
- Manutenção de ciclo de vida longo prazo para as equipas e aplicações.

Existem várias versões do OpenShift disponíveis:

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift dedicado

Das quatro versões abordadas neste artigo, apenas dois estão disponíveis para os clientes implementar no Azure: OpenShift origem e OpenShift contentor plataforma.

## <a name="openshift-origin"></a>OpenShift Origin

Origem é um [open source](https://www.openshift.org/) projeto a montante de OpenShift é Comunidade suportada. Origem pode ser instalada em CentOS ou Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

A plataforma do contentor é uma preparada para empresa [versão comercial](https://www.openshift.com) do e suportados pelo Red Hat. Com esta versão, os clientes comprar a elegibilidade para a plataforma de contentor OpenShift necessária e são responsáveis pela gestão de toda a infraestrutura e de instalação.

Porque os clientes "proprietário" a plataforma de toda, estes podem instalá-lo no seu centro de dados no local ou numa nuvem pública (por exemplo, o Azure, AWS ou da Google).

## <a name="openshift-online"></a>OpenShift Online

Online é gerido de Red Hat *multi-inquilino* OpenShift que utiliza a plataforma de contentor. Red Hat gere todos da infraestrutura subjacente (por exemplo, VMs, cluster de OpenShift, redes e armazenamento). 

Com esta versão, o cliente implementa contentores, mas não controla sobre os anfitriões os contentores executam. Uma vez Online é a multi-inquilino, contentores podem estar localizados nos anfitriões VM mesmos como contentores de outros clientes. Custo é por contentor.

## <a name="openshift-dedicated"></a>OpenShift dedicado

Dedicada é uma Red Hat gerida pelo *único inquilino* OpenShift que utiliza a plataforma de contentor. Red Hat gere todos da infraestrutura subjacente (VMs, OpenShift cluster, funcionamento em rede, armazenamento, etc.). O cluster é específico para um cliente e é executado numa nuvem pública (por exemplo, AWS ou da Google, com o Azure futuras do 2018 antecipado). Um cluster inicial inclui quatro nós de aplicação para $48,000 por ano (pagamento adiantado).

## <a name="next-steps"></a>Passos Seguintes

- [Configurar pré-requisitos comuns para OpenShift no Azure](./openshift-prerequisites.md)
- [Implementar OpenShift origem no Azure](./openshift-origin.md)
- [Implementar OpenShift contentor plataforma no Azure](./openshift-container-platform.md)
- [Tarefas de pós-implementação](./openshift-post-deployment.md)
- [Resolver problemas de implementação de OpenShift](./openshift-troubleshooting.md)
