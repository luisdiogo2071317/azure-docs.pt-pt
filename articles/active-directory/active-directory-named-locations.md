---
title: "Configurar localizações nomeadas no Azure Active Directory | Microsoft Docs"
description: "Saiba como configurar localizações com nome."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3b7bd6f4bea111815f647af09ebaa868696b25bc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Configurar localizações nomeadas no Azure Active Directory

Com nomeado localizações, pode Etiquetar fidedignos intervalos de endereços IP na sua organização. Azure Active Directory utiliza localizações chamadas no contexto de:

- A deteção de [eventos de risco](active-directory-reporting-risk-events.md) para reduzir o número de falsos positivos comunicados.  

- [Acesso condicional baseado na localização](active-directory-conditional-access-locations.md).


Este artigo explica, como pode configurar com o nome localizações no seu ambiente.


## <a name="entry-points"></a>Pontos de entrada

Pode aceder à página de configuração da localização nomeado no **segurança** secção da página do Azure Active Directory, clicando em:

![Pontos de entrada](./media/active-directory-named-locations/34.png)

- **Acesso condicional:**

    - No **gerir** secção, clique em **denominado localizações**.
    
        ![O comando de localizações denominado](./media/active-directory-named-locations/06.png)

- **Risco inícios de sessão:**

    - Na barra de ferramentas na parte superior, clique em **adicionar conhecido intervalos de endereços IP**.

       ![O comando de localizações denominado](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Exemplo de configuração

**Para configurar uma localização com nome:**

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) como administrador global.

2. No painel esquerdo, clique em **do Azure Active Directory**.

    ![A ligação do Azure Active Directory no painel esquerdo](./media/active-directory-named-locations/01.png)

3. No **do Azure Active Directory** na página de **segurança** secção, clique em **acesso condicional**.

    ![O comando de acesso condicional](./media/active-directory-named-locations/05.png)


4. No **acesso condicional** na página de **gerir** secção, clique em **denominado localizações**.

    ![O comando de localizações denominado](./media/active-directory-named-locations/06.png)


5. No **denominado localizações** página, clique em **nova localização**.

    ![O novo comando de localização](./media/active-directory-named-locations/07.png)


6. No **novo** página, efetue o seguinte:

    ![O novo painel](./media/active-directory-named-locations/56.png)

    a. No **nome** caixa, escreva um nome para a sua localização com nome.

    b. No **intervalos IP** caixa, escreva um intervalo de IP. O intervalo de IP tem de ser o *encaminhamento-Classless Inter-Domain* formato (CIDR).  

    c. Clique em **Criar**.



## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte:

- [Acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- [Condições de localização do acesso condicional do Azure Active Directory](active-directory-conditional-access-locations.md)

- [Eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md).

- [Relatório de risco inícios de sessão no portal do Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md).  
