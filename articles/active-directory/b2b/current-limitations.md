---
title: Limitações de colaboração B2B do Azure Active Directory do | Microsoft Docs
description: Limitações atuais para colaboração B2B do Azure Active Directory do
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 562076e9529ffeac4cb0f99c1ffd4d4866d0bd1a
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Limitações de colaboração B2B do Azure AD
Colaboração B2B do Active Directory (Azure AD) Azure está atualmente sujeitos as limitações descritas neste artigo.

## <a name="possible-double-multi-factor-authentication"></a>Autenticação multifator dupla possíveis
Com o Azure AD B2B, pode impor autenticação multifator na organização de recursos (a organização convidando). As razões para esta abordagem passo são detalhadas no [acesso condicional para os utilizadores de colaboração do B2B](conditional-access.md). Se um parceiro já tem a autenticação multifator, configurar e imposto, os seus utilizadores poderão ter de efetuar a autenticação de uma vez na respetiva organização inicial e, em seguida, novamente no seu.

## <a name="instant-on"></a>Instant-on
Os fluxos de colaboração B2B, iremos adicionar utilizadores ao diretório e atualizar dinamicamente durante resgate convite, atribuição de aplicação e assim sucessivamente. As atualizações e escritas normalmente acontecer na instância de um diretório e têm de ser replicadas em todas as instâncias. Conclusão da replicação depois de todas as instâncias são atualizadas. Por vezes, quando o objeto é escrito ou atualizado numa instância e a chamada para obter este objeto é outra instância, podem ocorrer latências de replicação. Se isto acontecer, atualize ou volte a tentar para o ajudar. Se estiver a escrever uma aplicação utilizando a nossa API, tentativas com algumas término é uma prática bom e defesas para reduzir este problema.

## <a name="next-steps"></a>Passos Seguintes

Consulte os artigos seguintes na colaboração B2B do Azure AD:

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Delegar B2bB convites de colaboração](delegate-invitations.md)

