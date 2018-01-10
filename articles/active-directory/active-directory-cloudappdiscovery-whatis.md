---
title: "Localizar aplicações da nuvem não geridos com o Cloud App Discovery no Azure Active Directory | Microsoft Docs"
description: "Fornece informações sobre como localizar e gerir aplicações com o Cloud App Discovery, quais são as vantagens e como funciona."
services: active-directory
keywords: "o cloud app discovery, gestão de aplicações"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1d0ad06fc7eec07f8e1e0ba47121b6eec01c87df
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="find-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Localizar aplicações da nuvem não geridos com o Cloud App Discovery
## <a name="summary"></a>Resumo

O cloud App Discovery no Azure Active Directory fornece agora uma experiência de sem agente de deteção avançada com tecnologia Microsoft Cloud App Security. Para utilizar a Cloud App Discovery, basta iniciar sessão com as suas credenciais do Azure AD Premium P1. Esta atualização está disponível sem custos adicionais para todos os clientes do Azure AD Premium P1. Introdução ao artigo [configurar Cloud App Discovery no Azure AD](https://docs.microsoft.com/azure/active-directory/cloudappdiscovery-get-started), em seguida, experimentar [Microsoft Cloud App Security](https://portal.cloudappsecurity.com/).

> [!IMPORTANT] 
> Eliminar o Azure AD Cloud App Discovery experiência com a deteção de agente está a ser desativada em 5 de Março de 2018, após o qual os agentes serão desativadas e dados atual. Execute a ação antes 5th de Março para começar a trabalhar na nova experiência para evitar a interrupção do serviço.  
 
**Com o Cloud App Discovery, pode:**

* Localize as aplicações em nuvem que está a ser utilizadas e medida que a utilização de pelo número de utilizadores, volume de tráfego ou número de pedidos da web para a aplicação.
* Identifique os utilizadores que estão a utilizar uma aplicação.
* Exporte dados para análise offline.
* Coloque estas aplicações sob controlo de TI e ativar o início de sessão para gestão de utilizadores.

## <a name="how-it-works"></a>Como funciona
1. Se encontram instalados agentes de utilização de aplicações nos computadores do utilizador.
2. As informações de utilização de aplicação capturadas pelos agentes são enviadas através de um canal seguro e encriptado para o serviço de deteção de aplicações em nuvem.
3. O serviço de Cloud App Discovery avalia os dados e gera relatórios.

![Diagrama do cloud App Discovery](./media/active-directory-cloudappdiscovery/cad01.png)


## <a name="next-steps"></a>Passos Seguintes
* [Considerações de privacidade e segurança do cloud App Discovery](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Definições de registo do cloud App Discovery para servidores Proxy com portas personalizadas](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Changelog de agente do cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)

