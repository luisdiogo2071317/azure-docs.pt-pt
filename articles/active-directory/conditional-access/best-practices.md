---
title: Melhores práticas para acesso condicional no Azure Active Directory | Documentos da Microsoft
description: Saiba mais sobre os aspetos que deve saber, e o que é que deve evitar ao fazê-lo quando configurar políticas de acesso condicional.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: a671cf76e52ee8fd6ac0c436a905f3270cbc6db1
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578335"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Melhores práticas para acesso condicional no Azure Active Directory

Com o [acesso condicional do Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), pode controlar o acesso de utilizadores autorizados como suas aplicações na cloud. Este artigo fornece informações sobre:

- Coisas que deve saber 
- O que é deve evitar quando configurar políticas de acesso condicional. 

Este artigo pressupõe que familiar os conceitos e terminologia descritos no [o que é o acesso condicional no Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>O que é necessário para tornar uma política de trabalho?

Quando cria uma nova política, não são os utilizadores, grupos, aplicações ou controlos de acesso selecionados.

![Aplicações na cloud](./media/best-practices/02.png)


Para tornar a sua política de trabalho, tem de configurar:


|O quê           | Como                                  | Por que motivo|
|:--            | :--                                  | :-- |
|**Aplicações na cloud** |Tem de selecionar uma ou mais aplicações.  | O objetivo de uma política de acesso condicional é que lhe permite controlar os utilizadores autorizados como podem aceder a aplicações na cloud.|
| **Utilizadores e grupos** | Tem de selecionar pelo menos um utilizador ou grupo que está autorizado a aceder às suas aplicações na nuvem selecionada. | Uma política de acesso condicional que tenha de não utilizadores e grupos atribuídos, nunca é acionado. |
| **Controlos de acesso** | Tem de selecionar o controlo de acesso de pelo menos um. | Se forem satisfeitas as condições, o processador de política tem de saber o que fazer.|




## <a name="what-you-should-know"></a>O que deve saber

### <a name="how-are-assignments-evaluated"></a>Como são avaliadas as atribuições?

Todas as atribuições são logicamente **ANDed**. Se tiver mais de uma atribuição configurada, todas as atribuições de devem ser satisfeitas para acionar uma política.  

Se precisar de configurar uma condição de localização que se aplica a todas as ligações feitas a partir de fora da rede da sua organização:

- Incluir **todas as localizações**
- Excluir **todos os IPs fidedignos**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>O que fazer se estiver impedido de aceder ao portal de administração do Azure AD?

Se estiver impedido de aceder ao portal do Azure AD devido a uma definição incorreta numa política de acesso condicional:

- Verifique se existem outros administradores na sua organização que não estão bloqueadas ainda. Um administrador com acesso ao portal do Azure pode desativar a política que está a afetar o seu início de sessão. 

- Se nenhum dos administradores na sua organização pode atualizar a política, tem de submeter um pedido de suporte. Suporte da Microsoft pode rever e atualizar as políticas de acesso condicional que estão a impedir o acesso.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>O que acontece se tiver políticas no portal clássico do Azure e no portal do Azure configurado?  

Ambas as políticas são aplicadas pelo Azure Active Directory e o utilizador obtém acesso apenas quando todos os requisitos são cumpridos.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>O que acontece se tiver políticas no portal do Intune Silverlight e o portal do Azure?

Ambas as políticas são aplicadas pelo Azure Active Directory e o utilizador obtém acesso apenas quando todos os requisitos são cumpridos.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>O que acontece se tiver múltiplas políticas para o mesmo utilizador configurado?  

Para cada início de sessão, o Azure Active Directory avalia todas as políticas e garante que todos os requisitos são cumpridos antes de acesso concedido ao utilizador.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Acesso condicional funciona com o Exchange ActiveSync?

Sim, pode utilizar o Exchange ActiveSync numa política de acesso condicional.






## <a name="what-you-should-avoid-doing"></a>O que deve evitar ao fazê-lo

A estrutura de acesso condicional fornece uma flexibilidade de configuração grande. No entanto, grande flexibilidade também significa que deve examinar cuidadosamente cada política de configuração antes da sua liberação para evitar resultados indesejáveis. Neste contexto, deve prestar atenção especial para atribuições de afetar conjuntos completos, como **todos os utilizadores / grupos / aplicações na cloud**.

No seu ambiente, deve evitar as seguintes configurações:


**Para todos os utilizadores, todas as aplicações na cloud:**

- **Bloquear o acesso** -esta configuração bloqueia toda a organização, que definitivamente não é uma boa idéia.

- **Exigir dispositivo em conformidade** - os para os utilizadores que não inscreveram os dispositivos ainda, esta política bloqueia todos os acessos, incluindo o acesso ao portal do Intune. Se for um administrador sem um dispositivo inscrito, esta política bloqueia-o partir voltando para o portal do Azure para alterar a política.

- **Exigir a associação a domínio** – este bloco de política acesso também tem o potencial para bloquear o acesso para todos os utilizadores na sua organização se ainda não tiver um dispositivo associado a um domínio.


**Para todos os utilizadores, todas as aplicações na cloud, todas as plataformas de dispositivos:**

- **Bloquear o acesso** -esta configuração bloqueia toda a organização, que definitivamente não é uma boa idéia.


## <a name="how-should-you-deploy-a-new-policy"></a>Como deve implementar uma nova política?

Como primeiro passo, deve avaliar sua política através da [e se a ferramenta](what-if-tool.md).

Quando estiver pronto para implementar uma nova política para o seu ambiente, deve fazê-lo nas fases:

1. Aplicar uma política a um pequeno conjunto de utilizadores e certifique-se de que funciona conforme o esperado. 

2.  Quando expande uma política para incluir mais usuários, continue excluir todos os administradores da política. Isso garante que os administradores ainda tem acesso de colunas e podem atualizar uma política, se for necessária uma alteração.

3. Aplica uma política a todos os utilizadores só se isso for realmente necessário. 

Como melhor prática, crie uma conta de utilizador que seja:

- Dedicada à administração da diretiva 
- Excluído da todas as suas políticas


## <a name="policy-migration"></a>Migração da política

Considere migrar as políticas que não tenha criado no portal do Azure porque:

- Agora pode solucionar cenários que não poderia manipular antes.

- Pode reduzir o número de políticas que tem de gerir por consolidá-las.   

- Pode gerir todas as suas políticas de acesso condicional num local central.

- Portal clássico do Azure foi desativado.   


Para obter mais informações, consulte [migrar políticas clássicas no portal do Azure](policy-migration.md).


## <a name="next-steps"></a>Passos Seguintes

Se quiser saber como configurar uma política de acesso condicional, consulte [exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory](app-based-mfa.md).
