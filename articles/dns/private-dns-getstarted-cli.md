---
title: Introdução às Zonas Privadas do DNS do Azure com a CLI 2.0 do Azure | Microsoft Docs
description: Saiba como criar uma zona DNS Privada e o registo no DNS do Azure. Este é um guia passo a passo para criar e gerir a sua primeira zona DNS Privada e registar com a CLI 2.0 do Azure.
services: dns
documentationcenter: na
author: KumuD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: d10f3201adb972468d8de7e66a940232ed19562d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30191454"
---
# <a name="get-started-with-azure-dns-private-zones-using-azure-cli-20"></a>Introdução às Zonas Privadas do DNS do Azure com a CLI 2.0 do Azure

> [!div class="op_single_selector"]
> * [PowerShell](private-dns-getstarted-powershell.md)
> * [CLI 2.0 do Azure](private-dns-getstarted-cli.md)

Este artigo explica-lhe os passos para criar a primeira zona DNS privada e registar com a CLI 2.0 do Azure de várias plataformas, que está disponível para Windows, Mac e Linux. Também pode executar estes passos com o Azure PowerShell.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Uma zona DNS é utilizada para alojar os registos DNS para um determinado domínio. Para começar a alojar o seu domínio no DNS do Azure, tem de criar uma zona DNS para esse nome de domínio. Cada registo DNS para o seu domínio é então criado no interior desta zona DNS. Para publicar uma zona DNS privada na sua rede virtual, especifique a lista de redes virtuais autorizadas a resolver registos na zona.  São denominadas “redes virtuais de resolução”.  Também pode especificar redes virtuais para as quais o DNS do Azure mantém registos de nomes de anfitrião sempre que uma VM for criada, mudar de IP ou for destruída.  Chamamos a isto uma “rede virtual de registo”.

Estas instruções partem do princípio de que já instalou e iniciou sessão na CLI 2.0 do Azure, e que também instalou a extensão da CLI necessária para o suporte às Zonas Privadas. Para obter ajuda, consulte [How to manage DNS zones using Azure CLI 2.0 (Como gerir zonas DNS com a CLI 2.0 do Azure)](dns-operations-dnszones-cli.md).

## <a name="to-installuse-azure-dns-private-zones-feature-public-preview"></a>Para instalar/utilizar a funcionalidade Zonas Privadas do DNS do Azure (Pré-visualização Pública)
A funcionalidade de Zona Privada do DNS do Azure foi lançada em Pré-visualização Pública através de uma extensão para a CLI do Azure. Instalar a extensão “dns” da CLI do Azure 

```
az extension add --name dns
``` 

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de criar a zona DNS, um grupo de recursos é criado para conter a zona DNS. O seguinte mostra o comando.

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-private-zone"></a>Criar uma zona DNS privada

Uma zona DNS privada é criada ao utilizar o comando `az network dns zone create`. Para ver a ajuda deste comando, escreva `az network dns zone create --help`.

O exemplo seguinte cria uma zona privada de DNS com o nome *contoso.local* no grupo de recursos *MyResourceGroup* e disponibiliza (liga a) a rede virtual *MyAzureVnet* com o parâmetro resolution-vnets. Utilize o exemplo para criar uma zona DNS, substituindo os valores pelos seus.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --resolution-vnets MyAzureVnet
```

Nota: no exemplo acima, a rede virtual “MyAzureVnet” pertence ao mesmo Grupo de Recursos e Subscrição da zona privada. Se precisar de ligar uma rede virtual que pertença a um Grupo de Recursos ou Subscrição diferente, tem de especificar o ID completo do Azure Resource Manager, em vez de apenas o nome da rede virtual, para o parâmetro – resolution-vnets. 

Se precisar que o Azure crie automaticamente registos de nomes de anfitrião na zona, utilize o parâmetro *registration-vnets*, em vez de *resolution-vnets*.  As redes virtuais de registo são ativadas automaticamente para resolução.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --registration-vnets MyAzureVnet
```

## <a name="create-a-dns-record"></a>Criar um registo DNS

Para criar um registo DNS, utilize o comando `az network dns record-set [record type] add-record`. Para obter ajuda, para um registo A, por exemplo, consulte `azure network dns record-set A add-record --help`.

