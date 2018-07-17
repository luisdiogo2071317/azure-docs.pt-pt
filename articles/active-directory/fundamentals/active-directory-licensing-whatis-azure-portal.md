---
title: O que é o licenciamento baseado no grupo no Azure AD? | Microsoft Docs
description: Descrição do licenciamento baseado no grupo do Azure Active Directory, como funciona e melhores práticas
services: active-directory
keywords: Licenciamento do Azure AD
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: overview
ms.workload: identity
ms.date: 06/13/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: c6800e5a00d2fb837538141571c105a1d21e4374
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860462"
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Noções básicas do licenciamento baseado no grupo no Azure Active Directory

Os serviços cloud pagos da Microsoft, como o Office 365, Enterprise Mobility + Security, Dynamics 365 e outros produtos semelhantes, requerem licenças. Estas licenças são atribuídas a cada utilizador que tenha de aceder a estes serviços. Para gerir licenças, os administradores utilizam um dos portais de gestão (Office ou Azure) e cmdlets do PowerShell. O Azure Active Directory (Azure AD) é a infraestrutura subjacente que suporta a gestão de identidades para todos os serviços cloud da Microsoft. O Azure AD armazena informações sobre os estados de atribuição de licenças para os utilizadores.

Até agora, só foi possível atribuir licenças ao nível do utilizador individual, o que pode dificultar a gestão em grande escala. Por exemplo, para adicionar ou remover licenças de utilizador baseadas em alterações organizacionais, como utilizadores a aderir ou a sair da organização ou um departamento, um administrador tem de escrever muitas vezes um script complexo do PowerShell. Este script faz chamadas individuais para o serviço cloud.

Para resolver esses desafios, o Azure AD inclui agora licenciamento baseado no grupo. Pode atribuir uma ou mais licenças de produto a um grupo. O Azure AD garante que as licenças são atribuídas a todos os membros do grupo. São atribuídas as licenças adequadas a quaisquer novos membros que adiram ao grupo. Ao saírem do grupo, essas licenças são removidas. Isto elimina a necessidade de automatizar a gestão de licenças através do PowerShell para refletir as alterações na organização e na estrutura departamental por utilizador.

>[!NOTE]
>Esta funcionalidade está atualmente em pré-visualização pública. Esteja preparado para reverter ou remover todas as alterações. A funcionalidade está disponível com qualquer plano de licença pago do Azure Active Directory (Azure AD) durante a pré-visualização pública. No entanto, quando a funcionalidade se torna disponível em geral, alguns aspetos da mesma podem exigir uma ou mais licenças do Azure Active Directory Premium.

## <a name="features"></a>Funcionalidades

Seguem-se as principais funcionalidades do licenciamento baseado no grupo:

- As licenças podem ser atribuídas a qualquer grupo de segurança no Azure AD. Os grupos de segurança podem ser sincronizados no local através do Azure AD Connect. Também pode criar grupos de segurança diretamente no Azure AD (também denominados grupos apenas na cloud) ou automaticamente através da funcionalidade de grupos dinâmicos do Azure AD.

- Quando uma licença de produto é atribuída a um grupo, o administrador pode desativar um ou mais planos de serviço no produto. Normalmente, isto é feito quando a organização ainda não está preparada para começar a utilizar um serviço incluído num produto. Por exemplo, o administrador pode atribuir o Office 365 a um departamento, mas desativar temporariamente o serviço Yammer.

- São suportados todos os serviços cloud da Microsoft que exigem licenciamento ao nível do utilizador. Isto inclui todos os produtos do Office 365, Enterprise Mobility + Security e Dynamics 365.

- O licenciamento baseado no grupo está atualmente disponível apenas através do [portal do Azure](https://portal.azure.com). Se utilizar principalmente outros portais de gestão para gestão de o utilizadores e grupos, como o portal do Office 365, pode continuar a fazê-lo. No entanto, deve utilizar o portal do Azure para gerir as licenças ao nível do grupo.

- O Azure AD gere automaticamente as modificações de licença que resultam de alterações à associação ao grupo. Normalmente, as modificações de licença entram em vigor alguns minutos após uma alteração de associação.

- Um utilizador pode ser membro de vários grupos com políticas de licença especificadas. Um utilizador também pode ter algumas licenças atribuídas diretamente, fora de quaisquer grupos. O estado de utilizador resultante é uma combinação de todos os produtos e licenças de serviço atribuídos.

- Em alguns casos, não é possível atribuir licenças a um utilizador. Por exemplo, podem não existir licenças disponíveis suficientes no inquilino ou podem ter sido atribuídos serviços em conflito ao mesmo tempo. Os administradores têm acesso a informações sobre os utilizadores para os quais o Azure AD não conseguiu processar totalmente licenças de grupo. Em seguida, podem tomar medidas corretivas com base nessas informações.

- Durante a pré-visualização pública, é necessária uma subscrição paga ou de avaliação das edições Azure AD Básico ou Premium no inquilino para utilizar a gestão de licenças baseadas no grupo.

## <a name="your-feedback-is-welcome"></a>Os seus comentários são bem-vindos!

Se tiver comentários ou pedidos de funcionalidades, partilhe-os connosco através do [fórum de administração do Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre outros cenários para gestão de licenças através do licenciamento baseado no grupo, veja:

* [Atribuir licenças a um grupo no Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Identificar e resolver problemas de licença para um grupo no Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Como migrar os utilizadores licenciados individuais para o licenciamento baseado no grupo no Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Cenários adicionais de licenciamento baseado no grupo do Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)
