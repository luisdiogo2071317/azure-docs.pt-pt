---
title: Proteger o Azure Data Lake Analytics para vários utilizadores
description: Saiba como configurar vários usuários para executar tarefas no Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: c6b86e25602f36896855d2593952609904396879
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051589"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Configurar o acesso de utilizador, informações de tarefa a informações de tarefa no Azure Data Lake Analytics 

No Azure Data Lake Analytics, pode utilizar várias contas de utilizador ou principais de serviço para executar tarefas. 

Para esses mesmos usuários ver as informações detalhadas de tarefa, os utilizadores tem de ser capaz de ler o conteúdo das pastas de trabalho. As pastas de trabalho estão localizadas em `/system/` diretório. 

Se as permissões necessárias não estiverem configuradas, o utilizador poderá ver um erro: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Configurar o acesso de utilizador a informação de tarefa

Pode utilizar o **Assistente para adicionar utilizador** para configurar as ACLs em pastas. Para obter mais informações, consulte [adicionar um novo utilizador](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Se precisar de mais controle granular nem necessidade de permissões, de script, em seguida, proteja as pastas da seguinte forma:

1. Concessão **executar** permissões (por meio de uma ACL de acesso) na pasta raiz:
   - /
   
2. Concessão **executar** e **ler** permissões (por meio de uma ACL de acesso e uma ACL predefinida) sobre as pastas que contêm as pastas de trabalho. Por exemplo, de uma tarefa específica que foi executada no dia 25 de Maio de 2018, essas pastas tem de ser acedido:
   - . cmd /System
   - / sistema/jobservice
   - /System/jobservice/Jobs
   - /System/jobservice/Jobs/Usql
   - /System/jobservice/Jobs/Usql/2018
   - /System/jobservice/Jobs/Usql/2018/05
   - /System/jobservice/Jobs/Usql/2018/05/25
   - /System/jobservice/Jobs/Usql/2018/05/25/11
   - /System/jobservice/Jobs/Usql/2018/05/25/11/01
   - sistema/jobservice/tarefas/u-SQL/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Passos Seguintes
[Adicionar um novo utilizador](data-lake-analytics-manage-use-portal.md#add-a-new-user)
