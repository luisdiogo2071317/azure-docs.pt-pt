---
title: Recursos e plataformas suportadas pelo centro de segurança do Azure | Documentos da Microsoft
description: Este documento fornece uma lista de recursos e plataformas suportadas pelo centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: f4bc90b2d1a80125ae88b4b5c4c11e42a34a985a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240431"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Plataformas e funcionalidades suportadas pelo centro de segurança do Azure

Monitorização de estado de segurança e recomendações, estão disponíveis para máquinas virtuais (VMs), criadas usando o clássicas e modelos de implementação do Resource Manager e computadores.

> [!NOTE]
> Saiba mais sobre o [clássico e modelos de implementação do Resource Manager](../azure-classic-rm.md) para recursos do Azure.
>
>

## <a name="supported-platforms"></a>Plataformas suportadas 

Esta secção lista as plataformas em que pode executar o agente do Centro de segurança do Azure e de que esta possa recolher dados.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas suportadas para VMs e computadores do Windows
Sistemas de operativos Windows suportados:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas suportadas para VMs e computadores Linux
Sistemas operativos Linux suportados:

* Versões do Ubuntu 12.04 LTS, 14.04 LTS e 16.04 LTS
* Debian versões 6, 7, 8, 9
* CentOS versões 5, 6, 7
* Versões de Red Hat Enterprise Linux (RHEL) 5, 6, 7
* SUSE Linux Enterprise Server (SLES) versões 11, 12
* Oracle Linux versões 5, 6, 7
* Amazon Linux 2012.09 por meio de 2017
* OpenSSL 1.1.0 só é suportado em plataformas de x86_64 (64-bit)

> [!NOTE]
> Análise comportamental de máquinas virtuais ainda não está disponível para sistemas operativos Linux.
>
>

## <a name="vms-and-cloud-services"></a>VMs e serviços Cloud
As VMs em execução num serviço cloud também são suportadas. Funções web e de trabalho em execução na produção são monitorizadas ranhuras de serviços de cloud única. Para saber mais sobre o serviço em nuvem, veja [descrição geral dos serviços de Cloud](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Recursos de IaaS suportados

> [!div class="mx-tableFixed"]
> 

|Servidor|Windows||Linux||
|----|----|----|----|----|
|Ambiente|Azure|Não Pertencente ao Azure|Azure|Não Pertencente ao Azure|
|Alertas de deteção de ameaças VMBA|✔|✔|✔ (em versões suportadas)|✔|
|Alertas de deteção de ameaças de rede com base em|✔|X|✔|X|
|Integração do Windows Defender ATP *|✔ (em versões suportadas)|✔|X|X|
|Patches em falta|✔|✔|✔|✔|
|Configurações de segurança|✔|✔|✔|✔|
|Antimalware|✔|✔|X|X|
|Acesso JIT da VM|✔|X|✔|X|
|Controlos de aplicações adaptáveis|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Encriptação de disco|✔|X|✔|X|
|Implantação de terceiros|✔|X|✔|X|
|NSGs|✔|X|✔|X|
|Deteção de ameaças Filess|✔|✔|X|X|
|Mapa de rede|✔|X|✔|X|
|Controlos de rede adaptável|✔|X|✔|X|

\* Estas funcionalidades são suportadas atualmente em pré-visualização pública.


## <a name="supported-paas-features"></a>Recursos de PaaS suportados


|Serviço|Recomendações|Deteção de ameaças|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL *|✔| ✔|
|MySQL *|✔| ✔|
|Blob storage contas *|✔| ✔|
|Serviços aplicacionais|✔| ✔|
|Serviços em nuvem|✔| X|
|Vnets|✔| ND|
|Sub-redes|✔| ND|
|NICs|✔| ✔|
|NSGs|✔| ND|
|Subscrição|✔| ✔|

\* Estas funcionalidades são suportadas atualmente em pré-visualização pública.

## <a name="next-steps"></a>Passos Seguintes

- [Guia de operações e planeamento do Centro de segurança do Azure](security-center-planning-and-operations-guide.md) – Saiba como planear e compreender as considerações de conceção para adoção do Centro de segurança do Azure
- [Alertas de segurança por tipo no Centro de segurança do Azure](security-center-alerts-type.md#virtual-machine-behavioral-analysis) - Saiba mais sobre a análise comportamental de máquinas virtuais e de falha de análise de informação de memória no Centro de segurança
- [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço
- [Blogue de segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) – encontre mensagens do Blogue acerca da segurança do Azure e de conformidade
