---
title: 'O Azure Active Directory Domain Services: Suspenso domínios | Documentos da Microsoft'
description: A suspensão do domínio gerido e eliminação
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: ergreenl
ms.openlocfilehash: 37524fbdf3cd521414a507f6fb67421708343ccd
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934249"
---
# <a name="suspended-domains"></a>Domínios suspensos
Quando o Azure AD Domain Services não consegue atender a um domínio gerido por um longo período de tempo, o domínio gerido é colocado num estado suspenso. Este artigo explicará por que os domínios geridos são suspensas, o comprimento de suspensão e para saber como corrigir um domínio suspenso.


## <a name="overview-of-suspended-domains"></a>Descrição geral dos domínios suspensos

![Linha cronológica de domínio suspenso](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

O gráfico anterior descreve como um domínio é suspenso, o tempo em que ela será suspensa e, por fim, a eliminação de um domínio gerido. As secções seguintes detalham os motivos por que um domínio pode ser suspenso e como anular a suspensão de um domínio gerido.


## <a name="why-are-managed-domains-suspended"></a>Por que os domínios geridos são suspensas?

Domínios geridos são suspensas quando estiverem no Estado em que o Azure AD Domain Services é não é possível gerir o domínio. Isto pode ser causado por uma configuração incorreta que bloqueia o acesso aos recursos necessários para o Azure AD Domain Services ou uma subscrição do Azure inativa. Após 15 dias a ser incapaz de atender a um domínio gerido, o Azure AD Domain Services suspenderá o domínio.

As razões exatas, por que o seu domínio foi possível suspender estão listadas abaixo:
* Ter um ou mais dos seguintes alertas presentes no seu domínio durante 15 dias consecutivos:
   * [AADDS104: Erro de rede](active-directory-ds-troubleshoot-nsg.md).
* A subscrição do Azure está expirado ou inativos


## <a name="what-happens-when-a-domain-is-suspended"></a>O que acontece quando um domínio é suspenso?

Quando um domínio estiver suspenso, o Azure AD Domain Services para as máquinas virtuais que o seu domínio gerido do serviço. Isso significa que as cópias de segurança já não são executadas, os utilizadores não conseguem iniciar sessão no seu domínio e já não é efetuada a sincronização com o Azure AD.

O domínio apenas irá manter-se num Estado de suspensão de um máximo de 15 dias. Para garantir uma recuperação em tempo hábil, recomenda-se que a suspensão de endereços assim que possível.

## <a name="how-do-i-know-if-my-domain-is-suspended"></a>Como posso saber se meu domínio está suspenso?
O domínio gerido vai receber uma [alerta](active-directory-ds-troubleshoot-alerts.md) na página de estado de funcionamento de serviços de domínio do Azure AD no portal do Azure, que declara o domínio suspenso. Além disso, o estado do domínio irão ser assinaladas como "Suspenso".


## <a name="unsuspending-and-restoring-domains"></a>Domínios unsuspending e restauro

Para anular a suspensão de um domínio, são necessários os seguintes passos:

1. Navegue para o [página do Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) no portal do Azure
2. Clique no domínio que pretende anular a suspensão
3. No painel de navegação esquerdo, clique em **estado de funcionamento**
4. Clique no alerta de suspensão (o ID do alerta será AADDS503 ou AADDS504, dependendo da causa de suspensão).
5. Clique na ligação resolução fornecida no alerta e siga os passos para resolver a suspensão.

O domínio só pode ser restaurado a partir da data da última cópia de segurança. A data da última cópia de segurança é apresentada na página de estado de funcionamento do seu domínio gerido. Todas as alterações desde a última cópia de segurança não serão restauradas durante unsuspension. Além disso, o Azure AD Domain Services só pode armazenar cópias de segurança até 30 dias. Se a cópia de segurança mais recente é a mais de 30 dias, a cópia de segurança têm de ser eliminada e o Azure AD Domain Services será possível restaurar a partir de uma cópia de segurança.

## <a name="deleting-domains"></a>A eliminação de domínios

Se o domínio é suspenso durante mais de 15 dias, o Azure AD Domain Services elimina o domínio gerido devido a inatividade e a incapacidade de domínio do serviço. Não será cobrado para serviços de domínio do Azure AD. Para restaurar o seu domínio gerido, terá de recriá-lo.


## <a name="next-steps"></a>Passos Seguintes
- [Resolver alertas no seu domínio gerido](active-directory-ds-troubleshoot-alerts.md)
- [Leia mais sobre o Azure AD Domain Services](active-directory-ds-overview.md)
- [Contacte a equipa de produto](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contacte-nos

Contacte a equipa de produto do Azure Active Directory Domain Services para [partilhar comentários ou para o suporte](active-directory-ds-contact-us.md).
