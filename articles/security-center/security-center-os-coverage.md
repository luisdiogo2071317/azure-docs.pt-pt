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
ms.openlocfilehash: 4108355415d1230f98db36a4f83497de2fa848f7
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53185584"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Plataformas e funcionalidades suportadas pelo centro de segurança do Azure

Monitorização de estado de segurança e recomendações, estão disponíveis para máquinas virtuais (VMs), criadas usando o clássico e modelos de implementação do Resource Manager e computadores.

> [!NOTE]
> Saiba mais sobre o [clássico e modelos de implementação do Resource Manager](../azure-classic-rm.md) para recursos do Azure.
>
>

## <a name="supported-platforms"></a>Plataformas suportadas 

Esta secção lista as plataformas em que pode executar o agente do Centro de segurança do Azure e de que esta possa recolher dados.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas suportadas para VMs e computadores do Windows
São suportados os seguintes sistemas operativos Windows:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas suportadas para VMs e computadores Linux
São suportados os seguintes sistemas operativos Linux:

* Versões de Ubuntu 12.04 LTS, 14.04 LTS e 16.04 LTS.
* Debian versões 6, 7, 8 e 9.
* CentOS versões 5, 6 e 7.
* Versões do Red Hat Enterprise Linux (RHEL) 5, 6 e 7.
* SUSE Linux Enterprise Server (SLES) versões 11 e 12.
* Versões de Linux do Oracle 5, 6 e 7.
* Amazon Linux 2012.09 por meio de 2017.
* OpenSSL 1.1.0 só é suportado em plataformas de x86_64, 64 bits.

> [!NOTE]
> Análise comportamental de máquinas virtuais ainda não está disponível para sistemas operativos Linux.
>
>

## <a name="vms-and-cloud-services"></a>VMs e serviços Cloud
VMs que são executadas num serviço cloud também são suportadas. Apenas funções na nuvem serviços web e de trabalho que são executados nas ranhuras de produção são monitorizadas. Para saber mais sobre os serviços cloud, veja [descrição geral dos serviços Cloud do Azure](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Recursos de IaaS suportados

> [!div class="mx-tableFixed"]
> 

|Servidor|Windows||Linux||
|----|----|----|----|----|
|Ambiente|Azure|Não Pertencente ao Azure|Azure|Não Pertencente ao Azure|
|Alertas de deteção de ameaças VMBA|✔|✔|✔ (em versões suportadas)|✔|
|Alertas de deteção de ameaças baseada em rede|✔|X|✔|X|
|Integração do Windows Defender ATP *|✔ (em versões suportadas)|✔|X|X|
|Patches em falta|✔|✔|✔|✔|
|Configurações de segurança|✔|✔|✔|✔|
|Programas de antimalware|✔|✔|X|X|
|Acesso JIT da VM|✔|X|✔|X|
|Controlos de aplicações adaptáveis|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Encriptação de disco|✔|X|✔|X|
|Implantação de terceiros|✔|X|✔|X|
|NSGs|✔|X|✔|X|
|Deteção de ameaças fileless|✔|✔|X|X|
|Mapa de rede|✔|X|✔|X|
|Controlos de rede adaptável|✔|X|✔|X|

\* Estas funcionalidades são suportadas atualmente em pré-visualização pública.


## <a name="supported-paas-features"></a>Recursos de PaaS suportados 


|Serviço|Recomendações|Deteção de ameaças|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL *|✔| ✔|
|MySQL *|✔| ✔|
|O Azure Blob storage contas *|✔| ✔|
|Serviços aplicacionais|✔| ✔|
|Serviços Cloud|✔| X|
|VNets|✔| ND|
|Sub-redes|✔| ND|
|NICs|✔| ✔|
|NSGs|✔| ND|
|Subscrição|✔| ✔|

\* Estas funcionalidades são suportadas atualmente em pré-visualização pública. 

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [planear e compreender as considerações de conceção para adoção do Centro de segurança do Azure](security-center-planning-and-operations-guide.md).
- Saiba mais sobre [análise comportamental de máquinas virtuais e despejo de memória análise de memória no Centro de segurança](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Encontrar [perguntas frequentes sobre o Centro de segurança do Azure](security-center-faq.md).
- Encontrar [mensagens do Blogue acerca da segurança do Azure e a conformidade](https://blogs.msdn.com/b/azuresecurity/).
