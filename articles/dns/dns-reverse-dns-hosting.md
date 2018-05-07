---
title: Alojar zonas de pesquisa inversas DNS no DNS do Azure | Microsoft Docs
description: Saiba como utilizar o DNS do Azure para alojar as zonas de pesquisa inversas do DNS para os intervalos de IP
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: kumud
ms.openlocfilehash: c5d53838ffb77ff8d33fee9d10aea4a74f3c523c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="host-reverse-dns-lookup-zones-in-azure-dns"></a>Anfitrião zonas DNS inversas pesquisa no DNS do Azure

Este artigo explica como alojar as zonas de pesquisa inversas do DNS para os intervalos IP atribuídos no DNS do Azure. Os intervalos IP representados pelas zonas de pesquisa inversa devem ser atribuídos à sua organização, normalmente pelo seu ISP.

Para configurar o DNS inverso para um endereço IP pertencente ao Azure que está atribuído ao seu serviço do Azure, consulte o artigo [configurar inversa de DNS para serviços alojados no Azure](dns-reverse-dns-for-azure-services.md).

Antes de ler este artigo, deve estar familiarizado com o [descrição geral do DNS inversa e de suporte no Azure](dns-reverse-dns-overview.md).

Este artigo explica os passos para criar a sua primeira zona DNS de pesquisa inversa e registo utilizando o portal do Azure, Azure PowerShell, CLI do Azure 1.0 ou 2.0 de CLI do Azure.

## <a name="create-a-reverse-lookup-dns-zone"></a>Criar uma zona de pesquisa inversa de DNS

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No **Hub** menu, selecione **novo** > **redes**e, em seguida, selecione **zona DNS**.

   ![Seleção de "Zona DNS"](./media/dns-reverse-dns-hosting/figure1.png)

