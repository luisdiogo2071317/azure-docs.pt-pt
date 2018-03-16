---
title: "Descrição geral sobre os vários portais necessários para criar uma oferta para Marketplace | Microsoft Docs"
description: "Descrição geral sobre os vários portais necessários para criar uma oferta para o Marketplace"
services: marketplace-publishing
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 89ce82b3-c28a-4b0d-b37a-db3112160a4e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: mbaldwin
ms.openlocfilehash: 663d9a01b80b0c41e37037b0cbec10c93511d24c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="portals-you-will-need"></a>Terá de portais
Antes de começar o processo de publicação de uma oferta, vamos introduzida para os vários portais que irá precisar. Segue-se o resumo curto sobre os portais – Centro para programadores, o Portal de publicação do Azure e o Portal do Azure – pela ordem que irá interagir com eles.                                                                            

## <a name="developer-center"></a>Centro de Programadores
[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure)

### <a name="description"></a>Descrição
Criar a sua conta Microsoft Developer Center é uma única tarefa. Certifique-se de que a empresa ainda não tiver uma conta de centro de programadores antes de tentar criar um. Durante o processo, podemos recolher as informações de endereço de empresa, informações de dedução dos impostos e informações da conta de bank.

> [!NOTE]
> Se estiverem a publicar ofertas apenas livres (ou bring-your-proprietário-licença oferece), não Requeremos informações dedução dos impostos e bank.
> 
> 

### <a name="identityaccount-used"></a>Identidade/conta utilizada
Idealmente, esta é uma lista de distribuição ou um grupo de segurança (por exemplo, azurepublishing @*partnercompany*empresa>.com). O grupo de segurança ou de lista de distribuição **tem** ser registado como uma conta Microsoft.

> [!TIP]
> Recomendamos que utilize uma lista de distribuição ou um grupo de segurança porque remove a dependência de qualquer individuais, embora também pode ser utilizada uma conta individuais.
> 
> 

## <a name="publishing-portal"></a>Portal de publicação
[https://publish.windowsazure.com](https://publish.windowsazure.com)

### <a name="description"></a>Descrição
Este é o portal que utilizar para desempenhar a oferta e para publicá-lo (marketing preços, publicação, certificação, se aplicável, etc.).

### <a name="identityaccount-used"></a>Identidade/conta utilizada
O grupo de segurança ou de lista de distribuição acima deve ser utilizado pela primeira vez para iniciar sessão portal de publicação. Mais tarde, outros utilizadores podem ser adicionados como coadministradores. Esta é a como obtém mapeado para os dados de registo do Centro de programadores.

## <a name="azure-portal"></a>Portal do Azure
[https://portal.azure.com](https://portal.azure.com)

### <a name="description"></a>Descrição
Este é o portal onde pode ver as ofertas testadas e publicadas no Azure Marketplace (aplicável para VMs, modelos de solução e serviços de programação baseada no Azure Resource Manager).

### <a name="identityaccount-used"></a>Identidade/conta utilizada
Enquanto estiver a testar uma oferta a partir do portal da publicação, um ID de subscrição tem de estar na lista de permissões. Da mesma subscrição (há um nome de utilizador e palavra-passe associada ao mesmo) tem de ser utilizado para iniciar sessão neste portal para testar a oferta faseada.

## <a name="see-also"></a>Consulte também
* [Introdução: como publicar uma oferta para o Azure Marketplace](marketplace-publishing-getting-started.md)

