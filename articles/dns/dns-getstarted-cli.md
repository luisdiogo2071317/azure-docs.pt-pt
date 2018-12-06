---
title: Início Rápido - Criar uma zona DNS do Azure e registar com a CLI do Azure
description: Início Rápido - Saiba como criar uma zona DNS e o registar no DNS do Azure. Este é um guia passo a passo para criar e gerir a sua primeira zona DNS e registar com a CLI do Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: e61975d81fd5920feb5fd47845c67d0aa5293ae6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962016"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-cli"></a>Início Rápido: criar uma zona DNS do Azure e registar com a CLI do Azure

Este artigo explica-lhe os passos para criar a primeira zona DNS e registar com a CLI 1.0 do Azure, que está disponível para Windows, Mac e Linux. Também pode executar estes passos com o [portal do Azure](dns-getstarted-portal.md) ou com o [Azure PowerShell](dns-getstarted-powershell.md).

Uma zona DNS é utilizada para alojar os registos DNS para um determinado domínio. Para começar a alojar o seu domínio no DNS do Azure, tem de criar uma zona DNS para esse nome de domínio. Cada registo DNS para o seu domínio é então criado no interior desta zona DNS. Por fim, para publicar a zona DNS na Internet, tem de configurar os servidores de nomes do domínio. Cada um destes passos está descrito abaixo.

O DNS do Azure também suporta agora zonas DNS privadas (atualmente em pré-visualização pública). Para saber mais sobre zonas DNS privadas, veja [Utilizar o DNS do Azure para domínios privados](private-dns-overview.md). Para obter um exemplo de como criar uma zona DNS privada, veja [Começar a utilizar zonas privadas do DNS do Azure com a CLI](./private-dns-getstarted-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de criar a zona DNS, crie um grupo de recursos para conter a zona DNS:

```azurecli
az group create --name MyResourceGroup --location "East US"
```

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

Uma zona DNS é criada ao utilizar o comando `az network dns zone create`. Para ver a ajuda deste comando, escreva `az network dns zone create -h`.

O exemplo seguinte cria uma zona DNS denominada *contoso.com* no grupo de recursos *MyResourceGroup*. Utilize o exemplo para criar uma zona DNS, substituindo os valores pelos seus.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```

## <a name="create-a-dns-record"></a>Criar um registo DNS

Para criar um registo DNS, utilize o comando `az network dns record-set [record type] add-record`. Para obter ajuda com os registos A, veja `azure network dns record-set A add-record -h`.

O exemplo seguinte cria um registo com o nome relativo “www” na zona DNS “contoso.com”, no grupo de recursos “MyResourceGroup”. O nome totalmente qualificado do conjunto de registos é “www.contoso.com”. O tipo de registo é “A”, com o endereço IP “1.2.3.4” e é utilizado um TTL predefinido de 3600 segundos (1 hora).

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

## <a name="view-records"></a>Ver registos

Para listar os registos DNS na sua zona, execute:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="update-name-servers"></a>Atualizar servidores de nomes

Depois de ter a certeza que a sua zona DNS e os registos foram configurados corretamente, tem de configurar o seu nome de domínio para utilizar os servidores de nomes do DNS do Azure para permitir que os outros utilizadores da Internet encontrem os seus registos de DNS.

Os servidores de nomes para a sua zona são indicados pelo comando `az network dns zone show`. Para ver os nomes dos servidores de nome, utilize a saída do JSON, conforme mostrado no exemplo seguinte.

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Estes servidores de nomes devem ser configurados com a entidade de registo de nomes de domínio (onde comprou o nome de domínio). A entidade de registo irá oferecer a opção para configurar os servidores de nomes do domínio. Para obter mais informações, consulte [Tutorial: alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="delete-all-resources"></a>Eliminar todos os recursos
 
Quando já não forem necessários, pode eliminar todos os recursos criados neste Início Rápido ao eliminar o grupo de recursos:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Agora que criou sua primeira zona DNS e o registo com a CLI do Azure, pode criar registos para uma aplicação Web num domínio personalizado.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
