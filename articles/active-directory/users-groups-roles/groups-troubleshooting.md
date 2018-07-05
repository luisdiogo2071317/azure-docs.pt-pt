---
title: Resolução de problemas de associação dinâmica para grupos | Documentos da Microsoft
description: Sugestões de resolução de problemas para associação dinâmica para grupos no Azure AD.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: aaded644635ab93cef9323ad38d1d150b15fe743
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450067"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Resolver problemas de associações dinâmicas a grupos
**Configurei uma regra num grupo, mas nenhuma das associações for atualizadas no grupo**<br/>Verifique os valores para atributos de utilizador na regra: existem utilizadores que satisfazem a regra? Se estiver tudo correto, aguarde algum tempo para o grupo ser preenchido. Dependendo do tamanho do seu inquilino, o grupo pode demorar até 24 horas a ser preenchido pela primeira vez, ou após uma alteração de regra.

**Configurei uma regra, mas agora os membros existentes da regra são removidos**<br/>Este comportamento está previsto. Os membros existentes do grupo são removidos quando uma regra está ativada ou alterada. Os utilizadores devolvidos da versão de avaliação da regra são adicionados como membros ao grupo.     

**Eu não vejo a associação é alterado instantaneamente quando adicionar ou alterar uma regra, por que não?**<br/>Avaliação da associação dedicado é feita periodicamente num processo assíncrono em segundo plano. Quanto tempo leva o processo é determinado pelo número de utilizadores no seu diretório e o tamanho do grupo criado como resultado a regra. Normalmente, os diretórios com um pequeno número de usuários verão as alterações de associação de grupo em menos de alguns minutos. Diretórios com um grande número de utilizadores podem demorar 30 minutos ou mais tempo a preencher.

### <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](../active-directory-apps-index.md)
* [O que é o Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](../connect/active-directory-aadconnect.md)
