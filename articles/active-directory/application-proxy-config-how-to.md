---
title: Como configurar uma aplicação de Proxy de aplicações | Microsoft Docs
description: Saiba como criar uma configurar uma aplicação de Proxy de aplicações em alguns passos simples
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
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 047679b52de1b095112948e869f35811346b846b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36331628"
---
# <a name="how-to-configure-an-application-proxy-application"></a>Como configurar uma aplicação de Proxy de aplicações

Este artigo ajuda-o a compreender como configurar uma aplicação de Proxy de aplicações dentro do Azure AD para expor as suas aplicações no local para a nuvem.

## <a name="recommended-documents"></a>Documentos recomendados 

Para saber mais sobre as configurações iniciais e a criação de uma aplicação de Proxy de aplicações através do Portal de administração, siga o [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md).

Para obter mais informações sobre como configurar conectores, consulte [ativar o Proxy da aplicação no portal do Azure](manage-apps/application-proxy-enable.md).

Para informações sobre como carregar certificados e a utilização de domínios personalizados, consulte [trabalhar com domínios personalizados no Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-custom-domain.md).

## <a name="create-the-applicationsetting-the-urls"></a>Criar a aplicação/definição os URLs

Se estiver a seguir os passos a [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md) tem e a documentação de obter um erro ao criar a aplicação, consulte os detalhes do erro para obter informações e sugestões para saber como corrigir o aplicação. A maioria das mensagens de erro incluem uma correção sugerida. Para evitar erros comuns, certifique-se:

-   É um administrador com permissão para criar uma aplicação de Proxy de aplicações

-   O URL interno é exclusivo

-   O URL externo é exclusivo

-   Os URLs começar a utilizar http ou https e terminar com um "/"

-   O URL deve ser um nome de domínio, não um endereço IP

A mensagem de erro deverá apresentar no canto superior direito ao criar a aplicação. Também pode selecionar o ícone de notificação para ver as mensagens de erro.

   ![Pedido de notificação](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Configurar grupos de conectores/conector

Se estiver a ter dificuldade em configurar a sua aplicação devido a aviso sobre os conectores e grupos de conector, consulte as instruções sobre como ativar o Proxy da aplicação para obter detalhes sobre como transferir conectores. Se pretender obter mais informações sobre conectores, consulte o [documentação de conectores](manage-apps/application-proxy-connectors.md).

Se os conectores estão inativos, isto significa que não conseguem alcançar o serviço. Isto é, muitas vezes, porque todas as portas necessárias não estão abertas. Para ver uma lista das portas necessárias, consulte a secção de pré-requisitos da documentação ativar Proxy de aplicações.

## <a name="upload-certificates-for-custom-domains"></a>Carregar certificados para domínios personalizados

Domínios personalizados permitem-lhe especificar o domínio da sua URLs externos. Para utilizar domínios personalizados, terá de carregar o certificado para esse domínio. Para obter informações sobre como utilizar domínios e certificados personalizados, consulte [trabalhar com domínios personalizados no Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-custom-domain.md). 

Se tiver problemas com a carregar o certificado, procure as mensagens de erro no portal para obter informações adicionais sobre o problema com o certificado. Problemas de certificado comuns incluem:

-   Certificado expirado

-   O certificado é autoassinado

-   Certificado está em falta a chave privada

Apresentar a mensagem de erro no canto superior direito, como a tenta carregar o certificado. Também pode selecionar o ícone de notificação para ver as mensagens de erro.

   ![Pedido de notificação](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Passos Seguintes
[Publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md)
