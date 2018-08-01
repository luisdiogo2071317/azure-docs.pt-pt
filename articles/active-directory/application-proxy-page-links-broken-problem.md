---
title: Links na página não funcionam com uma aplicação de Proxy de aplicações | Documentos da Microsoft
description: Como resolver problemas com ligações quebradas em aplicações de Proxy de aplicações que tiver integrado com o Azure AD
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
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 09fa527f8a1e03d568dec67369651bbcee2a26f2
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365826"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Links na página não funcionam com uma aplicação de Proxy de aplicações

Este artigo ajuda-o a resolver o motivo pelo qual os links na sua aplicação de Proxy de aplicações do Azure Active Directory não funcionam corretamente.

## <a name="overview"></a>Descrição geral 
Depois de publicar uma aplicação de Proxy de aplicações, as ligações só que funcionam por predefinição no aplicativo são links para destinos contidos o URL de raiz publicados. As ligações dentro as aplicações não estão funcionando, o URL interno para o aplicativo provavelmente não inclui todos os destinos de ligações na aplicação.

**Por que isso acontece?** Quando clicar num link num aplicativo, o Proxy de aplicações tenta resolver o URL como um URL interno dentro do mesmo aplicativo ou como um URL disponível externamente. Se o link apontar para um URL interno que não está dentro do mesmo aplicativo, ele não pertencem a nenhum destes registos e resultar num erro não encontrado.

## <a name="ways-you-can-resolve-broken-links"></a>Formas que o possa resolver quebrada ligações

Existem três formas de resolver este problema. As opções abaixo no constam aumentar a complexidade.

1.  Certifique-se de que a URL interna é uma raiz que contém todas as ligações relevantes para a aplicação. Isso permite que todas as ligações que seja resolvido como conteúdo publicado dentro do mesmo aplicativo.

    Se alterar o URL interno, mas não deseja alterar a página de destino para os utilizadores, altere o URL da Home page para o URL interno publicado anteriormente. Isso pode ser feito ao aceder ao "Azure Active Directory" -&gt; registos de aplicações -&gt; selecione a aplicação -&gt; propriedades. Neste separador de propriedades, verá o campo "Home Page do URL", o que pode fazer para ser a página de destino pretendido.

2.  Se as aplicações utilizam nomes de domínio completamente qualificado (FQDN), utilize [domínios personalizados](manage-apps/application-proxy-configure-custom-domain.md) para publicar as suas aplicações. Esta funcionalidade permite que o mesmo URL a ser utilizado tanto internamente e externamente.

    Essa opção assegura que as ligações na sua aplicação estão acessíveis externamente por meio do Proxy de aplicações, uma vez que as ligações na aplicação para os URLs internos também são reconhecidas externamente. Todas as ligações ainda têm de pertencer a uma aplicação publicada. No entanto, com esta opção as ligações não têm de pertencer à mesma aplicação e podem pertencer a várias aplicações.

3.  Se nenhuma dessas opções são viáveis, pode visualizar um novo recurso que faz a tradução/reescrever URL. Com esta funcionalidade, interno URLs ligações existentes no corpo do HTML de seus aplicativos são traduzidas ou "mapeadas", para os URLs de Proxy da aplicação externa publicados. Essa conversão só funciona em ligações no HTML ou CSS e não ajuda se a ligação é gerada por meio de JS. 

Como resultado, é altamente recomendável utilizar o [domínios personalizados](manage-apps/application-proxy-configure-custom-domain.md) solução se possível. Se quiser aderir à pré-visualização, envie um e-mail <aadapfeedback@microsoft.com> com o applicationId(s).

## <a name="next-steps"></a>Passos Seguintes
[Trabalhar com servidores de proxy no local existentes](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)

