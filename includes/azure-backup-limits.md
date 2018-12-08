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
ms.openlocfilehash: 63922eb623576379058c9a8a367d6e52249115f2
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109098"
---
Os limites seguintes aplicam-se a cópia de segurança do Azure.

| **Limite** | **Predefinição** |
| --- | --- |
| Servidores/máquinas que podem ser registadas num cofre | Windows Server/Windows Client/System Center DPM: 50 <br/><br/> VMs de IaaS: 1000  |
| Tamanho de uma origem de dados no armazenamento do Cofre |54400 GB no máximo. O limite não se aplica a cópia de segurança de VM de IaaS |
| Cofres de cópia de segurança de uma subscrição do Azure |500 cofres por região |
| Agendar cópias de segurança diárias |Windows Server/cliente: 3 por dia<br/> O System Center DPM: 2 por dia <br/> VMs de IaaS: Uma vez por dia  |
| Discos de dados anexados a uma VM do Azure para cópia de segurança | 32 |
| Disco de dados individuais ligado à VM do Azure para cópia de segurança| 4095 GB|



