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
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Melhores práticas para acesso condicional no Azure Active Directory

Este tópico fornece informações sobre os aspetos que deve conhecer e o que é que deve evitar fazê-lo quando configurar políticas de acesso condicional. Antes de ler este tópico, deverá familiarizar-se com os conceitos e a terminologia descritos em [de acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>O que deve conhecer

### <a name="whats-required-to-make-a-policy-work"></a>O que tem necessário para tornar uma política de trabalho?

Quando cria uma nova política, não são utilizadores, grupos, aplicações ou controlos de acesso selecionados.

![Aplicações na cloud](./media/active-directory-conditional-access-best-practices/02.png)


Para tornar a sua política de trabalho, tem de configurar o seguinte:


|O que           | Como                                  | Por que razão|
|:--            | :--                                  | :-- |
|**Aplicações na nuvem** |Tem de selecionar uma ou mais aplicações.  | O objetivo de uma política de acesso condicional é permitir-lhe otimizar a forma como os utilizadores autorizados podem aceder às suas aplicações.|
| **Utilizadores e grupos** | Tem de selecionar, pelo menos, um utilizador ou grupo que esteja autorizado a aceder às aplicações em nuvem que selecionou. | Uma política de acesso condicional que não tem nenhum utilizadores e grupos atribuídos, nunca é acionada. |
| **Controlos de acesso** | Tem de selecionar o controlo de acesso de pelo menos um. | O processador de política tem de saber o que fazer se a condições forem satisfeitas.|


Para além destes requisitos básicos, em muitos casos, deve também configurar uma condição. Enquanto uma política também funciona sem uma condição configurada, as condições são o fator de despertar para fine-tuning aceder às suas aplicações.


![Aplicações na cloud](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Como são avaliadas as atribuições?

Todas as atribuições de são logicamente **ANDed**. Se tiver mais do que uma atribuição configurada, para acionar uma política, todas as atribuições de devem ser satisfeitas.  

Se precisar de configurar uma condição de localização que se aplica a todas as ligações efetuadas a partir de fora da rede da sua organização, pode conseguir isto por:

- Incluindo **todas as localizações**
- Excluindo **todos os IPs fidedignos**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>O que acontece se tiver políticas no portal do Azure configurada e portal clássico do Azure?  
Ambas as políticas são impostas pelo Azure Active Directory e o utilizador obtém acesso apenas quando todos os requisitos são cumpridos.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>O que acontece se tiver políticas no portal do Silverlight do Intune e o Portal do Azure?
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



## <a name="policy-migration"></a>Migração de política

Deve considerar a migrar as políticas que não tiver criado no portal do Azure porque:

- Agora que pode abordar cenários que não foi possível processar antes.

- Pode reduzir o número de políticas, que tem de gerir por consolidá-los.   

- Pode gerir todas as políticas de acesso condicional numa localização central.

- Portal clássico do Azure será descontinuado.   


Para obter mais informações, consulte [migrar políticas clássicas no portal do Azure](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Passos seguintes

Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
