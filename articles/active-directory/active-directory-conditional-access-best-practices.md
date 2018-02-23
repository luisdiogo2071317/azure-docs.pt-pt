---
title: "Melhores práticas para acesso condicional no Azure Active Directory | Microsoft Docs"
description: "Saiba mais sobre os aspetos que deve conhecer e o que é que deve evitar fazê-lo quando configurar políticas de acesso condicional."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 16f9179b6cbaee00a2afbe2efe090cb3eb8b204a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Melhores práticas para acesso condicional no Azure Active Directory

Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar o acesso de utilizadores autorizados como as suas aplicações em nuvem. Este artigo fornece informações sobre:

- Coisas que deve conhecer 
- O que é deve evitar a fazer quando configurar políticas de acesso condicional. 

Este artigo pressupõe que tem familiar os conceitos e a terminologia descritos em [de acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>O que tem necessário para tornar uma política de trabalho?

Quando cria uma nova política, não existem nenhum utilizadores, grupos, aplicações ou os controlos de acesso selecionados.

![Aplicações na cloud](./media/active-directory-conditional-access-best-practices/02.png)


Para tornar a sua política de trabalho, tem de configurar:


|O que           | Como                                  | Por que razão|
|:--            | :--                                  | :-- |
|**Aplicações na nuvem** |Tem de selecionar uma ou mais aplicações.  | O objetivo de uma política de acesso condicional é permitem controlar utilizadores como autorizados podem aceder a aplicações na nuvem.|
| **Utilizadores e grupos** | Tem de selecionar, pelo menos, um utilizador ou grupo que esteja autorizado a aceder às suas aplicações em nuvem selecionado. | Uma política de acesso condicional que não tem nenhum utilizadores e grupos atribuídos, nunca é acionada. |
| **Controlos de acesso** | Tem de selecionar o controlo de acesso de pelo menos um. | Se forem satisfeitas as condições, o processador de política tem de saber o que fazer.|




## <a name="what-you-should-know"></a>O que deve conhecer

### <a name="how-are-assignments-evaluated"></a>Como são avaliadas as atribuições?

Todas as atribuições de são logicamente **ANDed**. Se tiver mais do que uma atribuição configurada, todas as atribuições de devem ser satisfeitas para acionar uma política.  

Se precisar de configurar uma condição de localização que se aplica a todas as ligações efetuadas a partir de fora da rede da sua organização:

- Incluir **todas as localizações**
- Excluir **todos os IPs fidedignos**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>O que fazer se estiver bloqueado fora do portal de administração do Azure AD?

Se estiver bloqueado fora do portal do Azure AD devido a uma definição incorreta da política de acesso condicional:

- Certifique-se de que o se existem outros administradores na sua organização que não são bloqueados ainda. Um administrador com acesso ao portal do Azure pode desativar a política que está a afetar o seu início de sessão. 

- Se nenhum dos administradores na sua organização pode atualizar a política, tem de submeter um pedido de suporte. Suporte da Microsoft pode rever e atualizar as políticas de acesso condicional que estão a impedir o acesso.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>O que acontece se tiver políticas no portal do Azure configurada e portal clássico do Azure?  

Ambas as políticas são impostas pelo Azure Active Directory e o utilizador obtém acesso apenas quando todos os requisitos são cumpridos.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>O que acontece se tiver políticas no portal do Silverlight do Intune e o portal do Azure?

Ambas as políticas são impostas pelo Azure Active Directory e o utilizador obtém acesso apenas quando todos os requisitos são cumpridos.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>O que acontece se tiver várias políticas para o mesmo utilizador configurado?  

Para cada início de sessão, o Azure Active Directory avalia todas as políticas e garante que todos os requisitos são cumpridos antes de acesso concedido ao utilizador.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Acesso condicional funciona com o Exchange ActiveSync?

Sim, pode utilizar o Exchange ActiveSync numa política de acesso condicional.






## <a name="what-you-should-avoid-doing"></a>O que deve evitar fazê-lo

A estrutura de acesso condicional fornece-lhe uma flexibilidade de configuração excelente. No entanto, uma enorme flexibilidade significa também que cuidadosamente deve rever cada política de configuração antes de libertar para evitar resultados de indesejáveis. Neste contexto, deve prestar especial atenção a atribuições afetar conjuntos de conclusão, tais como **todos os utilizadores / grupos / aplicações em nuvem**.

No seu ambiente, deve evitar as seguintes configurações:


**Para todos os utilizadores, todas as aplicações em nuvem:**

- **Bloquear o acesso** -esta configuração bloqueia toda a organização, que não sem dúvida uma boa ideia.

- **Exigir conformidade do dispositivo** - os para os utilizadores que não inscreveram os dispositivos ainda, esta política bloqueia todo o acesso, incluindo o acesso ao portal do Intune. Se for um administrador sem um dispositivo inscrito, esta política bloqueia-o de voltar a obtê no portal do Azure para alterar a política.

- **Exigir a associação a domínio** - este bloco de política acesso também tem o potencial de bloquear o acesso para todos os utilizadores na sua organização se ainda não tem um dispositivo associado a um domínio.


**Para todos os utilizadores, todas as aplicações na nuvem, todas as plataformas de dispositivos:**

- **Bloquear o acesso** -esta configuração bloqueia toda a organização, que não sem dúvida uma boa ideia.


## <a name="how-should-you-deploy-a-new-policy"></a>Como implementar uma nova política deve?

Como primeiro passo, deve avaliar a política a utilizar o [e se a ferramenta](active-directory-conditional-access-whatif.md).

Quando estiver pronto para implementar uma nova política para o seu ambiente, deve fazê-lo nas fases:

1. Aplicar uma política para um pequeno conjunto de utilizadores e certifique-se de que esta se comporta conforme esperado. 

2.  Quando expande uma política de modo a incluir mais utilizadores, continue a excluir todos os administradores da política. Isto garante que os administradores ainda tem acesso e que podem atualizar uma política, se for necessária a uma alteração.

3. Aplica uma política a todos os utilizadores apenas se realmente necessário. 

Como melhor prática, crie uma conta de utilizador que seja:

- Dedicado à administração da política 
- Excluídos todas as políticas


## <a name="policy-migration"></a>Migração de política

Considere migrar as políticas que não tiver criado no portal do Azure porque:

- Agora que pode abordar cenários que não foi possível processar antes.

- Pode reduzir o número de políticas, que tem de gerir por consolidá-los.   

- Pode gerir todas as políticas de acesso condicional numa localização central.

- Portal clássico do Azure foi extinguido.   


Para obter mais informações, consulte [migrar políticas clássicas no portal do Azure](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Passos Seguintes

Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
