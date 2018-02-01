---
title: "Delegar o seu domínio ao DNS do Azure | Microsoft Docs"
description: "Compreenda como alterar a delegação de domínios e utilizar servidores de nomes de DNS do Azure para fornecer o alojamento de domínios."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: a13d9b360e2c43aa2a3d4f62215e4570ce42cd5b
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="delegate-a-domain-to-azure-dns"></a>Delegar um domínio ao DNS do Azure

Pode utilizar o DNS do Azure para alojar uma zona DNS e gerir os registos DNS para um domínio no Azure. Para que as consultas do DNS de um domínio alcancem o DNS do Azure, o domínio tem de ser delegado ao DNS do Azure a partir do domínio principal. Tenha em atenção que o DNS do Azure não é a entidade de registo de domínios. Este artigo explica como delegar o seu domínio ao DNS do Azure.

Relativamente aos domínios que comprar junto de uma entidade de registo, a entidade de registo irá oferecer a opção para configurar estes registos NS. Não tem de ter um nome de domínio para criar uma zona DNS com esse nome de domínio no DNS do Azure. No entanto, é necessário possuir o domínio para configurar a delegação ao DNS do Azure com a entidade de registo.

Por exemplo, suponha que compra o domínio contoso.net e cria uma zona com o nome contoso.net no DNS do Azure. Na qualidade de proprietário do domínio, a sua entidade de registo oferece-lhe a opção de configurar os endereços do servidor de nomes (ou seja, os registos NS) do seu domínio. A entidade de registo armazena estes registos NS no domínio principal, .net. Os clientes em todo o mundo podem, então, ser direcionados para o seu domínio na zona DNS do Azure quando tentam resolver os registos DNS em contoso.net.

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

1. Inicie sessão no Portal do Azure.
1. No menu **Hub**, selecione **Novo** > **Redes** > **Zona DNS** para abrir a página  **Criar zona DNS**.

   ![Zona DNS](./media/dns-domain-delegation/dns.png)

