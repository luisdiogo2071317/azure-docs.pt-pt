---
title: A filiação dinâmica para grupos de resolução de problemas | Microsoft Docs
description: Sugestões de resolução de problemas para a filiação dinâmica para grupos no Azure AD.
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
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 6d8d04273e9f29b2634c8b77b0268f3c7b77b1e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Resolver problemas de associações dinâmicas a grupos
**Posso configurar uma regra num grupo, mas não associações for atualizadas no grupo**<br/>Verifique os valores para os atributos de utilizador na regra: existem utilizadores que satisfazem a regra? Se tudo procura boa, aguarde algum tempo para preencher o grupo. Dependendo do tamanho do seu inquilino, o grupo pode demorar até 24 horas a ser preenchido pela primeira vez, ou após uma alteração de regra.

**Posso configurado uma regra, mas agora são removidos os membros existentes da regra**<br/>Este comportamento está previsto. Membros existentes do grupo são removidos quando uma regra está ativada ou alterada. Os utilizadores devolvidos de avaliação da regra são adicionados como membros ao grupo.     

**Não consigo ver associação mudar instantly quando adicionar ou alterar uma regra, por que motivo não?**<br/>Avaliação da associação dedicada é feita periodicamente num processo em segundo plano assíncronas. Tempo de demora o processo é determinado pelo número de utilizadores no seu diretório e o tamanho do grupo criado no seguimento da regra. Normalmente, os diretórios com pequeno número de utilizadores verão as alterações de associação de grupo em menos de alguns minutos. Diretórios com um grande número de utilizadores podem demorar de 30 minutos ou mais para preencher.

### <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](active-directory-manage-groups.md)
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)
* [O que é o Azure Active Directory?](active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
