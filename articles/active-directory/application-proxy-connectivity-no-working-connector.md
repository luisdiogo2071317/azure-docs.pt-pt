---
title: Foi encontrado nenhum grupo de conector de trabalho para uma aplicação de Proxy de aplicações | Microsoft Docs
description: Resolver problemas que poderão surgir quando não existe nenhum trabalho conector num grupo de conector para a sua aplicação com o Proxy de aplicações do Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: e9e00dc199abe833c6a282ae7da7c209566a700f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335599"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Nenhum grupo de conector de trabalho encontrado para uma aplicação de Proxy de aplicações

Este artigo ajuda a resolver os problemas comuns deparam quando não existe um conector detetado para uma aplicação de Proxy de aplicações integrada com o Azure Active Directory.

## <a name="overview-of-steps"></a>Descrição geral dos passos
Se não houver nenhum trabalho conector num grupo de conector para a sua aplicação, existem algumas formas de resolver o problema:

-   Se tiver não conectores no grupo, pode:

    -   Transfira um novo conector no servidor de direita no local e atribua-a este grupo

    -   Mova um conector do Active Directory para o grupo

-   Se tiver não conectores Active Directory no grupo, pode:

    -   Identificar o motivo pelo qual que o conector está inativo e resolver

    -   Mova um conector do Active Directory para o grupo

Para descobrir o problema, abra o menu de "Proxy da aplicação" na sua aplicação e observe a mensagem de aviso do grupo de conector. Se não houver nenhuma conectores no grupo, a mensagem de aviso Especifica que o grupo tem de, pelo menos, um conector. Se tiver não conectores Active Directory, a mensagem de aviso explica que. É comum para conectores inativos. 

   ![Seleção de grupo do conector no portal do Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Para obter detalhes sobre cada uma destas opções, consulte a secção correspondente abaixo. As instruções partem do princípio de que estão a começar a página de gestão do conector. Se estiver à procura a mensagem de erro acima, pode aceder a esta página ao clicar na mensagem de aviso. Também pode aceder à página acedendo a **do Azure Active Directory**, clicando no **aplicações empresariais**, em seguida, **Proxy de aplicações.**

   ![Gestão de grupo do conector no portal do Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Transfira um novo conector

Para transferir um novo Conetor, utilize o botão "Transferir o conector" na parte superior da página.

Instale o conector num computador com a linha direta de visão, para a aplicação de back-end. Normalmente, o conector é instalado no mesmo servidor que a aplicação. Depois de transferir, o conector deve aparecer neste menu. Clique o conector e utilize o menu pendente "conector do grupo" para se certificar de que pertença ao grupo à direita. Guarde a alteração.

   ![Transferir o conector do portal do Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Mover um conector do Active Directory

Se tiver um conector do Active Directory deve pertencer ao grupo e com a linha de visão para a aplicação de back-end de destino, pode mover o conector para o grupo atribuído. Para tal, clique o conector. No campo "Grupo de conector", utilize a lista pendente para selecionar o grupo correto e clique em Guardar.

## <a name="resolve-an-inactive-connector"></a>Resolver um conector inativo

Se os conectores apenas no grupo estão inativos, que são provável num computador que não tem todas as portas necessárias desbloqueadas.

consulte o documento de resolução de problemas de portas para obter detalhes sobre a investigar o problema.

## <a name="next-steps"></a>Passos Seguintes
[Compreender os conectores de Proxy de aplicações do Azure AD](manage-apps/application-proxy-connectors.md)


