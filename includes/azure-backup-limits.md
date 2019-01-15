---
title: incluir ficheiro
description: incluir ficheiro
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 9ad1208283527f35e04dede2706fd0e424096dc5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54300696"
---
Os limites seguintes aplicam-se a cópia de segurança do Azure.

| **Limite** | **Predefinição** |
| --- | --- |
| Servidores/máquinas que podem ser registadas num cofre | Centro de sistema de cliente/servidor/Windows do Windows DPM: 50 <br/><br/> VMs de IaaS: 1000  |
| Tamanho de uma origem de dados no armazenamento do Cofre |54400 GB no máximo. O limite não se aplica a cópia de segurança de VM de IaaS |
| Cofres de cópia de segurança de uma subscrição do Azure |500 cofres por região |
| Agendar cópias de segurança diárias |Windows Server/cliente: 3 por dia<br/> O System Center DPM: 2 por dia <br/> VMs de IaaS: Uma vez por dia  |
| Discos de dados anexados a uma VM do Azure para cópia de segurança | 16 |
| Disco de dados individuais ligado à VM do Azure para cópia de segurança| 4095 GB|
