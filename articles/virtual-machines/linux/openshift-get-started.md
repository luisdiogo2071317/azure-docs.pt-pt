---
title: OpenShift na descrição geral do Azure | Documentos da Microsoft
description: Uma visão geral do OpenShift no Azure.
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
ms.openlocfilehash: e3ab060c1cea28f83c18dc89aeea7716ec86572a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190348"
---
# <a name="openshift-in-azure"></a>OpenShift no Azure

OpenShift é uma plataforma de aplicação de contentor aberta e extensível que fornece o Docker e Kubernetes para a empresa.  

OpenShift inclui Kubernetes para orquestração de contentores e gestão. Ele adiciona e operações-centrado no desenvolvedor ferramentas que permitem:

- Desenvolvimento rápido de aplicativos.
- Facilitar a implementação e dimensionamento.
- Manutenção de ciclo de vida longa duração para equipes e aplicativos.

Há várias versões do OpenShift disponíveis:

- OKD (anteriormente conhecido como o OpenShift Origin)
- OpenShift Container Platform
- OpenShift Online
- OpenShift dedicado

Das quatro versões abordadas neste artigo, apenas dois estão disponível para os clientes implementam no Azure: OpenShift Origin e OpenShift Container Platform.

## <a name="okd-formerly-openshift-origin"></a>OKD (anteriormente conhecido como o OpenShift Origin)

OKD é um [aberto](https://www.okd.io/) projeto a montante do OpenShift suportada na Comunidade. OKD pode ser instalado no CentOS ou Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Plataforma de contentores é uma prontas para empresas [versão comercial](https://www.openshift.com) partir e suportados pelo Red Hat. Com esta versão, os clientes comprar as elegibilidades necessárias para a plataforma de contentores do OpenShift e são responsáveis pela instalação e gestão de toda a infraestrutura.

Uma vez que os clientes "proprietário" a plataforma de toda, eles podem instalá-lo em datacenters no local ou numa nuvem pública (por exemplo, o Azure, AWS ou o Google).

## <a name="openshift-online"></a>OpenShift Online

Online é gerida de Red Hat *multi-inquilino* OpenShift que utiliza a plataforma de contentores. Red Hat gerencia todos da infraestrutura subjacente (por exemplo, VMs, cluster de OpenShift, funcionamento em rede e armazenamento). 

Com esta versão, o cliente implementa contentores, mas não tem controle sobre os anfitriões que executam os contentores. Uma vez Online é a multi-inquilino, os contentores podem estar localizados nos anfitriões VM mesmo como contentores de outros clientes. Custo é por contentor.

## <a name="openshift-dedicated"></a>OpenShift dedicado

É dedicada, gerida de Red Hat *inquilino único* OpenShift que utiliza a plataforma de contentores. Red Hat gerencia todos da infraestrutura subjacente (VMs, cluster do OpenShift, rede, armazenamento, etc.). O cluster é específico para um cliente e é executado numa nuvem pública (por exemplo, o AWS ou o Google, com o Azure fornecidos no início de 2018). Um cluster inicial inclui quatro nós de aplicação para US $48.000 por ano (pago antecipadamente).

## <a name="next-steps"></a>Passos Seguintes

- [Configurar a pré-requisitos comuns para OpenShift no Azure](./openshift-prerequisites.md)
- [Implementar o OpenShift Origin no Azure](./openshift-origin.md)
- [Implementar o OpenShift Container Platform no Azure](./openshift-container-platform.md)
- [Tarefas de pós-implementação](./openshift-post-deployment.md)
- [Resolver problemas de implementação do OpenShift](./openshift-troubleshooting.md)
