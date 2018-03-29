---
title: Página da aplicação não é apresentado corretamente para uma aplicação de Proxy de aplicações | Microsoft Docs
description: Orientações para quando a página não está corretamente apresentação de uma aplicação de Proxy de aplicação ter integrado com o Azure AD
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: asteen
ms.openlocfilehash: d187b545a486be28fc80e6baf8e58079ff94ec5e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Página da aplicação não é apresentado corretamente para uma aplicação de Proxy de aplicações

Este artigo ajudá-lo a resolver problemas com aplicações de Proxy de aplicações do Azure Active Directory quando navegar para a página, mas algo na página de não ter um aspeto correto.

## <a name="overview"></a>Descrição geral
Quando publica uma aplicação de Proxy de aplicações, apenas páginas na sua raiz estão acessíveis quando o acesso à aplicação. Se a página não apresentar corretamente, o URL interno de raiz utilizado para a aplicação poderá estar em falta alguns recursos de página. Para resolver, certifique-se tiver publicado *todos os* os recursos para a página como parte da sua aplicação.

Pode verificar este é o problema, abrindo o controlador de rede (por exemplo, o Fiddler ou F12 ferramentas no Internet Explorer/Edge), carregamento da página e está à procura de 404 erros. Indica que as páginas que atualmente não não possível localizar e ainda poderão ter de ser publicado.

Como um exemplo de neste caso, partem do princípio de ter publicado uma aplicação de despesas utilizando um URL interno de <http://myapps/expenses>, mas a aplicação utilizar a folha de estilos <http://myapps/style.css>. Neste caso, a folha de estilos não está publicada na sua aplicação, para carregar a aplicação de despesas gerar um erro 404 ao tentar carregar style.css. Neste exemplo, seria possível resolver o problema por publicar a aplicação com um URL interno de <http://myapp/> em vez disso.

## <a name="problems-with-publishing-as-one-application"></a>Problemas com a publicação como uma aplicação

Se não for possível publicar todos os recursos na mesma aplicação, terá de publicar a várias aplicações e ativar as ligações entre eles.

Para tal, é recomendável utilizar o [domínios personalizados](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) solução. No entanto, esta solução requer que o certificado que é proprietário para o seu domínio e as suas aplicações utilizam nomes de domínio completamente qualificado (FQDN). Para outras opções, consulte o [resolver problemas de ligações quebrada documentação](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Passos Seguintes
[Publicar aplicações através do Proxy de aplicações do Azure AD](application-proxy-publish-azure-portal.md)
