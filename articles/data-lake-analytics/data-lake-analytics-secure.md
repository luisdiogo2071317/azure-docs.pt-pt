---
title: Proteger o Azure Data Lake Analytics para vários utilizadores
description: Saiba como configurar vários utilizadores para executar tarefas no Azure Data Lake Analytics.
ms.service: data-lake-analytics
services: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 1d92e6d0e619584dedcbc9a66450c25dd1ac8b75
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701420"
---
# <a name="configure-user-access-to-job-information-to-job-information-in-azure-data-lake-analytics"></a>Configurar o acesso de utilizador para as informações da tarefa para as informações da tarefa no Azure Data Lake Analytics 

No Azure Data Lake Analytics, pode utilizar várias contas de utilizador ou principais de serviço para executar tarefas. 

Para esses mesmos utilizadores ver a informação da tarefa de detalhado, os utilizadores necessitam conseguir ler o conteúdo das pastas de trabalho. As pastas de trabalho estão localizadas em `/system/` diretório. 

Se não estiverem configuradas as permissões necessárias, o utilizador poderá ver um erro: `Graph data not available - You don't have permissions to access the graph data.` 

## <a name="configure-user-access-to-job-information"></a>Configurar o acesso de utilizador para as informações da tarefa

Pode utilizar o **Assistente para adicionar utilizador** para configurar as ACLs de pastas. Para obter mais informações, consulte [adicionar um novo utilizador](data-lake-analytics-manage-use-portal.md#add-a-new-user).

Se precisar de mais controlo granular nem a necessidade de um script as permissões, em seguida, proteger as pastas da seguinte forma:

1. Conceder **executar** permissões da pasta de raiz (através de um acesso ACL):
   - /
   
2. Conceder **executar** e **ler** permissões (através de um ACL de acesso e uma predefinição ACL) sobre as pastas que contêm as pastas de trabalho. Por exemplo, para uma tarefa específica que foi executada na 25 de Maio de 2018, estas pastas tem de ser acedido:
   - /System
   - / sistema/jobservice
   - /System/jobservice/Jobs
   - /System/jobservice/Jobs/Usql
   - /System/jobservice/Jobs/Usql/2018
   - /System/jobservice/Jobs/Usql/2018/05
   - /System/jobservice/Jobs/Usql/2018/05/25
   - /System/jobservice/Jobs/Usql/2018/05/25/11
   - /System/jobservice/Jobs/Usql/2018/05/25/11/01
   - sistema/jobservice/tarefas/Usql/2018/05/25/11/01/b074bd7a-1448-d879-9d75-f562b101bd3d

## <a name="next-steps"></a>Passos Seguintes
[Adicionar um novo utilizador](data-lake-analytics-manage-use-portal.md#add-a-new-user)
