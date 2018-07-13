---
title: Atualize o agente de cópia de segurança do Azure
description: Estas informações explicam por que deve atualizar o agente de cópia de segurança do Azure e onde pode transferir a atualização.
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750335"
---
## <a name="upgrade-the-mars-agent"></a>Atualize o agente de MARS
As versões do agente do serviço de recuperação do Azure (MARS) da Microsoft abaixo 2.0.9083.0 têm uma dependência no serviço de controlo de acesso do Azure (ACS). Em 2018 Azure serão [Preterir o serviço de controlo de acesso do Azure (ACS)](../articles/active-directory/develop/active-directory-acs-migration.md). A partir de 19 de Março de 2018, todas as versões do agente MARS abaixo 2.0.9083.0 irão experienciar falhas de cópia de segurança. Para evitar ou resolver falhas de cópia de segurança, [atualizar o agente MARS para a versão mais recente](https://go.microsoft.com/fwlink/?linkid=229525). Para identificar os servidores que necessitam de uma atualização do agente MARS, siga os passos a [blog de cópia de segurança para atualizar agentes de cópia de segurança do Azure](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). O agente de MARS é utilizado para fazer cópias de segurança de tipos de dados para o Azure:
- Ficheiros 
- Dados de estado do sistema
- cópia de segurança de System Center DPM para o Azure
- Servidor do Backup do Azure (MABS)