1. No **zona DNS criar** painel, nome da sua zona DNS. O nome da zona é crafted de forma diferente para os prefixos de IPv4 e IPv6. Utilize as instruções para [IPv4](#ipv4) ou [IPv6](#ipv6) para nomear a sua zona. Quando tiver terminado, selecione **criar** para criar a zona.

### <a name="ipv4"></a>IPv4

Baseia-se no intervalo de IP que representa o nome de uma zona de pesquisa inversa IPv4. Deverá estar no seguinte formato: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Para obter exemplos, consulte [descrição geral do DNS inversa e de suporte no Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Quando estiver a criar classless zonas de pesquisa inversas de DNS no DNS do Azure, tem de utilizar um hífen (`-`) em vez de uma barra (`/`) no nome da zona.
>
> Por exemplo, para 192.0.2.128/26 o intervalo IP, tem de utilizar `128-26.2.0.192.in-addr.arpa` como o nome da zona em vez de `128/26.2.0.192.in-addr.arpa`.
>
> Embora as normas DNS suportarem ambos os métodos, o DNS do Azure não suporta nomes de zona DNS que contêm de barra (`/`) caráter.

O exemplo seguinte mostra como criar uma zona DNS inversa de classe C com o nome `2.0.192.in-addr.arpa` no DNS do Azure através do portal do Azure:

 ![Painel "Criar a zona DNS", com caixas preenchidos](./media/dns-reverse-dns-hosting/figure2.png)

**Localização do grupo de recursos** define a localização para o grupo de recursos. Este não tem impacto sobre a zona DNS. A localização da zona DNS é sempre “global” e não está apresentada.

Os exemplos seguintes mostram como concluir esta tarefa utilizando o Azure PowerShell e da CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

O nome de uma zona de pesquisa inversa IPv6 deve estar no seguinte formato: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Para obter exemplos, consulte [descrição geral do DNS inversa e de suporte no Azure](dns-reverse-dns-overview.md#ipv6).


O exemplo seguinte mostra como criar uma IPv6 inversa zona de pesquisa DNS com o nome `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` no DNS do Azure através do portal do Azure:

 ![Painel "Criar a zona DNS", com caixas preenchidos](./media/dns-reverse-dns-hosting/figure3.png)

**Localização do grupo de recursos** define a localização para o grupo de recursos. Este não tem impacto sobre a zona DNS. A localização da zona DNS é sempre “global” e não está apresentada.

Os exemplos seguintes mostram como concluir esta tarefa utilizando o Azure PowerShell e da CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Delegar a uma zona de pesquisa inversa DNS

Agora que criou a zona de pesquisa inversa DNS, certifique-se de que a zona é delegada da zona principal. Delegação de DNS permite que o processo de resolução de nome DNS para localizar os servidores de nomes que alojam a zona de pesquisa inversa DNS. Esses servidores de nomes, em seguida, podem responder às consultas inversas de DNS para os endereços IP no intervalo de endereços.

Para zonas de pesquisa direta, o processo de delegação de uma zona DNS é descrito no [delegar o domínio ao DNS do Azure](dns-delegate-domain-azure-dns.md). A delegação de zonas de pesquisa inversa funciona da mesma forma. A única diferença é que tem de configurar os servidores de nomes com o ISP que forneceu o intervalo de IP, em vez da sua entidade de registo de nome de domínio.

## <a name="create-a-dns-ptr-record"></a>Criar um registo DNS PTR

### <a name="ipv4"></a>IPv4

O exemplo seguinte explica o processo de criação de um registo PTR numa zona DNS inversa no DNS do Azure. Para outros tipos de registo e para modificar registos existentes, consulte [Manage DNS records and record sets by using the Azure portal (Gerir registos DNS e conjuntos de registos com o portal do Azure)](dns-operations-recordsets-portal.md).

1. Na parte superior a **zona DNS** painel, selecione **+ conjunto de registos** para abrir o **adicionar conjunto de registos** painel.

   ![Botão para criar um conjunto de registos](./media/dns-reverse-dns-hosting/figure4.png)

1. O nome do registo definido para um registo PTR tem de ser o resto do endereço de IPv4 na ordem inversa. 

   Neste exemplo, os três primeiros octetos já estão povoados como parte do nome de zona (.2.0.192). Por conseguinte, apenas o último octeto é fornecido no **nome** caixa. Por exemplo, pode dar o nome do conjunto de registos **15** para um recurso cujo endereço IP é 192.0.2.15.  
1. Para **tipo**, selecione **PTR**.  
1. Para **nome de domínio**, introduza o nome de domínio completamente qualificado (FQDN) do recurso que utiliza o IP.
1. Selecione **OK** na parte inferior do painel para criar o DNS de registo.

 ![Painel "Adicionar o conjunto de registos", com caixas preenchidos](./media/dns-reverse-dns-hosting/figure5.png)

Os exemplos seguintes mostram como concluir esta tarefa utilizando o PowerShell ou a CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

O exemplo seguinte explica o processo de criação do novo registo PTR. Para outros tipos de registo e para modificar registos existentes, consulte [Manage DNS records and record sets by using the Azure portal (Gerir registos DNS e conjuntos de registos com o portal do Azure)](dns-operations-recordsets-portal.md).

1. Na parte superior a **zona DNS** painel, selecione **+ conjunto de registos** para abrir o **adicionar conjunto de registos** painel.

   ![Botão para criar um conjunto de registos](./media/dns-reverse-dns-hosting/figure6.png)

2. O nome do registo definido para um registo PTR tem de ser o resto do endereço IPv6 na ordem inversa. Não tem de incluir quaisquer zero compressão. 

   Neste exemplo, os primeiro 64 bits de IPv6 já estão povoados como parte do nome de zona (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Por conseguinte, apenas os últimos 64 bits são fornecidos no **nome** caixa. Os últimos 64 bits do endereço IP são introduzidos na ordem inversa, com um período como delimitador entre cada número hexadecimal. Por exemplo, pode dar o nome do conjunto de registos **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f** para um recurso cujo endereço IP é 2001:0db8:abdc:0000:f524:10bc:1af9:405e.  
3. Para **tipo**, selecione **PTR**.  
4. Para **nome de domínio**, introduza o FQDN do recurso que utiliza o IP.
5. Selecione **OK** na parte inferior do painel para criar o DNS de registo.

![Painel "Adicionar o conjunto de registos", com caixas preenchidos](./media/dns-reverse-dns-hosting/figure7.png)

Os exemplos seguintes mostram como concluir esta tarefa utilizando o PowerShell ou a CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Ver registos

Para ver os registos que criou, navegue até à sua zona DNS no portal do Azure. Na parte inferior do **zona DNS** painel, pode ver os registos para a zona DNS. Deverá ver a predefinição NS e registos SOA, para além de quaisquer novos registos que criou. Os registos NS e SOA são criados em cada zona. 

### <a name="ipv4"></a>IPv4

O **zona DNS** painel mostra os registos IPv4 PTR:

![Painel "Zona DNS" com os registos de IPv4](./media/dns-reverse-dns-hosting/figure8.png)

Os exemplos seguintes mostram como ver os registos PTR utilizando o PowerShell ou a CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

O **zona DNS** painel mostra os registos IPv6 PTR:

![Painel "Zona DNS" com os registos de IPv6](./media/dns-reverse-dns-hosting/figure9.png)

Os exemplos seguintes mostram como visualizar registos utilizando o PowerShell ou a CLI do Azure.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>CLI do Azure 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>FAQ

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Pode alojar zonas de pesquisa inversas DNS para os meus blocos IP ISP atribuído no DNS do Azure?

Sim. Que aloja as zonas de pesquisa inversa (ARPA) para os seus próprios intervalos IP no DNS do Azure é totalmente suportada.

Crie a zona de pesquisa inversa DNS do Azure conforme explicado neste artigo e, em seguida, trabalhe com o seu ISP para [delegar a zona](dns-domain-delegation.md). Em seguida, pode gerir os registos PTR para cada pesquisa inversa da mesma forma que outros tipos de registo.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>É quanto o meu custo de zona de pesquisa DNS inverso de alojamento?

Que aloja a zona de pesquisa inversa DNS para o bloco IP ISP atribuído no DNS do Azure é cobrado [a taxas padrão do DNS do Azure](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Pode alojar zonas de pesquisa DNS inversas para endereços IPv4 e IPv6 no DNS do Azure?

Sim. Este artigo explica como criar IPv4 e IPv6 zonas de pesquisa DNS inversas no DNS do Azure.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Pode importar uma zona de pesquisa DNS inversa existente?

Sim. Pode utilizar a CLI do Azure para importar existentes zonas DNS para o DNS do Azure. Este método funciona para as zonas de pesquisa direta e zonas de pesquisa inversa.

Para obter mais informações, consulte [importar e exportar um ficheiro de zona DNS ao utilizar a CLI do Azure](dns-import-export.md).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre inversa DNS, consulte [pesquisa inversa de DNS em Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Saiba como [gerir registos inversas de DNS para os serviços do Azure](dns-reverse-dns-for-azure-services.md).
