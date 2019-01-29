---
title: 'Início Rápido: Transferir um relatório de auditoria no portal do Azure | Microsoft Docs'
description: Aprender a transferir um relatório de auditoria no portal do Azure
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: e589f613eb3afc8efe409773f37a9855f8fc5432
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180336"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Início rápido: Transferir um relatório de auditoria no portal do Azure

Neste início rápido, vai aprender a transferir os registos de auditoria do seu inquilino das últimas 24 horas. Pode baixar até 5000 registos do portal do Azure. Os registos são ordenados pelos mais recentes para que, por padrão, obtém os registos de 5000 mais recentes. 

## <a name="prerequisites"></a>Pré-requisitos

É necessário:

* Um inquilino do Azure Active Directory. 
* Um utilizador, o que está a **administrador de segurança**, **leitor de segurança**, ou **Administrador Global** função para o inquilino. Além disso, qualquer utilizador no inquilino pode aceder aos seus próprios registos de auditoria.

## <a name="quickstart-download-an-audit-report"></a>Início rápido: Transferir um relatório de auditoria

1. Navegue para o [portal do Azure](https://portal.azure.com).
2. Selecione **Azure Active Directory** no painel de navegação esquerdo e utilize o botão **Trocar diretório** para selecionar o Active Directory.
3. No dashboard, selecione **Azure Active Directory** e, em seguida, selecione **Registos de auditoria**. 
4. Escolha **últimas 24 horas** no filtro pendente **Intervalo de datas** e selecione **Aplicar** para ver os registos de auditoria das últimas 24 horas. 
5. Selecione o botão **Transferir** para transferir um ficheiro CSV com os registos filtrados. 

![Relatórios](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Passos Seguintes

* [Relatórios de atividade de início de sessão no portal do Azure Active Directory](concept-sign-ins.md)
* [Retenção de relatórios do Azure Active Directory](reference-reports-data-retention.md)
* [Latências dos relatórios do Azure Active Directory](reference-reports-latencies.md)
