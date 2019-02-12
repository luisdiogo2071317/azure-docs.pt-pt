---
title: Início rápido – Criar uma zona DNS do Azure e registar com o Azure PowerShell
description: Saiba como criar uma zona DNS e o registar no DNS do Azure. Este é um guia passo a passo para criar e gerir a sua primeira zona DNS e registar com o Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: 839c97ccccbc1ce2cf646afcd27894a190eda1b0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000898"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Início rápido: Criar uma zona DNS do Azure e o registo com o Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Neste início rápido, cria a sua primeira zona DNS e regista-a com o Azure PowerShell. Também pode executar estes passos com o [portal do Azure](dns-getstarted-portal.md) ou o [CLI do Azure](dns-getstarted-cli.md). 

Uma zona DNS é utilizada para alojar os registos DNS para um determinado domínio. Para começar a alojar o seu domínio no DNS do Azure, tem de criar uma zona DNS para esse nome de domínio. Cada registo DNS para o seu domínio é então criado no interior desta zona DNS. Por fim, para publicar a zona DNS na Internet, tem de configurar os servidores de nomes do domínio. Cada um destes passos está descrito abaixo.

O DNS do Azure também suporta a criação de domínios privados. Para obter instruções passo-a-passo sobre como criar a primeira zona DNS privada e o registo, veja [Introdução às zonas privadas do DNS do Azure com o PowerShell](private-dns-getstarted-powershell.md).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de criar a zona DNS, crie um grupo de recursos para conter a zona DNS:

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

Uma zona DNS é criada ao utilizar o cmdlet `New-AzDnsZone`. O exemplo seguinte cria uma zona DNS denominada *contoso.com* no grupo de recursos denominado *MyResourceGroup*. Utilize o exemplo para criar uma zona DNS, substituindo os valores pelos seus.

```powershell
New-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Criar um registo DNS

Pode criar conjuntos de registos com o cmdlet `New-AzDnsRecordSet`. O exemplo seguinte cria um registo com o nome relativo "www" na zona DNS "contoso.com", no grupo de recursos "MyResourceGroup". O nome completamente qualificado do conjunto de registos é “www.contoso.com”. O tipo de registo é "A", com o endereço IP "1.2.3.4" e o TTL é de 3600 segundos.

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4")
```

## <a name="view-records"></a>Ver registos

Para listar os registos DNS na sua zona, utilize:

```powershell
Get-AzDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="update-name-servers"></a>Atualizar servidores de nomes

Depois de ter a certeza que a sua zona DNS e os registos foram configurados corretamente, tem de configurar o seu nome de domínio para utilizar os servidores de nomes do DNS do Azure. Isto permite que outros utilizadores na Internet encontrem os seus registos DNS.

Os servidores de nomes para a sua zona são indicados pelo cmdlet `Get-AzDnsZone`:

```powershell
Get-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Estes servidores de nomes devem ser configurados com a entidade de registo de nomes de domínio (onde comprou o nome de domínio). A entidade de registo irá oferecer a opção para configurar os servidores de nomes do domínio. Para obter mais informações, consulte [Tutorial: Aloje o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md#delegate-the-domain).

## <a name="delete-all-resources"></a>Eliminar todos os recursos

Quando já não forem necessários, pode eliminar todos os recursos criados neste Início Rápido ao eliminar o grupo de recursos:

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Agora que criou sua primeira zona DNS e o registo com o Azure PowerShell, pode criar registos para uma aplicação Web num domínio personalizado.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)