1. Na página **Criar zona DNS**, introduza os valores seguintes e, em seguida, selecione **Criar**:

   | **Definição** | **Valor** | **Detalhes** |
   |---|---|---|
   |**Nome**|contoso.net|Forneça o nome da zona DNS.|
   |**Subscrição**|[A sua subscrição]|Selecione uma subscrição para criar o gateway de aplicação.|
   |**Grupo de recursos**|**Criar novo:** contosoRG|Crie um grupo de recursos. O nome do grupo de recursos tem de ser exclusivo dentro da subscrição que selecionou. Para saber mais sobre grupos de recursos, veja o artigo de descrição geral do [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Localização**|EUA Oeste||

> [!NOTE]
> A localização do grupo de recursos não tem qualquer impacto na zona DNS. A localização da zona DNS é sempre “global” e não está apresentada.

## <a name="retrieve-name-servers"></a>Obter servidores de nomes

Antes de pode delegar a zona DNS ao DNS do Azure, terá de conhecer os servidores de nomes da sua zona. O DNS do Azure aloca servidores de nomes a partir de um conjunto sempre que é criada uma zona.

1. Com a zona DNS criada, no painel **Favoritos** do portal do Azure, selecione **Todos os recursos**. Na página **Todos os recursos**, selecione a zona de DNS **contoso.net**. Se a subscrição que selecionou já tiver vários recursos, pode introduzir **contoso.net** na caixa **Filtrar por nome** para aceder facilmente ao gateway da aplicação. 

1. Obtenha os servidores de nome na página da zona DNS. Neste exemplo, foram atribuídos à zona contoso.net os servidores de nomes ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org e ns4-01.azure-dns.info:

   ![Lista dos servidores de nomes](./media/dns-domain-delegation/viewzonens500.png)

O DNS do Azure cria automaticamente registos NS autoritativos na sua zona para conter os servidores de nomes atribuídos. Para ver os servidores de nomes através do Azure PowerShell ou da CLI do Azure, obtenha estes registos.

Os exemplos seguintes mostram os passos para obter os servidores de nomes de uma zona no DNS do Azure com o PowerShell e a CLI do Azure.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

O exemplo seguinte é a resposta:

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>CLI do Azure

```azurecli
az network dns record-set list --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

O exemplo seguinte é a resposta:

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Delegar o domínio

Agora que a zona DNS está criada e que já tem os servidores de nomes, tem de atualizar o domínio principal com os servidores de nomes DNS do Azure. Cada entidade de registo tem as suas próprias ferramentas de gestão de DNS para alterar os registos do servidor de nomes de um domínio. Na página de gestão do DNS da entidade de registo, edite os registos NS e substitua-os por aqueles que o DNS do Azure criou.

Quando estiver a delegar um domínio ao DNS do Azure, tem de utilizar os servidores de nomes fornecidos por esse DNS do Azure. Recomendamos que utilize os quatro servidores de nomes, independentemente do seu domínio. A delegação de domínio não necessita que um servidor de nomes utilize o mesmo domínio de nível superior que o seu domínio.

Não utilize *registos “glue”* para que apontem para os endereços IP do servidor de nomes do DNS do Azure, uma vez que estes endereços IP podem ser alterados no futuro. As delegações que utilizam servidores de nomes de nome na sua própria zona, por vezes denominados *servidores de nomes personalizados*, não são atualmente suportadas no DNS do Azure.

## <a name="verify-that-name-resolution-is-working"></a>Confirmar que a resolução de nomes está a funcionar

Depois de concluir a delegação, pode confirmar que a resolução de nomes está a funcionar ao utilizar uma ferramenta como a nslookup para consultar o registo SOA da sua zona. (O registo SOA é criado automaticamente quando a zona é criada.)

Não é necessário especificar os servidores de nomes DNS do Azure. Se a delegação estiver configurada corretamente, o processo de resolução DNS normal localiza os servidores de nomes automaticamente.

```
nslookup -type=SOA contoso.com
```

Segue-se uma resposta de exemplo do comando anterior:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-subdomains-in-azure-dns"></a>Delegar subdomínios no DNS do Azure

Se pretende configurar uma zona subordinada em separado, pode delegar um subdomínio no DNS do Azure. Por exemplo, suponha que configurou e delegou contoso.net no DNS do Azure. Agora pretende configurar uma zona subordinada em separado, partners.contoso.net.

1. Crie a zona subordinada partners.contoso.net no DNS do Azure.
2. Procure os registos NS autoritativos na zona subordinada para obter os servidores de nomes que alojam a zona subordinada no DNS do Azure.
3. Delegue a zona subordinada ao configurar os registos NS na zona principal que aponta para a zona subordinada.

### <a name="create-a-dns-zone"></a>Criar uma zona DNS

1. Inicie sessão no Portal do Azure.
1. No menu **Hub**, selecione **Novo** > **Redes** > **Zona DNS** para abrir a página  **Criar zona DNS**.

   ![Zona DNS](./media/dns-domain-delegation/dns.png)

1. Na página **Criar zona DNS**, introduza os valores seguintes e, em seguida, selecione **Criar**:

   | **Definição** | **Valor** | **Detalhes** |
   |---|---|---|
   |**Nome**|partners.contoso.net|Forneça o nome da zona DNS.|
   |**Subscrição**|[A sua subscrição]|Selecione uma subscrição para criar o gateway de aplicação.|
   |**Grupo de recursos**|**Utilizar existente:** contosoRG|Crie um grupo de recursos. O nome do grupo de recursos tem de ser exclusivo dentro da subscrição que selecionou. Para saber mais sobre grupos de recursos, veja o artigo de descrição geral do [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Localização**|EUA Oeste||

> [!NOTE]
> A localização do grupo de recursos não tem qualquer impacto na zona DNS. A localização da zona DNS é sempre “global” e não está apresentada.

### <a name="retrieve-name-servers"></a>Obter servidores de nomes

1. Com a zona DNS criada, no painel **Favoritos** do portal do Azure, selecione **Todos os recursos**. Selecione a zona DNS **partners.contoso.net**, na página **Todos os recursos**. Se a subscrição que selecionou já tiver vários recursos, pode introduzir **partners.contoso.net** na caixa **Filtrar por nome** para aceder facilmente à zona DNS.

1. Obtenha os servidores de nome na página da zona DNS. Neste exemplo, foram atribuídos à zona contoso.net os servidores de nomes ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org e ns4-01.azure-dns.info:

   ![Lista dos servidores de nomes](./media/dns-domain-delegation/viewzonens500.png)

O DNS do Azure cria automaticamente registos NS autoritativos na sua zona para conter os servidores de nomes atribuídos.  Para ver os servidores de nomes através do Azure PowerShell ou da CLI do Azure, obtenha estes registos.

### <a name="create-a-name-server-record-in-the-parent-zone"></a>Criar um registo do servidor de nomes na zona principal

1. No portal do Azure, navegue para a zona DNS **contoso.net**.
1. Selecione **+ Conjunto de registos**.
1. Na página **Adicionar conjunto de registos**, introduza os valores seguintes e selecione **OK**:

   | **Definição** | **Valor** | **Detalhes** |
   |---|---|---|
   |**Nome**|partners|Introduza o nome da zona DNS subordinada.|
   |**Tipo**|NS|Utilize NS para registos do servidor de nome.|
   |**TTL**|1|Introduza o tempo de duração.|
   |**Unidade de TTL**|Horas|Defina a unidade de tempo de duração como horas.|
   |**SERVIDOR DE NOME**|{servidores de nome da zona partners.contoso.net}|Introduza os quatro servidores de nomes da zona partners.contoso.net. |

   ![Valores do registo do servidor de nomes](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegate-subdomains-in-azure-dns-by-using-other-tools"></a>Delegar subdomínios no DNS do Azure com outras ferramentas

Os exemplos seguintes mostram os passos para delegar subdomínios no DNS do Azure com o PowerShell e a CLI do Azure.

#### <a name="powershell"></a>PowerShell

O exemplo do PowerShell que se segue demonstra como isto funciona. Pode seguir os mesmos passos através do portal do Azure ou da CLI do Azure multiplataformas.

```powershell
# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This information contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name (in this case, "partners").
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Utilize `nslookup` para procurar o registo SOA da zona subordinada e confirmar que tudo está configurado corretamente.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>CLI do Azure

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Obtenha os servidores de nome da zona `partners.contoso.net` a partir do resultado.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Crie o conjunto de registos e os registos NS para cada servidor de nome.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create an NS record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Eliminar todos os recursos

Para eliminar todos os recursos criados neste artigo, conclua os seguintes passos:

1. No painel **Favoritos** do portal do Azure, selecione **Todos os recursos**. Na página **Todos os recursos**, selecione o grupo de recursos **contosorg**. Se a subscrição que selecionou já tiver vários recursos, pode introduzir **contosorg** na caixa **Filtrar por nome** para aceder facilmente ao grupo de recursos.
1. Na página **contosorg**, selecione o botão **Eliminar**.
1. O portal requer que escreva o nome do grupo de recursos para confirmar que pretende eliminá-lo. Escreva **contosorg** no nome do grupo de recursos e selecione **Eliminar**. 

A eliminação de um grupo de recursos dita a eliminação de todos os recursos no grupo de recursos. Por conseguinte, confirme os conteúdos de um grupo de recursos antes de o eliminar. O portal elimina todos os recursos no grupo de recursos e, em seguida, elimina o próprio grupo de recursos. Este processo demora vários minutos.

## <a name="next-steps"></a>Passos seguintes

[Gerir zonas DNS](dns-operations-dnszones.md)

[Gerir registos DNS](dns-operations-recordsets.md)
