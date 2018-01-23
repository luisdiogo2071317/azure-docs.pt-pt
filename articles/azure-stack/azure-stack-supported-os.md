---
title: Sistemas operativos convidados suportados para a pilha do Azure | Microsoft Docs
description: Estes sistemas operativos de convidado pode ser utilizados na pilha do Azure.
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: c9f5bee38772623fb79fa081be8eaece981cc8ab
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Sistemas operativos convidados suportados na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

## <a name="windows"></a>Windows
Pilha do Azure suporta os seguintes sistemas de operativos de convidado do Windows. Imagens no Marketplace estão disponíveis para transferência para a pilha do Azure. Imagens de cliente do Windows não estão disponíveis no mercado.

Durante a implementação, pilha do Azure irá garantir que uma versão adequada do agente convidado é inserida na imagem.

| Sistema operativo | Descrição | Publicador | Tipo de SO | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 bits | Microsoft | Windows | Datacenter |
| Windows Server 2012 | 64 bits | Microsoft | Windows | Datacenter |
| Windows Server 2012 R2 | 64 bits | Microsoft | Windows | Datacenter |
| Windows Server 2016 | 64 bits | Microsoft | Windows | Centro de dados, Centro de dados principal, Datacenter com contentores |
| Windows 7 | 64 bits, Pro e Enterprise | Microsoft | Windows | Não |
| Windows 8.1 | 64 bits, Pro e Enterprise | Microsoft | Windows | Não |
| Windows 10 *(ver nota 1)* | 64 bits, Pro e Enterprise | Microsoft | Windows | Não |

***Nota 1:****para implementar sistemas de operativos cliente Windows 10 na pilha do Azure, tem de ter [Windows por utilizador licenciamento](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) ou comprar através de um fornecedor de alojamento qualificado do multi-inquilino ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

As distribuições do Linux listadas aqui incluem o agente Linux de Azure de Windows necessário (WALA).

> [!NOTE]   
> Imagens incorporadas com versões WALA mais antigas que são 2.2.3 *não* suportado e é improvável que implementar. Algumas versões do agente WALA se sabe não funcionam em VMs de pilha do Azure, incluindo versões 2.2.12 e 2.2.13.


| Distribuição | Descrição | Publicador | Marketplace |
| --- | --- | --- | --- | --- | --- |
| Contentor Linux |  64 bits | CoreOS | Estável |
| Com base em centOS 6.9 | 64 bits | Rogue Wave | Sim |
| CentOS-based 7.3 | 64 bits | Rogue Wave | Sim |
| 7.4 baseado em centOS | 64 bits | Rogue Wave | Sim |
| Debian 8 "Jessie" | 64 bits | credativ |  Sim |
| Debian 9 "Stretch" | 64 bits | credativ | Sim |
| Oracle Linux | 64 bits | Oracle | Não |
| Red Hat Enterprise Linux 7.x | 64 bits | Red Hat | Não |
| SLES 11SP4 | 64 bits | SUSE | Sim |
| SLES 12SP3 | 64 bits | SUSE | Sim |
| Ubuntu 14.04-LTS | 64 bits | Canónico | Sim |
| Ubuntu 16.04-LTS | 64 bits | Canónico | Sim |

Outras as distribuições do Linux podem ser suportadas no futuro.
