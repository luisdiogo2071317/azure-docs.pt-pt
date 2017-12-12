---
title: "Com o nome localizações no Azure Active Directory | Microsoft Docs"
description: "Saiba o que com o nome localizações são e como configurá-las."
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
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 231255d9a119c404c0c947c00414572aaab82719
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Localizações nomeadas no Azure Active Directory

Com nomeado localizações, pode Etiquetar fidedignos intervalos de endereços IP na sua organização. Azure Active Directory utiliza localizações chamadas no contexto de:

- A deteção de [eventos de risco](active-directory-reporting-risk-events.md) para reduzir o número de falsos positivos comunicados.  

- [Acesso condicional baseado na localização](active-directory-conditional-access-azure-portal.md#locations).


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



## <a name="what-you-should-know"></a>O que deve conhecer

**Em massa atualizações**: ao criar ou atualizar localizações com nome, para as atualizações em massa, pode carregar ou transferir um ficheiro CSV com os intervalos de IP. Um carregamento adiciona os intervalos de IP no ficheiro de lista em vez de substituir a lista.

![As ligações de carregamento e transferência](./media/active-directory-named-locations/09.png)


**Limitações**: pode definir um máximo de 60 localizações com nome, com um intervalo IP atribuído a cada um deles. Se tiver apenas uma localização com nome configurada, pode definir até 500 intervalos IP para o mesmo.


## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre:

- **Eventos de risco**, consulte [eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md).

- **Acesso condicional**, consulte [de acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- **Relatórios de inícios de sessão arriscados**, consulte [relatório arriscados inícios de sessão no portal do Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md).  
