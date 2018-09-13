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
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: c616618df0ef3a7b97a8d90977559889022db232
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44718479"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Resolver problemas de associações dinâmicas a grupos

**Configurei uma regra num grupo, mas nenhuma das associações for atualizadas no grupo**<br/>Verifique os valores para atributos de utilizador na regra: existem utilizadores que satisfazem a regra? Se estiver tudo correto, aguarde algum tempo para o grupo ser preenchido. Dependendo do tamanho do seu inquilino, o grupo pode demorar até 24 horas a ser preenchido pela primeira vez, ou após uma alteração de regra.

**Configurei uma regra, mas agora os membros existentes da regra são removidos**<br/>Este comportamento está previsto. Os membros existentes do grupo são removidos quando uma regra está ativada ou alterada. Os utilizadores devolvidos da versão de avaliação da regra são adicionados como membros ao grupo.

**Eu não vejo a associação é alterado instantaneamente quando adicionar ou alterar uma regra, por que não?**<br/>Avaliação da associação dedicado é feita periodicamente num processo assíncrono em segundo plano. Quanto tempo leva o processo é determinado pelo número de utilizadores no seu diretório e o tamanho do grupo criado como resultado a regra. Normalmente, os diretórios com um pequeno número de usuários verão as alterações de associação de grupo em menos de alguns minutos. Diretórios com um grande número de utilizadores podem demorar 30 minutos ou mais tempo a preencher.

**Deparei-me uma regra de erro de processamento**<br/>A tabela seguinte lista erros comuns de regra de associação dinâmica e como corrigi-las.

| Erro da regra de analisador | Utilização de erro | Utilização corrigida |
| --- | --- | --- |
| Erro: O atributo não suportado. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Certifique-se de que o atributo for a [suportada a lista de propriedades](groups-dynamic-membership.md#supported-properties). |
| Erro: O operador não é suportado no atributo. |(user.accountEnabled-contém o verdadeiro) |(user.accountEnabled - eq verdadeiro)<br/><br/>O operador usado não é suportado para o tipo de propriedade (neste exemplo,-contém não pode ser utilizado no tipo Booleano). Utilize os operadores corretos para o tipo de propriedade. |
| Erro: Erro de compilação de consulta. | 1. (user.department -eq "Sales") (user.department -eq "Marketing")<br>2. (user.userPrincipalName-corresponder a "*@domain.ext") | 1. Operador em falta. Utilize o - e ou - ou dois associar predicados<br>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br>2. Erro ao corresponder a expressão regular utilizada com -<br>(user.userPrincipalName-correspondência ". *@domain.ext")<br>ou, em alternativa: (user.userPrincipalName-correspondência "@domain.ext$") |

## <a name="next-steps"></a>Passos Seguintes

Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Gestão de aplicações no Azure Active Directory](../manage-apps/what-is-application-management.md)
* [O que é o Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](../connect/active-directory-aadconnect.md)
