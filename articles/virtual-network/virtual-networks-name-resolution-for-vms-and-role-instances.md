---
title: Resolução de nomes para máquinas virtuais e instâncias de função
description: Cenários de resolução de nome para o IaaS do Azure, as soluções híbridas, entre diferentes serviços em nuvem, do Active Directory e utilizando o seu próprio servidor DNS.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: e46f6617b1a6d73ace00d4eafa1410785315a8c8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="name-resolution-for-virtual-machines-and-role-instances"></a>Resolução de nomes para máquinas virtuais e instâncias de função

Dependendo de como utilizar o Azure para alojar o IaaS, PaaS e soluções híbridas, poderá precisar de permitir que as máquinas virtuais (VMs) e instâncias de função que criar para comunicar entre si. Embora pode ativar esta comunicação através da utilização de endereços IP, é muito mais simples utilizar nomes que podem ser memorizados facilmente e não alteram. 

Quando precisam de resolver os nomes de domínio para endereços IP internos instâncias de função e as VMs alojadas no Azure, podem utilizar um dos dois métodos:

* [Resolução de nome fornecidos pelo Azure](#azure-provided-name-resolution)
* [Resolução que utiliza o seu próprio servidor DNS de nomes](#name-resolution-that-uses-your-own-dns-server) (que pode reencaminhar consultas para os servidores DNS fornecidos pelo Azure) 

O tipo de resolução de nomes que utiliza depende de como as VMs e instâncias de função têm de comunicar entre si. A tabela seguinte ilustra os cenários e soluções de resolução de nome correspondente:

> [!NOTE]
> Dependendo do seu cenário, poderá utilizar a funcionalidade de zonas de DNS privado do Azure, que está atualmente em pré-visualização pública. Para obter mais detalhes, consulte [utilizando o DNS do Azure para domínios privados](../dns/private-dns-overview.md)
>

| **Cenário** | **Solução** | **Suffix** |
| --- | --- | --- |
| Resolução de nome entre instâncias de função ou VMs localizadas no mesmo serviço em nuvem ou de rede virtual. | [Zonas de DNS privado do Azure](../dns/private-dns-overview.md) ou [resolução de nome fornecidos pelo Azure](#azure-provided-name-resolution) |nome de anfitrião ou o FQDN |
| Resolução de nome entre instâncias de função ou VMs localizadas em redes virtuais diferentes. |[Zonas de DNS privado do Azure](../dns/private-dns-overview.md) ou, servidores DNS geridos pelo cliente reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Apenas FQDN |
| Resolução de nome de um serviço de aplicações do Azure (aplicação Web, a função ou Bot) utilizando a integração de rede virtual para instâncias de função ou VMs localizado na mesma rede virtual. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Apenas FQDN |
| Resolução do nome de Web Apps do App Service para VMs localizado na mesma rede virtual. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Apenas FQDN |
| Resolução do nome de Web Apps do App Service para VMs localizados numa rede virtual diferente. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server-for-web-apps). |Apenas FQDN |
| Resolução de nomes de computador e o serviço no local de instâncias de função ou VMs no Azure. |Gerida pelo cliente servidores DNS (controlador de domínio no local, controlador de domínio só de leitura local ou um DNS secundário sincronizada com êxito utilizando transferências de zona, por exemplo). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Apenas FQDN |
| Resolução de nomes de anfitrião do Azure a partir de computadores no local. |Consultas de reencaminhar para um servidor de proxy gerida pelo cliente DNS na rede virtual correspondente, o servidor de proxy reencaminha consultas para o Azure para a resolução. Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Apenas FQDN |
| Inverte o DNS para IPs interno. |[Resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-that-uses-your-own-dns-server). |Não aplicável |
| Resolução de nome entre VMs ou instâncias de função localizadas nos serviços de nuvem diferente, não numa rede virtual. |Não aplicável. A conectividade entre VMs e instâncias de função nos serviços de nuvem diferente não é suportada fora de uma rede virtual. |Não aplicável|

## <a name="azure-provided-name-resolution"></a>Resolução de nome fornecidos pelo Azure

Juntamente com a resolução de nomes DNS públicos, o Azure oferece resolução dos nomes internos para VMs e instâncias de função que residem dentro da mesma rede virtual ou serviço em nuvem. VMs e instâncias de um serviço em nuvem partilham o mesmo sufixo DNS (para que o nome de anfitrião autónomo é suficiente). Mas em redes virtuais que utilizam o modelo de implementação clássica, os serviços de nuvem diferente ter diferentes sufixos DNS. Nesta situação, terá do FQDN para resolver nomes entre os serviços de nuvem diferente. Nas redes virtuais no modelo de implementação Azure Resource Manager, o sufixo DNS é consistente entre a rede virtual (para que o FQDN não é necessário). Nomes DNS podem ser atribuídos a NICs e VMs. Apesar de resolução de nome fornecidos pelo Azure necessita de qualquer configuração, não é a opção adequada para todos os cenários de implementação, como mostrado na tabela anterior.

> [!NOTE]
> Quando utilizar funções web e de trabalho, também pode aceder os endereços IP internos de instâncias de função. Isto baseia-se o número de nome e a instância de função, utilizando a API de REST de gestão de serviço do Azure. Para obter mais informações, consulte o [referência de API de REST de gestão de serviço](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Funcionalidades

Resolução de nome fornecidos pelo Azure inclui as seguintes funcionalidades:
* Facilidade de utilização. É necessária nenhuma configuração.
* Elevada disponibilidade. Não precisa de criar e gerir clusters dos seus servidores DNS.
* Pode utilizar o serviço em conjunto com os seus próprios servidores DNS, para resolver os nomes de anfitrião do Azure e no local.
* Pode utilizar a resolução de nome entre instâncias de função e as VMs no mesmo serviço de nuvem, sem a necessidade de um FQDN.
* Pode utilizar a resolução de nome entre VMs em redes virtuais que utilizam o modelo de implementação Azure Resource Manager, sem necessidade de um FQDN. Quando a resolução de nomes nos serviços de nuvem diferente as redes virtuais no modelo de implementação clássica requerem um FQDN. 
* Pode utilizar nomes de anfitriões que melhor descrevem as implementações, em vez de trabalhar com nomes gerado automaticamente.

### <a name="considerations"></a>Considerações

Seguem-se pontos a considerar quando estiver a utilizar a resolução de nome fornecidos pelo Azure:
* Não é possível modificar o sufixo DNS criados no Azure.
* Não é possível registar manualmente os seus próprios registos.
* Não são suportados o WINS e NetBIOS (não é possível ver as suas VMs no Explorador do Windows).
* Os nomes de anfitrião tem de ser compatível com o DNS. Nomes têm de utilizar apenas 0-9, a-z, e '-' e não pode começar nem terminar com um '-'.
* O tráfego de consulta DNS está limitado para cada VM. Limitação não deve afetar a maioria das aplicações. Se a limitação do pedido é observado, certifique-se de que a colocação em cache do lado do cliente está ativada. Para obter mais informações, consulte [configuração de cliente DNS](#dns-client-configuration).
* Apenas as VMs nos serviços de 180 nuvem primeiro estão registadas para cada rede virtual de um modelo de implementação clássica. Este limite não é aplicável a redes virtuais no Gestor de recursos do Azure.

## <a name="dns-client-configuration"></a>Configuração de cliente DNS

Esta secção abrange a colocação em cache do lado do cliente e de tentativas do lado do cliente.

### <a name="client-side-caching"></a>Colocação em cache do lado do cliente

Nem todas as consultas DNS tem de ser enviadas através da rede. Colocação em cache do lado do cliente ajuda a reduzir a latência e melhorar a maior resiliência às blips de rede, ao resolver as consultas DNS periódicas a partir de uma cache local. Registos DNS contêm um mecanismo de (TTL) time-to-live, o que permite que a cache armazenar o registo para desde possíveis sem afetar a actualização de registo. Assim, a colocação em cache do lado do cliente é adequada para a maioria das situações.

A predefinição do cliente DNS do Windows tem uma cache DNS incorporada. Algumas distribuições de Linux não incluem a colocação em cache por predefinição. Se achar que não é uma cache local já, adicione uma cache DNS para cada VM com Linux.

Há uma série de DNS diferentes pacotes disponíveis (por exemplo, dnsmasq) a colocação em cache. Eis como instalar dnsmasq nas distribuições mais comuns:

* **Ubuntu (utiliza resolvconf)**:
  * Instalar o pacote de dnsmasq com `sudo apt-get install dnsmasq`.
* **SUSE (utiliza netconf)**:
  * Instalar o pacote de dnsmasq com `sudo zypper install dnsmasq`.
  * Ativar o serviço de dnsmasq com `systemctl enable dnsmasq.service`. 
  * Iniciar o serviço de dnsmasq com `systemctl start dnsmasq.service`. 
  * Editar **/etc/sysconfig/network/config**e alterar *NETCONFIG_DNS_FORWARDER = ""* para *dnsmasq*.
  * Atualizar resolv.conf com `netconfig update`, para definir a cache, como a resolução DNS local.
* **OpenLogic (utiliza NetworkManager)**:
  * Instalar o pacote de dnsmasq com `sudo yum install dnsmasq`.
  * Ativar o serviço de dnsmasq com `systemctl enable dnsmasq.service`.
  * Iniciar o serviço de dnsmasq com `systemctl start dnsmasq.service`.
  * Adicionar *preceder nome-servidores domínio 127.0.0.1;* para **/etc/dhclient-eth0.conf**.
  * Reinicie o serviço de rede com `service network restart`, para definir a cache, como a resolução DNS local.

> [!NOTE]
> O pacote de dnsmasq é apenas um dos muitos caches DNS disponíveis para o Linux. Antes de o utilizar, verifique a respetiva adequação para as suas necessidades particulares e verifique se nenhuma outra cache está instalado.
> 
> 
    
### <a name="client-side-retries"></a>Tentativas de lado do cliente

O DNS é principalmente um protocolo UDP. Porque o protocolo UDP não garante a entrega de mensagens, a lógica de repetição é processada no próprio protocolo DNS. Cada cliente DNS (sistema operativo) pode apresentem um lógica de repetição diferentes, dependendo da preferência o criador:

* Sistemas operativos Windows novamente depois de um segundo e, em seguida, novamente após outro duas segundos, quatro segundos e segundos, quatro outro. 
* As repetições a configuração do Linux predefinida que se após cinco segundos. Recomendamos que alterar as especificações de repetição para cinco vezes, em intervalos de um segundo.

Verifique as definições atuais numa VM com Linux com `cat /etc/resolv.conf`. Observe o *opções* linha, por exemplo:

```bash
options timeout:1 attempts:5
```

O ficheiro resolv.conf é, normalmente, gerado automaticamente e não deve ser editado. Os passos específicos para adicionar o *opções* linha variam consoante a distribuição:

* **Ubuntu** (utiliza resolvconf):
  1. Adicionar o *opções* linha para **/etc/resolveconf/resolv.conf.d/head**.
  2. Executar `resolvconf -u` para atualizar.
* **SUSE** (utiliza netconf):
  1. Adicionar *timeout:1 tentativas: 5* para o **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parâmetro **/etc/sysconfig/network/config**. 
  2. Executar `netconfig update` para atualizar.
* **OpenLogic** (utiliza NetworkManager):
  1. Adicionar *eco "timeout:1 tentativas: 5 opções"* para **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  2. Atualizar com `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Resolução de nomes que utiliza o seu próprio servidor DNS

Esta secção abrange as VMs e instâncias de função e as aplicações web.

### <a name="vms-and-role-instances"></a>VMs e instâncias de função

Necessidades de resolução de nome poderão ir para além das funcionalidades fornecidas pelo Azure. Por exemplo, pode ser o caso quando estiver a utilizar domínios do Active Directory ou quando necessitar de resolução de DNS entre redes virtuais. Para estes cenários, o Azure oferece a capacidade para que possa utilizar os seus próprios servidores DNS.

Servidores DNS dentro de uma rede virtual podem reencaminhar consultas DNS para as resoluções de recursiva no Azure. Isto permite-lhe resolver nomes de anfitrião dentro da rede virtual. Por exemplo, um controlador de domínio (DC) em execução no Azure pode responder às consultas DNS para os domínios e reencaminhar todas as outras consultas para o Azure. Consultas de reencaminhamento, permite que as VMs ver os seus recursos no local (através de DC) e os nomes de anfitrião fornecidos pelo Azure (através do reencaminhador). Acesso para as resoluções de recursiva no Azure é fornecido através de virtual IP 168.63.129.16.

Reencaminhamento de DNS também permite a resolução de DNS entre redes virtuais e permite que as máquinas no local resolver nomes de anfitrião fornecidos pelo Azure. Para resolver o nome do anfitrião da VM, o servidor DNS VM tem de residir na mesma rede virtual e a ser configurada para consultas de nome de anfitrião de reencaminhar para o Azure. Porque o sufixo DNS é diferente em cada rede virtual, pode utilizar regras de reencaminhamento condicional para enviar consultas DNS para a rede virtual correta para a resolução. A imagem seguinte mostra duas redes virtuais e uma rede no local ao executar a resolução DNS entre redes virtuais, ao utilizar este método. Um reencaminhador DNS de exemplo está disponível no [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) e [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Uma instância de função pode efetuar a resolução de nome de VMs dentro da mesma rede virtual. Isto é feito utilizando o FQDN, que consiste em nome do anfitrião da VM e **internal.cloudapp.net** sufixo DNS. No entanto, neste caso, resolução de nomes só for bem sucedida se a instância de função tem o nome VM definido no [esquema de função (ficheiro. cscfg)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Instâncias de função que tem de efetuar a resolução de nome de VMs na outra rede virtual (FQDN, utilizando o **internal.cloudapp.net** sufixo) tem de fazê-lo utilizando o método descrito nesta secção (servidores DNS personalizados reencaminhamento entre as duas redes virtuais).
>

![Diagrama de DNS entre redes virtuais](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Quando estiver a utilizar a resolução de nome fornecidos pelo Azure, Azure configuração protocolo DHCP (Dynamic Host) fornece um sufixo DNS interno (**. internal.cloudapp.net**) para cada VM. Este sufixo permite que a resolução de nome de anfitrião porque os registos de nome de anfitrião estão no **internal.cloudapp.net** zona. Quando estiver a utilizar o sua própria solução de resolução de nome, este sufixo não é fornecido para VMs porque-interfere com outras arquiteturas DNS (por exemplo, cenários de associados a um domínio). Em vez disso, o Azure oferece um marcador de posição não está em funcionamento (*reddog.microsoft.com*).

Se necessário, pode determinar o sufixo DNS interno utilizando o PowerShell ou a API:

* Para as redes virtuais em modelos de implementação Azure Resource Manager, o sufixo está disponível através de [REST API de interface de rede](/rest/api/virtualnetwork/networkinterfaces/get), a [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) cmdlet do PowerShell e o [mostrar de nic de rede az](/cli/azure/network/nic#az-network-nic-show) comando da CLI do Azure.
* Modelos de implementação clássica, o sufixo está disponível através de [obter API de implementação](https://msdn.microsoft.com/library/azure/ee460804.aspx) chamar ou o [Get-AzureVM-depurar](/powershell/module/azure/get-azurevm) cmdlet.

Se as consultas de reencaminhamento para o Azure não conforme as suas necessidades, deve fornecer as suas próprias soluções DNS. A solução DNS tem de:

* Forneça um anfitrião adequado resolução do nome, através de [DDNS](virtual-networks-name-resolution-ddns.md), por exemplo. Tenha em atenção que se estiver a utilizar DDNS, poderá ter de desativar a eliminação de registos DNS. Concessões DHCP do Azure são longos e eliminação poderá remover registos DNS prematuramente. 
* Resolução recursiva adequadas para permitir a resolução de nomes de domínio externo.
* Estar acessível (TCP e UDP na porta 53) dos clientes que tem e de conseguir aceder à internet.
* Estar protegida contra acesso a partir da internet, a mitigar ameaças colocadas por agentes externos.

> [!NOTE]
> Para melhor desempenho, quando estiver a utilizar as VMs do Azure como servidores DNS IPv6 deve ser desativado. Um [IP público de nível de instância](virtual-networks-instance-level-public-ip.md) deve ser atribuído a cada VM de servidor DNS. Para análise de desempenho adicionais e otimizações quando estiver a utilizar o Windows Server como servidor DNS, consulte [nome desempenho de resolução de um recursivo de DNS de Windows Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web Apps
Suponha que tem de efetuar a resolução do nome da sua aplicação web criada utilizando o serviço de aplicações, ligada a uma rede virtual, para as VMs na mesma rede virtual. Para além de configurar um DNS personalizado o servidor que tenha um reencaminhador DNS reencaminha consultas para o Azure (virtual IP 168.63.129.16), execute os seguintes passos:
1. Ativar a integração de rede virtual para a sua aplicação web, não se ainda tiver feito, conforme descrito em [integrar a sua aplicação com uma rede virtual](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. No portal do Azure, para o plano de serviço de aplicações que alojam a aplicação web, selecione **sincronização rede** em **redes**, **integração de rede Virtual**.

    ![Captura de ecrã da resolução do nome de rede virtual](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Suponha que tem de efetuar a resolução do nome da sua aplicação web criada utilizando o serviço de aplicações, ligada a uma rede virtual, para as VMs numa rede virtual diferente. Isto requer a utilização de servidores DNS personalizados em ambas as redes virtuais, da seguinte forma: 
* Configure um servidor DNS na sua rede virtual de destino, numa VM que também pode reencaminhar consultas para o resolvedor recursivo no Azure (virtual IP 168.63.129.16). Um reencaminhador DNS de exemplo está disponível no [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) e [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Configure um reencaminhador DNS na rede virtual de origem, numa VM. Configure esta reencaminhador DNS para reencaminhar consultas para o servidor DNS na sua rede virtual de destino.
* Configure o servidor DNS de origem nas definições da sua origem da rede virtual.
* Ativar a integração de rede virtual para a sua aplicação web ligar à rede virtual de origem, siga as instruções no [integrar a sua aplicação com uma rede virtual](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* No portal do Azure, para o plano de serviço de aplicações que alojam a aplicação web, selecione **sincronização rede** em **redes**, **integração de rede Virtual**. 

## <a name="specify-dns-servers"></a>Especifique os servidores DNS
Quando estiver a utilizar os seus próprios servidores DNS, o Azure oferece a capacidade de especificar vários servidores DNS, por rede virtual. Também pode fazer isto, por interface de rede (para o Azure Resource Manager) ou por serviço em nuvem (para o modelo de implementação clássica). Servidores DNS especificados para uma interface de rede ou serviço em nuvem obter precedência através de servidores DNS especificados para a rede virtual.

> [!NOTE]
> Propriedades de ligação de rede, tais como servidor DNS IPs, não devem ser editadas diretamente no VMs do Windows. Isto acontece porque poderá obter apagadas durante o serviço heal quando o adaptador de rede virtual obtém substituído. 
> 
> 

Quando estiver a utilizar o modelo de implementação Azure Resource Manager, pode especificar os servidores DNS no portal do Azure. Consulte [redes virtuais](https://msdn.microsoft.com/library/azure/mt163661.aspx) e [Interfaces de rede](https://msdn.microsoft.com/library/azure/mt163668.aspx). Também pode fazer isto no PowerShell. Consulte [rede Virtual](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) e [interface de rede](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Quando estiver a utilizar o modelo de implementação clássica, pode especificar os servidores DNS para a rede virtual no portal do Azure ou o [ficheiro de configuração de rede](https://msdn.microsoft.com/library/azure/jj157100). Para serviços em nuvem, pode especificar servidores DNS através do [ficheiro de configuração do serviço](https://msdn.microsoft.com/library/azure/ee758710) ou utilizando o PowerShell, com [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Se alterar as definições de DNS para uma rede virtual ou a máquina virtual que já tenha sido implementada, terá de reiniciar cada VM afetado para que as alterações entrem em vigor.
> 
> 

## <a name="next-steps"></a>Passos Seguintes

Modelo de implementação Azure Resource Manager:

* [Criar ou atualizar uma rede virtual](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Criar ou atualizar uma placa de interface de rede](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Modelo de implementação clássica:

* [Esquema de configuração do serviço do Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Esquema de configuração de rede virtual](https://msdn.microsoft.com/library/azure/jj157100)
* [Configurar uma rede Virtual, utilizando um ficheiro de configuração de rede](virtual-networks-using-network-configuration-file.md)
