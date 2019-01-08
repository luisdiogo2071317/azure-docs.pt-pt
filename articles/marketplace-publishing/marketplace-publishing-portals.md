---
title: Descrição geral dos vários portais necessários criar uma oferta do Marketplace | Documentos da Microsoft
description: Descrição geral dos vários portais necessários criar uma oferta do Marketplace
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 89ce82b3-c28a-4b0d-b37a-db3112160a4e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: d1cc0efa1da90d90bd035eaa495a1336b6ff96c2
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074194"
---
# <a name="portals-you-will-need"></a>Precisará de portais

Antes de começar o processo de publicação de uma oferta, vamos ajudá-lo introduzido para os vários portais que irá precisar. Segue-se a breve resumo sobre os portais – Developer Center, o Portal de publicação do Azure e o portal do Azure - na ordem que irão interagir com eles.                                                                            

## <a name="developer-center"></a>Centro de Programadores
[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)

### <a name="description"></a>Descrição
Criar a sua conta do Microsoft Developer Center é uma tarefa de uso individual. Certifique-se de que a empresa ainda não tiver uma conta do Centro de programadores antes de tentar criar uma. Durante o processo, podemos recolher informações de conta bancária, informações fiscais e informações de endereço da empresa.

> [!NOTE]
> Se está a publicar ofertas gratuitas apenas (ou oferece bring-your-own-license), não é necessário informações fiscais e bancárias.
> 
> 

### <a name="identityaccount-used"></a>Conta/identidade utilizada
Idealmente, este valor é uma lista de distribuição ou um grupo de segurança (por exemplo, azurepublishing @*partnercompany*.com). O grupo de segurança ou lista de distribuição **tem** ser registado como uma conta Microsoft.

> [!TIP]
> Recomendamos que utilize uma lista de distribuição ou um grupo de segurança porque remove a dependência de qualquer pessoa, embora uma conta individual pode ser usada também.
> 
> 

## <a name="publishing-portal"></a>Portal de publicação
[https://publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Descrição

Este valor é o portal que utiliza para trabalhar sobre a oferta e publicá-lo (de marketing, preços, publicar, certificação, se aplicável, etc).

### <a name="identityaccount-used"></a>Conta/identidade utilizada
O grupo de segurança ou lista de distribuição acima deve ser utilizado pela primeira vez para iniciar sessão no portal de publicação. Mais tarde, os outros utilizadores podem ser adicionados como coadministradores. Esta lista é como ele é mapeado para os dados de registo do Centro de programadores.

## <a name="azure-portal"></a>Portal do Azure
[Portal do Azure](https://portal.azure.com)

### <a name="description"></a>Descrição
Este valor é o portal onde pode ver as ofertas de testada e publicadas no Azure Marketplace (aplicável a VMs, modelos de soluções e serviços de programação com base no Azure Resource Manager).

### <a name="identityaccount-used"></a>Conta/identidade utilizada
Enquanto estiver a testar uma oferta a partir do portal de publicação, um ID de subscrição tem de estar na lista de permissões. Da mesma subscrição (há um nome de utilizador e palavra-passe associada ele) tem de ser utilizada para iniciar sessão neste portal para testar a oferta em etapas.

## <a name="see-also"></a>Consulte também
* [Introdução: Como publicar uma oferta para o Azure Marketplace](marketplace-publishing-getting-started.md)

