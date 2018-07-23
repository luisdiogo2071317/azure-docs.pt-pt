---
title: Gerir zonas DNS no DNS do Azure - portal do Azure | Documentos da Microsoft
description: Pode gerir zonas DNS com o portal do Azure. Este artigo descreve como atualizar, excluir e criar zonas DNS no DNS do Azure
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: victorh
ms.openlocfilehash: ca9d03cb14e79b23ccc2021e0a31650eb9bbd95b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171243"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Como gerir zonas DNS no portal do Azure

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [CLI do Azure 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [CLI 2.0 do Azure](dns-operations-dnszones-cli.md)

Este artigo mostra-lhe como gerir as suas zonas DNS com o portal do Azure. Também pode gerir as suas zonas DNS com a Multiplataforma [CLI do Azure](dns-operations-dnszones-cli.md) ou do Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

1. Iniciar sessão no portal do Azure
2. No Hub menu, clique e clique em **criar um recurso > redes >** e, em seguida, clique em **zona DNS** para abrir o painel criar zona DNS.

    ![Zona DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. No painel **Criar zona DNS**, introduza os valores seguinte e clique em **Criar**:


   | **Definição** | **Valor** | **Detalhes** |
   |---|---|---|
   |**Nome**|contoso.com|O nome da zona DNS|
   |**Subscrição**|[A sua subscrição]|Selecione uma subscrição para criar a zona DNS.|
   |**Grupo de recursos**|**Criar novo:** contosoDNSRG|Crie um grupo de recursos. O nome do grupo de recursos tem de ser exclusivo dentro da subscrição que selecionou. Para saber mais sobre grupos de recursos, veja o artigo de descrição geral do [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Localização**|EUA Oeste||

> [!NOTE]
> Esta definição refere à localização do grupo de recursos e não tem qualquer impacto na zona DNS. A localização da zona DNS é sempre "global" e não está apresentada.

## <a name="list-dns-zones"></a>Listar zonas DNS

No portal do Azure, navegue até **mais serviços** > **rede** > **zonas DNS**. Cada zona DNS é é próprios recursos, informações como o número de conjuntos de registos e servidores de nomes são visíveis a partir desta vista. A coluna **servidores de nomes** não está na vista predefinida, adicioná-lo clique **colunas**, selecione **servidores de nomes** e clique em **feito**.

![listar zonas DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Eliminar uma zona DNS

Navegue para uma zona DNS no portal. Sobre o **zona DNS** painel, clique em **eliminar zona**. São-lhe pedido para confirmar que pretende eliminar a zona DNS. Eliminar uma zona DNS também elimina a todos os registos que estão contidos na zona.

## <a name="next-steps"></a>Passos Seguintes

Saiba como trabalhar com a sua zona DNS e registos ao visitar [introdução ao DNS do Azure com o portal do Azure](dns-getstarted-portal.md).