O exemplo seguinte cria um registo com o nome relativo “ip1” na Zona DNS “contoso.com”, no grupo de recursos “MyResourceGroup”. O nome totalmente qualificado do conjunto de registos é “ip1.contoso.local”. O tipo de registo é “A”, com o endereço IP “10.0.0.1”.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.local -n ip1 -a 10.0.0.1
```

Para outros tipos de registos, para conjuntos de registos com mais de um registo, para os valores de TTL alternativos e para modificar registos existentes, consulte [Manage DNS records and record sets using the Azure CLI 2.0 (Gerir registos DNS e conjuntos de registos com a CLI 2.0 do Azure)](dns-operations-recordsets-cli.md).

## <a name="view-records"></a>Ver registos

Para listar os registos DNS na sua zona, utilize:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="get-a-dns-private-zone"></a>Obter uma zona DNS privada

Para obter uma zona DNS privada, utilize `az network dns zone show`. Para obter ajuda, consulte `az network dns zone show --help`.

O exemplo seguinte devolve a zona DNS *contoso. local* e os respetivos dados associados do grupo de recursos *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group MyResourceGroup --name contoso.local
```

O exemplo seguinte é a resposta.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.local",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.local",
  "nameServers": null,
  "numberOfRecordSets": 1,
  "registrationVirtualNetworks": [],
  "resolutionVirtualNetworks": [
    {
      "additionalProperties": {},
      "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyAzureVnet",
      "resourceGroup": "MyResourceGroup"
    }
  ]
  "resourceGroup": "MyResourceGroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones",
  "zoneType": "Private"
}
```

Tenha em atenção que os registos DNS não são devolvidos pelo `az network dns zone show`. Para listar os registos DNS, utilize `az network dns record-set list`.


## <a name="list-dns-zones"></a>Listar zonas DNS

Para enumerar as zonas DNS, utilize `az network dns zone list`. Para obter ajuda, consulte `az network dns zone list --help`.

Especificar o grupo de recursos lista apenas essas zonas no grupo de recursos:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

A omissão do grupo de recursos lista todas as zonas na subscrição:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Atualizar uma zona DNS

É possível efetuar alterações a um recurso de zona DNS com `az network dns zone update`. Para obter ajuda, consulte `az network dns zone update --help`.

Este comando não atualiza qualquer um dos conjuntos de registos de DNS na zona (veja [Como gerir recursos DNS](dns-operations-recordsets-cli.md)). Só é utilizado para atualizar propriedades do recurso da própria zona. Para zonas privadas, pode atualizar as redes virtuais de Registo ou Resolução ligadas a uma zona. 

O exemplo seguinte mostra como atualizar a rede virtual de Resolução ligada a uma zona DNS privada. A rede virtual de Resolução existente e ligada é substituída pela nova rede virtual especificada.

```azurecli
az network dns zone update --resource-group MyResourceGroup --name contoso.local --zone-type Private --resolution-vnets MyNewAzureVnet
```

## <a name="delete-a-dns-zone"></a>Eliminar uma zona DNS

As zonas DNS podem ser eliminadas com `az network dns zone delete`. Para obter ajuda, consulte `az network dns zone delete --help`.

> [!NOTE]
> A eliminação de uma zona DNS também elimina todos os registos DNS na zona. Esta operação não pode ser anulada. Se a zona DNS estiver em utilização, os serviços que utilizam a zona irão falhar quando a zona for eliminada.
>
>Para proteção contra a eliminação acidental de uma zona, veja [Como proteger zonas e registos DNS](dns-protect-zones-recordsets.md).

Este comando solicita a confirmação. O `--yes` comutador opcional suprime esta linha de comandos.

O exemplo seguinte mostra como eliminar a zona *contoso.local* do grupo de recursos *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.local
```

## <a name="delete-all-resources"></a>Eliminar todos os recursos
 
Para eliminar todos os recursos criados neste artigo, realize o seguinte passo:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre DNS do Azure, consulte [Azure DNS overview (Descrição geral do DNS do Azure)](dns-overview.md).

Para saber mais sobre a gestão de zonas DNS no DNS do Azure, consulte [Manage DNS zones in Azure DNS using Azure CLI 2.0 (Gerir zonas DNS no DNS do Azure com a CLI 2.0 do Azure)](dns-operations-dnszones-cli.md).

Para saber mais sobre a gestão de registos DNS no DNS do Azure, consulte [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0 (Gerir registos DNS e conjuntos de registos no DNS do Azure com a CLI 2.0 do Azure)](dns-operations-recordsets-cli.md).
