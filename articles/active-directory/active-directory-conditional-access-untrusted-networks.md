---
title: Como a - políticas de acesso condicional de configurar o Azure Active Directory para tentativas de acesso de redes não fidedignas | Microsoft Docs
description: Saiba como configurar uma política de acesso condicional no Azure Active Directory (Azure AD) para tentativas de acesso de redes não fidedignas.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 952a3a3952a96c7125e0b0dbe770b72c17a57101
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232516"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Como: Configurar políticas de acesso condicional para tentativas de acesso de redes não fidedignas   

No mundo mobile-primeiro, primeiro de nuvem, o Azure Active Directory (Azure AD) permite início de sessão único para dispositivos, aplicações e serviços em qualquer lugar. Como resultado, os utilizadores podem aceder as aplicações na nuvem não apenas a partir da rede da sua organização, mas também a partir de qualquer localização não fidedigna de Internet. Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar os utilizadores autorizados como pode aceder às suas aplicações em nuvem. É um requisito comuns neste contexto para controlar as tentativas de acesso iniciadas a partir de redes não fidedignas. Este artigo fornece-lhe as informações necessárias configurar uma política de acesso condicional que gere este requisito. 

## <a name="prerequisites"></a>Pré-requisitos

Este artigo assume que está familiarizado com: 

- Os conceitos básicos do acesso condicional do Azure AD 
- Configurar políticas de acesso condicional no portal do Azure

Veja:

- [O que é o acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md) - para uma descrição geral do acesso condicional 

- [Início rápido: Exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory](active-directory-conditional-access-app-based-mfa.md) - para obter alguma experiência com a configuração de políticas de acesso condicional. 


## <a name="scenario-description"></a>Descrição do cenário

A mestre o equilíbrio entre produtividade e segurança, poderá ser suficiente para que possa exigir apenas que o utilizador ser autenticado através de uma palavra-passe. No entanto, quando é efetuada uma tentativa de acesso a partir de uma localização de rede não fidedigna, há um maior risco de inícios de sessão não são realizadas por utilizadores legítimos. Lidar com esta preocupação, pode bloquear as tentativas de acesso de redes não fidedignas. Em alternativa, também pode exigir a autenticação multifator (MFA) para obter novamente adicional garantia que foi efetuada uma tentativa pelo proprietário legítimo da conta. 

Com o acesso condicional do Azure AD, pode resolver este requisito com uma única política que concede acesso: 

- Para aplicações em nuvem selecionado

- Para utilizadores e grupos selecionados  

- Exigir a autenticação multifator 

- Quando é efetuada uma tentativa de acesso de: 

    - Uma localização que não é fidedigna


## <a name="considerations"></a>Considerações

O desafio deste cenário é traduzir *quando é efetuada uma tentativa de acesso de uma localização que não é fidedigna* para uma condição de acesso condicional. Na política de acesso condicional, pode configurar o [condição localizações](active-directory-conditional-access-locations.md) para cenários de endereços que estão relacionados com as localizações de rede. A condição de localizações permite selecionar [denominado localizações](active-directory-conditional-access-locations.md#named-locations), que representam os agrupamentos lógicos de intervalos de endereços IP, países e regiões.  

Normalmente, a sua organização é proprietária um ou mais intervalos de endereços, por exemplo, 199.30.16.0 - 199.30.16.24.
Pode configurar uma localização com nome por:

- Especificar este intervalo (199.30.16.0/24) 

- Atribuir um nome descritivo como **rede empresarial** 


Em vez de tentar definir o que todas as localizações encontram-se que não são fidedignas, pode:

- Incluir 

    ![Acesso condicional](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Excluir todas as localizações fidedignas 

    ![Acesso condicional](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Implementação

Com a abordagem descrita neste artigo, agora pode configurar uma política de acesso condicional para localizações não fidedignas. Deve sempre testar a política antes de a disponibilizar para produção para se certificar de que funciona conforme esperado. Idealmente, deve efetuar os testes iniciais de um inquilino de teste se possível. Para obter mais informações, consulte [como deve implementar uma nova política](active-directory-conditional-access-best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Passos Seguintes

Se quiser saber mais sobre o acesso condicional, consulte o artigo [que é o acesso condicional no Azure Active Directory?](active-directory-conditional-access-azure-portal.md)