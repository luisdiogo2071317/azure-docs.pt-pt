---
title: Como-políticas de acesso condicional de configurar o Azure Active Directory para tentativas de acesso de redes não fidedignas | Documentos da Microsoft
description: Saiba como configurar uma política de acesso condicional no Azure Active Directory (Azure AD) para tentativas de acesso a partir de redes não confiáveis.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: conditional-access
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5ddde65b2a68e71d86af6ce3dcd2847736cf5823
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627190"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Como: Configurar políticas de acesso condicional para tentativas de acesso de redes não fidedignas   

Num mundo de dispositivos móveis e da cloud, Azure Active Directory (Azure AD) permite início de sessão único para dispositivos, aplicações e serviços de qualquer lugar. Como resultado, os utilizadores podem aceder as aplicações na cloud, não apenas da rede da sua organização, mas também a partir de qualquer localização não fidedigna do Internet. Com o [acesso condicional do Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), pode controlar os utilizadores autorizados como pode aceder às suas aplicações na cloud. É um requisito comum neste contexto controlar as tentativas de acesso iniciadas a partir de redes não fidedignas. Este artigo fornece as informações necessárias configurar uma política de acesso condicional que lida com esse requisito. 

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com: 

- Os conceitos básicos do acesso condicional do Azure AD 
- Configurar políticas de acesso condicional no portal do Azure

Veja:

- [O que é o acesso condicional no Azure Active Directory](../active-directory-conditional-access-azure-portal.md) – para uma visão geral do acesso condicional 

- [Início rápido: Exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory](app-based-mfa.md) - para obter alguma experiência com a configuração de políticas de acesso condicional. 


## <a name="scenario-description"></a>Descrição do cenário

Dominar o equilíbrio entre segurança e produtividade, poderá ser suficiente para exigir que apenas o utilizador ser autenticado através de uma palavra-passe. No entanto, quando é efetuada uma tentativa de acesso a partir de uma localização de rede não fidedigna, há um maior risco de inícios de sessão não são realizadas por utilizadores legítimos. Para abordar esta questão, pode bloquear tentativas de acesso a partir de redes não confiáveis. Em alternativa, pode também exigir a autenticação multifator (MFA) para obter a garantia de adicional que foi efetuada uma tentativa pelo legítimo proprietário da conta. 

Com acesso condicional do Azure AD, pode solucionar esse requisito com uma única política que concede acesso: 

- Para aplicações na cloud selecionada

- Para utilizadores e grupos selecionados  

- Exigir autenticação multifator 

- Quando é efetuada uma tentativa de acesso de: 

    - Uma localização que não é fidedigna


## <a name="considerations"></a>Considerações

O desafio desse cenário é traduzir *quando é efetuada uma tentativa de acesso de um local que não é fidedigno* numa condição de acesso condicional. Na política de acesso condicional, pode configurar o [condição de localizações](location-condition.md) cenários relacionados com a localizações de rede. A condição de localizações permite-lhe selecionar localizações com nome, que representam os agrupamentos lógicos de intervalos de endereços IP, países e regiões.  

Normalmente, a sua organização é proprietária um ou mais intervalos de endereços, por exemplo, 199.30.16.0 - 199.30.16.24.
Pode configurar um local nomeado por:

- Especificar este intervalo (199.30.16.0/24) 

- Atribuir um nome descritivo como **rede empresarial** 


Em vez de tentar definir o que todos os locais são que não são confiáveis, pode:

- Incluir 

    ![Acesso condicional](./media/untrusted-networks/02.png)

- Excluir todas as localizações fidedignas 

    ![Acesso condicional](./media/untrusted-networks/01.png)



## <a name="implementation"></a>Implementação

Com a abordagem descrita neste artigo, pode configurar uma política de acesso condicional para localizações não fidedignas. Sempre deve testar a política antes de distribuí-lo em produção para se certificar de que ele funciona conforme esperado. Idealmente, deve fazer seus testes iniciais num inquilino de teste se possível. Para obter mais informações, consulte [como deve implementar uma nova política](best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Passos Seguintes

Se gostaria de saber mais sobre o acesso condicional, consulte o artigo [o que é o acesso condicional no Azure Active Directory?](../active-directory-conditional-access-azure-portal.md)