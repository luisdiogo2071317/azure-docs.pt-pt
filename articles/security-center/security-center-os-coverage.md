---
title: Plataformas suportadas no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento fornece uma lista dos sistemas de operatings Windows e Linux suportados no Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: terrylan
ms.openlocfilehash: 54d173caa0e3eb4bbd8bda7c924e56d546a99662
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297507"
---
# <a name="supported-platforms-in-azure-security-center"></a>Plataformas suportadas no Centro de segurança do Azure
Monitorização de estado de segurança e recomendações, estão disponíveis para máquinas virtuais (VMs), criadas usando o clássicas e modelos de implementação do Resource Manager e computadores.

> [!NOTE]
> Saiba mais sobre o [clássico e modelos de implementação do Resource Manager](../azure-classic-rm.md) para recursos do Azure.
>
>

## <a name="supported-platforms-for-windows-computers-and-vms"></a>Plataformas suportadas para VMs e computadores do Windows
Sistemas de operativos Windows suportados:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-computers-and-vms"></a>Plataformas suportadas para VMs e computadores Linux
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

## <a name="next-steps"></a>Passos Seguintes

- [Guia de operações e planeamento do Centro de segurança do Azure](security-center-planning-and-operations-guide.md) – Saiba como planear e compreender as considerações de conceção para adoção do Centro de segurança do Azure
- [Alertas de segurança por tipo no Centro de segurança do Azure](security-center-alerts-type.md#virtual-machine-behavioral-analysis) - Saiba mais sobre a análise comportamental de máquinas virtuais e de falha de análise de informação de memória no Centro de segurança
- [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço
- [Blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – encontre mensagens do Blogue acerca da segurança do Azure e de conformidade
