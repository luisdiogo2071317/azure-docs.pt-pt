---
title: Gerir programas e as revisões de acesso de controles para o Azure AD | Documentos da Microsoft
description: Pode criar programas adicionais para cada governação, gestão de riscos e iniciativa de conformidade na sua organização para recolher e organizar as revisões de acesso do Azure Active Directory como controles.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: b62a150160daa1d6708dbf5edaa6772688e2ffa1
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607816"
---
# <a name="manage-programs-and-their-controls"></a>Gerir programas e respetivos controlos 

Azure Active Directory (Azure AD) inclui as revisões de acesso de membros do grupo e o acesso à aplicação. Estes exemplos de controles Certifique-se de supervisão para quem tem acesso a aplicações e associações a grupos da sua organização. As organizações podem utilizar estes controlos para abordar com eficiência seus governação, gestão de riscos e os requisitos de conformidade.

## <a name="create-and-manage-programs-and-their-controls"></a>Criar e gerir programas e respetivos controlos
Pode simplificar a forma de controlar e recolher as revisões de acesso para diferentes fins, organizando-os em programas. Cada revisão de acesso pode ser associado a um programa. Em seguida, quando preparar relatórios para um auditor, se possa focar nas revisões de acesso no âmbito de uma iniciativa específica.  Programas e os resultados de revisão de acesso são visíveis para os utilizadores na função de Administrador Global, administrador de conta de utilizador, administrador de segurança ou leitor de segurança.

Para ver uma lista de programas, vá para o [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) e selecione **programas**.

**Programa predefinido** sempre esteja presente. Se fizer parte de um administrador global ou função de administrador de conta de utilizador, pode criar programas adicionais. Por exemplo, pode optar por ter um programa para cada iniciativa de conformidade ou de suas metas de negócios.

Se já não necessita de um programa e ele não tem quaisquer controles ligados ao mesmo, pode eliminá-la.

## <a name="next-steps"></a>Passos Seguintes

- [Criar uma revisão de acesso para os membros de um grupo ou o acesso a uma aplicação](create-access-review.md)
- [Recuperar os resultados de uma revisão de acesso](retrieve-access-review.md)
