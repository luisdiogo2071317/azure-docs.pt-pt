---
title: incluir ficheiro
description: incluir ficheiro
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 1f41567f7d5dcc9b6536b47ef639ba53abff8ec4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31613641"
---
Os seguintes limites aplicam-se a cópia de segurança do Azure.

| Identificador de limite | Limite Predefinido |
| --- | --- |
| Número de servidores/máquinas que podem ser registados em relação a cada Cofre |50 para Windows Server SCDPM/cliente <br/> 1000 para as VMs do IaaS |
| Tamanho de uma origem de dados para dados armazenados no armazenamento de cofre do Azure |Máx. de 54400 GB<sup>1</sup> |
| Número de cofres de cópia de segurança que podem ser criados em cada subscrição do Azure |500 cofres dos serviços de recuperação por região |
| Número de vezes que a cópia de segurança pode ser agendada por dia |3 por dia para o Windows Server/cliente Windows <br/> 2 por dia para SCDPM <br/> Uma vez por dia para as VMs do IaaS |
| Discos de dados ligados a uma máquina virtual do Azure para cópia de segurança |16 |
| Tamanho do disco de dados individuais ligado a uma máquina virtual do Azure para cópia de segurança| 4095 GB <sup>2</sup>|

* <sup>1</sup>o limite de 54400 GB não é aplicável a cópia de segurança de VM do IaaS.
 

