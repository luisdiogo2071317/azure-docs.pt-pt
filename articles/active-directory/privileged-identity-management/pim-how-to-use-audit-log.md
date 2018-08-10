---
title: Como utilizar o log de auditoria no Azure AD Privileged Identity Management | Documentos da Microsoft
description: Saiba como utilizar o registo de auditoria na extensão do Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 922658f75a0d00998072c7bd8f2160e9db27f318
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627795"
---
# <a name="using-the-audit-log-in-pim"></a>Usando o log de auditoria no PIM
Pode utilizar o log de auditoria do Privileged Identity Management (PIM) para ver todas as atribuições de utilizador e ativações dentro de um determinado período de tempo. Se pretender ver o histórico de auditoria de atividade no seu inquilino, incluindo administrador, o utilizador final e atividade de sincronização, pode utilizar o [relatórios de acesso e utilização do Azure Active Directory.](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-the-audit-log"></a>Navegue para o registo de auditoria
Partir do [portal do Azure](https://portal.azure.com) dashboard, selecione o **Azure AD Privileged Identity Management** aplicação. A partir daí, acesso ao registo de auditoria clicando **gerir funções com privilégios** > **histórico de auditorias** no dashboard do PIM.

## <a name="the-audit-log-graph"></a>O gráfico de log de auditoria
Pode utilizar o log de auditoria para ver o total de ativações, ativações máximas por dia e ativações média por dia num gráfico de linha.  Também pode filtrar os dados por função, se houver mais de uma função no histórico de auditorias.

Utilize o **tempo**, **ação**, e **função** botões para ordenar o registo.

## <a name="the-audit-log-list"></a>A lista de registo de auditoria
As colunas na lista de registo de auditoria são:

* **Requerente** -o utilizador que pediu a ativação de função ou a alteração.  Se o valor for "Azure System", verifique o registo de auditoria do Azure para obter mais informações.
* **Utilizador** -o utilizador que está atribuído a uma função ou a ativação.
* **Função** -a função atribuída ou ativada pelo utilizador.
* **Ação** - as ações executadas pelo requerente. Isto pode incluir a atribuição, unassignment, ativação ou desativação.
* **Tempo** - quando ocorreu a ação.
* **Raciocinando** -se qualquer texto foi inserido no campo motivo durante a ativação, irá aparecer aqui.
* **Expiração** - relevante apenas para a ativação de funções.

## <a name="filter-the-audit-log"></a>Filtre o registo de auditoria
Pode filtrar as informações que aparece no log de auditoria, clicando a **filtro** botão.  O **painel de parâmetros do gráfico de atualização** aparecerá.

Depois de definir os filtros, clique em **atualização** para filtrar os dados no registo.  Se os dados não aparecerem imediatamente, atualize a página.

### <a name="change-the-date-range"></a>Alterar o intervalo de datas
Utilize o **hoje**, **semana passada**, **último mês**, ou **personalizado** botões para alterar o intervalo de tempo de log de auditoria.

Ao escolher o **personalizada** botão, terá a chance um **da** campo de data e um **para** campo de data para especificar um intervalo de datas para o log.  Pode introduzir as datas no formato DD/MM/AAAA ou clique nas **calendário** ícone e selecione a data de um calendário.

### <a name="change-the-roles-included-in-the-log"></a>Alterar as funções incluídas no registo
Marque ou desmarque os **função** caixa de verificação junto a cada função para incluir ou exclui-lo do log.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

