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
ms.openlocfilehash: b345283f87c446ff3b583b0c5dd8a4be222303ae
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
Os seguintes limites aplicam-se a cópia de segurança do Azure.

| Identificador de limite | Limite Predefinido |
| --- | --- |
| Número de servidores/máquinas que podem ser registados em relação a cada Cofre |50 para Windows Server SCDPM/cliente <br/> 200 para as VMs do IaaS |
| Tamanho de uma origem de dados para dados armazenados no armazenamento de cofre do Azure |Máx. de 54400 GB<sup>1</sup> |
| Número de cofres de cópia de segurança que podem ser criados em cada subscrição do Azure |25 cofres dos serviços de recuperação por região |
| Número de vezes que a cópia de segurança pode ser agendada por dia |3 por dia para o Windows Server/cliente Windows <br/> 2 por dia para SCDPM <br/> Uma vez por dia para as VMs do IaaS |
| Discos de dados ligados a uma máquina virtual do Azure para cópia de segurança |16 |
| Tamanho do disco de dados individuais ligado a uma máquina virtual do Azure para cópia de segurança| 4095 GB <sup>2</sup>|

* <sup>1</sup>o limite de 54400 GB não é aplicável a cópia de segurança de VM do IaaS.
 

