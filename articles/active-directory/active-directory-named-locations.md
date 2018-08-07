---
title: Configurar localizações com nome no Azure Active Directory | Documentos da Microsoft
description: Saiba como configurar localizações com nome.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.component: protection
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 497c3b54238c9e38144a5840b59cac38af581f39
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525931"
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Configurar localizações com nome no Azure Active Directory

Com localizações com nome, pode etiquetar os intervalos de endereços IP fidedignos na sua organização. O Azure Active Directory utiliza localizações com nome no contexto de:

- A detecção de [eventos de risco](active-directory-reporting-risk-events.md) para reduzir o número de comunicadas falsos positivos.  

- [Acesso condicional com base na localização](conditional-access/location-condition.md).


Este artigo explica, como pode configurar localizações com nome no seu ambiente.


## <a name="entry-points"></a>Pontos de entrada

Pode acessar a página de configuração local nomeado no **segurança** secção da página do Azure Active Directory ao clicar em:

![Pontos de entrada](./media/active-directory-named-locations/34.png)

- **Acesso condicional:**

    - Na **Manage** secção, clique em **localizações com nome**.
    
        ![O comando de localizações com nome](./media/active-directory-named-locations/06.png)

- **Risco inícios de sessão:**

    - Na barra de ferramentas na parte superior, clique em **adicionar conhecido intervalos de endereços IP**.

       ![O comando de localizações com nome](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Exemplo de configuração

**Para configurar um local nomeado:**

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) como administrador global.

2. No painel esquerdo, clique em **do Azure Active Directory**.

    ![A ligação do Azure Active Directory no painel esquerdo](./media/active-directory-named-locations/01.png)

3. Na **do Azure Active Directory** página, além do **segurança** secção, clique em **acesso condicional**.

    ![O comando de acesso condicional](./media/active-directory-named-locations/05.png)


4. Na **acesso condicional** página, além da **gerir** secção, clique em **localizações com nome**.

    ![O comando de localizações com nome](./media/active-directory-named-locations/06.png)


5. Sobre o **localizações com nome** página, clique em **nova localização**.

    ![O comando de localização nova](./media/active-directory-named-locations/07.png)


6. Sobre o **New** página, efetue o seguinte:

    ![O painel do novo](./media/active-directory-named-locations/61.png)

    a. Na **nome** , escreva um nome para a sua localização com nome.

    b. Na **intervalos de IP** , escreva um intervalo de IP. O intervalo de IP tem de constar da *Classless Inter-Domain Routing* formato (CIDR).  

    c. Clique em **Criar**.



## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte:

- [Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- [Condições de localização no acesso condicional do Azure Active Directory](conditional-access/location-condition.md)

- [Eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md).

- [Relatório de inícios de sessão de risco no portal do Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md).  
