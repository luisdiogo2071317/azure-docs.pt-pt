---
title: Atualize o agente de cópia de segurança do Azure
description: Estas informações explicam por que motivo, deve atualizar o agente do Backup do Azure e onde pode transferir a atualização.
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 3/16/2018
ms.author: markgal
ms.openlocfilehash: 4142f88c3ccb376acbdd2af07c5cfdf8fd275780
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
ms.locfileid: "29973304"
---
## <a name="upgrade-the-mars-agent"></a>Atualize o agente MARS
As versões do agente do serviço de recuperação do Azure (MARS) da Microsoft abaixo 2.0.9083.0 têm uma dependência no serviço de controlo de acesso do Azure (ACS). 2018 Azure irá [despromover o serviço de controlo de acesso do Azure (ACS)](../articles/active-directory/develop/active-directory-acs-migration.md). A partir 19 de Março de 2018, todas as versões do agente MARS abaixo 2.0.9083.0 irão ocorrer falhas de cópia de segurança. Para evitar ou resolver falhas de cópia de segurança, [atualizar o agente MARS para a versão mais recente](https://go.microsoft.com/fwlink/?linkid=229525). Para identificar os servidores que necessitam de uma atualização de agente MARS, siga os passos a [blogue de cópia de segurança para atualizar agentes de Backup do Azure](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). O agente MARS é utilizado para fazer cópias de segurança dos seguintes tipos de dados do Azure:
- Ficheiros 
- Dados de estado do sistema
- cópia de segurança do System Center DPM no Azure
- Servidor do Backup do Azure (MABS)
