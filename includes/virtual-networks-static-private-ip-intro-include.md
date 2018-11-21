---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 5c1caf87f32ddd827b85263ee634d3e15d821124
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52271696"
---
As máquinas virtuais (VMs) IaaS e instâncias de função de PaaS numa rede virtual recebem automaticamente um endereço IP privado a partir de um intervalo que especificou, com base na sub-rede estão ligados a. Esse endereço é mantido pela VMs e instâncias de função, até que estas forem desativadas. Desativar uma instância de função de VM ou ao pará-la a partir do PowerShell, a CLI do Azure ou o portal do Azure. Nesses casos, assim que a instância VM ou a função for iniciada novamente, ele irá receber um endereço IP disponível da infraestrutura do Azure, que pode não ser o mesmo tinha anteriormente. Se encerrar a instância VM ou função do sistema operativo convidado, mantém o endereço IP que tinha.  

Em certos casos, desejar uma instância VM ou função para ter um endereço IP estático, por exemplo, se a VM será executado DNS ou vai ser um controlador de domínio. Pode fazê-lo ao definir um endereço IP privado estático.

