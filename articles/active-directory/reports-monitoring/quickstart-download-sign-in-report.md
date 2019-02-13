---
title: 'Início Rápido: Transferir um relatório de início de sessão no portal do Azure | Microsoft Docs'
description: Aprender a transferir um relatório de início de sessão no portal do Azure
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: eeb5a06aa4eb21ba374344623f9f96ce92a59ff5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168822"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Início rápido: Transferir um relatório de início de sessão no portal do Azure

Neste início rápido, vai aprender a transferir os dados de início de sessão do seu inquilino das últimas 24 horas. Pode baixar até 5000 registos do portal do Azure. Os registos são ordenados pelos mais recentes para que, por padrão, obtém os registos de 5000 mais recentes. 

## <a name="prerequisites"></a>Pré-requisitos

É necessário:

* Um inquilino do Azure Active Directory, com uma licença Premium para ver o relatório de atividade de início de sessão. Ver [introdução ao Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para atualizar a sua edição do Azure Active Directory. Tenha em atenção que, se não tem quaisquer dados de atividades antes da atualização, demorará alguns dias para os dados sejam apresentados nos relatórios depois de atualizar para uma licença premium.
* Um utilizador, o que está a **administrador de segurança**, **leitor de segurança**, **leitor de relatório** ou **Administrador Global** função para o inquilino. Além disso, qualquer utilizador no inquilino pode aceder aos seus inícios de sessão.

## <a name="quickstart-download-a-sign-in-report"></a>Início rápido: Transferir um relatório de início de sessão

1. Navegue para o [portal do Azure](https://portal.azure.com).
2. Selecione **Azure Active Directory** no painel de navegação esquerdo e utilize o botão **Trocar diretório** para selecionar o Active Directory.
3. No dashboard, selecione **Azure Active Directory** e, em seguida, selecione **Inícios de sessão**. 
4. Escolha **últimas 24 horas** no filtro pendente **Data** e selecione **Aplicar** para ver os inícios de sessão das últimas 24 horas. 
5. Selecione o botão **Transferir** para transferir um ficheiro CSV com os registos filtrados. 

![Relatórios](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Passos Seguintes

* [Relatórios de atividade de início de sessão no portal do Azure Active Directory](concept-sign-ins.md)
* [Retenção de relatórios do Azure Active Directory](reference-reports-data-retention.md)
* [Latências dos relatórios do Azure Active Directory](reference-reports-latencies.md)
