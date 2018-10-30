---
title: O que é o licenciamento no Azure Active Directory com base em grupo? | Microsoft Docs
description: Saiba mais sobre o Azure Active Directory com base em grupo licenciamento, incluindo como funciona e melhores práticas.
services: active-directory
keywords: Licenciamento do Azure AD
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 3f23b28c1b20155e50fddf17db90cd2a53c04855
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209824"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>O que é o licenciamento no Azure Active Directory com base em grupo?

Os serviços cloud pagos da Microsoft, como o Office 365, Enterprise Mobility + Security, Dynamics 365 e outros produtos semelhantes, requerem licenças. Estas licenças são atribuídas a cada utilizador que tenha de aceder a estes serviços. Para gerir licenças, os administradores utilizam um dos portais de gestão (Office ou Azure) e cmdlets do PowerShell. O Azure Active Directory (Azure AD) é a infraestrutura subjacente que suporta a gestão de identidades para todos os serviços cloud da Microsoft. O Azure AD armazena informações sobre os estados de atribuição de licenças para os utilizadores.

Até agora, só foi possível atribuir licenças ao nível do utilizador individual, o que pode dificultar a gestão em grande escala. Por exemplo, para adicionar ou remover licenças de utilizador baseadas em alterações organizacionais, como utilizadores a aderir ou a sair da organização ou um departamento, um administrador tem de escrever muitas vezes um script complexo do PowerShell. Este script faz chamadas individuais para o serviço cloud.

Para resolver esses desafios, o Azure AD inclui agora licenciamento baseado no grupo. Pode atribuir uma ou mais licenças de produto a um grupo. O Azure AD garante que as licenças são atribuídas a todos os membros do grupo. São atribuídas as licenças adequadas a quaisquer novos membros que adiram ao grupo. Ao saírem do grupo, essas licenças são removidas. Isto elimina a necessidade de automatizar a gestão de licenças através do PowerShell para refletir as alterações na organização e na estrutura departamental por utilizador.


## <a name="features"></a>Funcionalidades

Seguem-se as principais funcionalidades do licenciamento baseado no grupo:

- As licenças podem ser atribuídas a qualquer grupo de segurança no Azure AD. Grupos de segurança podem ser sincronizados no local, utilizando o Azure AD Connect. Também pode criar grupos de segurança diretamente no Azure AD (também denominados grupos apenas na cloud) ou automaticamente através da funcionalidade de grupos dinâmicos do Azure AD.

- Quando uma licença de produto é atribuída a um grupo, o administrador pode desativar um ou mais planos de serviço no produto. Normalmente, isto é feito quando a organização ainda não está preparada para começar a utilizar um serviço incluído num produto. Por exemplo, o administrador pode atribuir o Office 365 a um departamento, mas desativar temporariamente o serviço Yammer.

- São suportados todos os serviços cloud da Microsoft que exigem licenciamento ao nível do utilizador. Isto inclui todos os produtos do Office 365, Enterprise Mobility + Security e Dynamics 365.

- O licenciamento baseado no grupo está atualmente disponível apenas através do [portal do Azure](https://portal.azure.com). Se utilizar principalmente outros portais de gestão para gestão de o utilizadores e grupos, como o portal do Office 365, pode continuar a fazê-lo. No entanto, deve utilizar o portal do Azure para gerir as licenças ao nível do grupo.

- O Azure AD gere automaticamente as modificações de licença que resultam de alterações à associação ao grupo. Normalmente, as modificações de licença entram em vigor alguns minutos após uma alteração de associação.

- Um utilizador pode ser membro de vários grupos com políticas de licença especificadas. Um utilizador também pode ter algumas licenças atribuídas diretamente, fora de quaisquer grupos. O estado de utilizador resultante é uma combinação de todos os produtos e licenças de serviço atribuídos. Se a mesma licença é atribuída a um utilizador de várias origens, a licença será consumida apenas uma vez.

- Em alguns casos, não é possível atribuir licenças a um utilizador. Por exemplo, podem não existir licenças disponíveis suficientes no inquilino ou podem ter sido atribuídos serviços em conflito ao mesmo tempo. Os administradores têm acesso a informações sobre os utilizadores para os quais o Azure AD não conseguiu processar totalmente licenças de grupo. Em seguida, podem tomar medidas corretivas com base nessas informações.

- Uma subscrição paga ou de avaliação para o Azure AD básico ou um paga ou de avaliação do Office 365 Enterprise E3, Office 365 A3 e superiores edições é necessário no inquilino para utilizar a gestão de licença baseada em grupo. Esta funcionalidade requer a licença para cada utilizador exclusivo que é um membro dos grupos que são atribuídos a licença. Não é necessário atribuir licenças aos utilizadores para os mesmos para serem membros de grupos que são atribuídos a licença, mas tem de ter o número mínimo de licenças no inquilino para cobrir todos esses usuários. Por exemplo, se tivesse um total de 1000 utilizadores exclusivos em todos os grupos com licenças atribuídas no seu inquilino, precisaria 1.000, pelo menos, licenças para cumprir o requisito de licença.

## <a name="your-feedback-is-welcome"></a>Os seus comentários são bem-vindos!

Se tiver comentários ou pedidos de funcionalidades, partilhe-os connosco através do [fórum de administração do Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre outros cenários para gestão de licenças através do licenciamento baseado no grupo, veja:

* [Atribuir licenças a um grupo no Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Identificar e resolver problemas de licença para um grupo no Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Como migrar os utilizadores licenciados individuais para o licenciamento baseado no grupo no Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Como migrar os utilizadores entre licenças de produto através do licenciamento com o botão com base em grupo no Azure Active Directory](../users-groups-roles/licensing-groups-change-licenses.md)
* [Cenários adicionais de licenciamento baseado no grupo do Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)
* [Exemplos do PowerShell para licenciamento com o botão com base em grupo no Azure Active Directory](../users-groups-roles/licensing-ps-examples.md)
