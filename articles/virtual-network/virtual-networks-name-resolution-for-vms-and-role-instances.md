---
title: "Resolução de nomes para VMs e instâncias de função"
description: "Cenários de resolução de nome para o IaaS do Azure, as soluções híbridas, entre diferentes serviços em nuvem, do Active Directory e utilizando o seu próprio servidor DNS."
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
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Resolução de nomes para VMs e instâncias de função

Dependendo de como utilizar o Azure para alojar o IaaS, PaaS e soluções híbridas, se pretender permitir que as VMs e instâncias de função que criar para comunicar entre si. Embora esta comunicação pode ser feita através da utilização de endereços IP, é muito mais simples utilizar nomes que podem ser memorizados facilmente e não alteram. 

Quando precisam de resolver os nomes de domínio para endereços IP internos instâncias de função e as VMs alojadas no Azure, podem utilizar um dos dois métodos:

* [Resolução de nome fornecidos pelo Azure](#azure-provided-name-resolution)
* [Resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server) (que pode reencaminhar consultas para os servidores DNS fornecidos pelo Azure) 

O tipo de resolução de nomes que utiliza depende de como as VMs e instâncias de função têm de comunicar entre si. A tabela seguinte ilustra os cenários e soluções de resolução de nome correspondente:

| **Cenário** | Solução | **Suffix** |
| --- | --- | --- |
| Resolução de nome entre instâncias de função ou VMs localizadas no mesmo serviço em nuvem ou de rede virtual. |[Resolução de nome fornecidos pelo Azure](#azure-provided-name-resolution) |nome de anfitrião ou o FQDN |
| Resolução de nome de um serviço de aplicações do Azure (aplicação Web, a função ou Bot) utilizando a integração de rede virtual para instâncias de função ou VMs localizado na mesma rede virtual. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Apenas FQDN |
| Resolução de nome entre instâncias de função ou VMs localizadas em redes virtuais diferentes. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Apenas FQDN |
| Resolução do nome de Web Apps do App Service para VMs localizado na mesma rede virtual. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Apenas FQDN |
| Resolução do nome de Web Apps do App Service para VMs localizados numa rede virtual diferente. |Gerida pelo cliente servidores DNS reencaminhamento consultas entre redes virtuais para a resolução pelo Azure (proxy DNS). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server-for-web-apps). |Apenas FQDN |
| Resolução de nomes de computador e o serviço no local de instâncias de função ou VMs no Azure. |Gerida pelo cliente servidores DNS (controlador de domínio no local, controlador de domínio só de leitura local ou um DNS secundário sincronizada com êxito utilizando transferências de zona, por exemplo). Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Apenas FQDN |
| Resolução de nomes de anfitrião do Azure a partir de computadores no local. |Consultas de reencaminhar para um servidor de proxy gerida pelo cliente DNS na rede virtual correspondente, o servidor de proxy reencaminha consultas para o Azure para a resolução. Consulte [resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Apenas FQDN |
| Inverte o DNS para IPs interno. |[Resolução de nomes utilizando o seu próprio servidor DNS](#name-resolution-using-your-own-dns-server). |Não aplicável |
| Resolução de nome entre VMs ou instâncias de função localizadas nos serviços de nuvem diferente, não numa rede virtual. |Não aplicável. A conectividade entre VMs e instâncias de função nos serviços de nuvem diferente não é suportada fora de uma rede virtual. |Não aplicável|

## <a name="azure-provided-name-resolution"></a>Resolução de nome fornecidos pelo Azure

Juntamente com a resolução de nomes DNS públicos, o Azure oferece resolução dos nomes internos para VMs e instâncias de função que residem dentro da mesma rede virtual ou serviço em nuvem. VMs/instâncias num serviço em nuvem partilham o mesmo sufixo DNS (para que o nome de anfitrião autónomo é suficiente) mas numa nuvem diferente de redes virtuais clássicas serviços têm diferentes sufixos DNS para que o FQDN é necessária para resolver nomes entre os serviços de nuvem diferente. Nas redes virtuais no modelo de implementação Resource Manager, o sufixo DNS é consistente entre a rede virtual (para que o FQDN não é necessário) e nomes DNS podem ser atribuídos a NICs e VMs. Apesar de resolução de nome fornecidos pelo Azure necessita de qualquer configuração, não é a opção adequada para todos os cenários de implementação, como mostrado na tabela anterior.

> [!NOTE]
> Quando utilizar funções web e de trabalho, também pode aceder os endereços IP internos de instâncias de função com base no número de nome e a instância de função utilizando a API de REST de gestão de serviço do Azure. Para obter mais informações, consulte [referência de API de REST de gestão de serviço](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Funcionalidades

* Facilidade de utilização: é necessária nenhuma configuração para poder utilizar a resolução de nome fornecidos pelo Azure.
* O serviço de resolução de nome fornecidos pelo Azure está altamente disponível, poupando a necessidade de criar e gerir clusters dos seus servidores DNS.
* Pode ser utilizado em conjunto com os seus próprios servidores DNS para resolver os nomes de anfitrião do Azure e no local.
* Resolução de nomes é fornecida entre instâncias de função/VMs no mesmo serviço em nuvem sem necessidade de um FQDN.
* Resolução de nomes é fornecida entre VMs em redes virtuais que utilizam o modelo de implementação Resource Manager, sem necessidade de um FQDN. Ao resolver os nomes de serviços de nuvem diferente as redes virtuais no modelo de implementação clássica requerem um FQDN. 
* Pode utilizar nomes de anfitriões que melhor descrevem as implementações, em vez de trabalhar com nomes gerado automaticamente.

### <a name="considerations"></a>Considerações

* Não é possível modificar o sufixo DNS criados no Azure.
* Não é possível registar manualmente os seus próprios registos.
* Não são suportados o WINS e NetBIOS (não é possível ver as suas VMs no Explorador do Windows).
* Os nomes de anfitrião tem de ser compatível com o DNS. Nomes têm de utilizar apenas 0-9, a-z e '-' e não pode começar nem terminar com um '-'. Consulte RFC 3696 secção 2.
* O tráfego de consulta DNS está limitado para cada VM. Limitação não deve afetar a maioria das aplicações. Se a limitação do pedido é observado, certifique-se de que a colocação em cache do lado do cliente está ativada. Para obter mais informações, consulte [ao obter o máximo partido do resolução de nome fornecidos pelo Azure](#Getting-the-most-from-Azure-provided-name-resolution).
* Apenas as VMs nos serviços de 180 nuvem primeiro estão registadas para cada rede virtual de um modelo de implementação clássica. Este limite não é aplicável a redes virtuais em modelos de implementação do Resource Manager.

## <a name="dns-client-configuration"></a>Configuração de cliente DNS

### <a name="client-side-caching"></a>Colocação em cache do lado do cliente

Nem todas as consultas DNS tem de ser enviadas através da rede. Colocação em cache do lado do cliente ajuda a reduzir a latência e melhorar a maior resiliência às blips de rede ao resolver as consultas DNS periódicas a partir de uma cache local. Registos DNS contêm uma time-to-live (TTL) que permite que a cache armazenar o registo para desde possíveis sem afetar a actualização de registo, pelo que a colocação em cache do lado do cliente é adequada para a maioria das situações.

A predefinição do cliente DNS do Windows tem uma cache DNS incorporada. Alguns distros de Linux não incluem a colocação em cache por predefinição. A adição de uma cache DNS a cada VM com Linux (após verificar que não é uma cache local já), é recomendado.

Há uma série de DNS diferente, colocação em cache pacotes disponíveis. Por exemplo, dnsmasq. Os seguintes passos listam como instalar dnsmasq na distros mais comuns:

* **Ubuntu (utiliza resolvconf)**:
  * Instalar o pacote de dnsmasq com `sudo apt-get install dnsmasq`.
* **SUSE (utiliza netconf)**:
  * Instalar o pacote de dnsmasq com `sudo zypper install dnsmasq`.
  * Ativar o serviço de dnsmasq com `systemctl enable dnsmasq.service`. 
  * Iniciar o serviço de dnsmasq com `systemctl start dnsmasq.service`. 
  * Editar **/etc/sysconfig/network/config** e alterar *NETCONFIG_DNS_FORWARDER = ""* para *dnsmasq*.
  * Atualizar resolv.conf com `netconfig update` para definir a cache de como a resolução DNS local.
* **OpenLogic (utiliza NetworkManager)**:
  * Instalar o pacote de dnsmasq com `sudo yum install dnsmasq`.
  * Ativar o serviço de dnsmasq com `systemctl enable dnsmasq.service`.
  * Iniciar o serviço de dnsmasq com `systemctl start dnsmasq.service`.
  * Adicionar *preceder nome-servidores domínio 127.0.0.1;* para **/etc/dhclient-eth0.conf**.
  * Reinicie o serviço de rede com `service network restart` para definir a cache de como a resolução DNS local.

> [!NOTE]
> O pacote de 'dnsmasq' é apenas um dos muitos caches DNS disponíveis para o Linux. Antes de o utilizar, verifique a respetiva adequação para as suas necessidades particulares e que não existem outro cache está instalado.
> 
> 
    
### <a name="client-side-retries"></a>Tentativas de lado do cliente

O DNS é principalmente um protocolo UDP. Como o protocolo UDP não garante a entrega de mensagens, lógica de repetição é processada no próprio protocolo DNS. Cada cliente DNS (sistema operativo) pode apresentem um lógica de repetição diferentes consoante preferência o criador:

* Sistemas operativos Windows novamente depois de um segundo e, em seguida, novamente após outro 2, 4 e outro quatro segundos. 
* As repetições a configuração do Linux predefinida que se após cinco segundos. Alterar o novamente para cinco vezes em intervalos de 1 segundo, é recomendado.

Verifique as definições atuais numa VM com Linux com `cat /etc/resolv.conf`. Observe o *opções* linha, por exemplo:

```bash
options timeout:1 attempts:5
```

O ficheiro resolv.conf, normalmente, é gerado para automática e não deve ser editado. Os passos específicos para adicionar o *opções* linha variam consoante o distro:

* **Ubuntu** (utiliza resolvconf):
  * Adicione a linha de opções para **/etc/resolveconf/resolv.conf.d/head**.
  * Executar `resolvconf -u` para atualizar.
* **SUSE** (utiliza netconf):
  * Adicionar *timeout:1 tentativas: 5* para o **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parâmetro **/etc/sysconfig/network/config**. 
  * Executar `netconfig update` para atualizar.
* **OpenLogic** (utiliza NetworkManager):
  * Adicionar *eco "timeout:1 tentativas: 5 opções"* para **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  * Atualizar com `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Resolução de nomes utilizando o seu próprio servidor DNS

### <a name="vms-and-role-instances"></a>VMs e instâncias de função

Existem várias situações em que as suas necessidades de resolução de nome poderão ir para além das funcionalidades fornecidas pelo Azure, por exemplo quando utilizar domínios do Active Directory ou quando necessitar de resolução de DNS entre redes virtuais. Para estes cenários, o Azure oferece a capacidade para que possa utilizar os seus próprios servidores DNS.

Servidores DNS dentro de uma rede virtual podem reencaminhar consultas DNS para resoluções de recursiva do Azure para resolver os nomes de anfitrião dentro da rede virtual. Por exemplo, um controlador de domínio (DC) em execução no Azure pode responder às consultas DNS para os domínios e reencaminhar todas as outras consultas para o Azure. Consultas de reencaminhamento, permite que as VMs ver os seus recursos no local (através de DC) e o hostnames fornecidos pelo Azure (através do reencaminhador). Acesso para resoluções de recursiva do Azure é fornecido através de virtual IP 168.63.129.16.

Reencaminhamento de DNS também permite a resolução de DNS de rede intervirtual e permite que as máquinas no local resolver os nomes de anfitrião fornecidos pelo Azure. Para resolver o nome do anfitrião da VM, o servidor DNS VM tem de residir na mesma rede virtual e de ser configurado para consultas de nome de anfitrião direta para o Azure. Como o sufixo DNS é diferente em cada rede virtual, pode utilizar regras de reencaminhamento condicional para enviar consultas DNS para a rede virtual correta para a resolução. A imagem seguinte mostra duas redes virtuais e uma rede no local ao executar a resolução DNS ao utilizar este método de rede intervirtual. Um reencaminhador DNS de exemplo está disponível no [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) e [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Instâncias de função podem efetuar a resolução de nome de VMs dentro da mesma rede virtual utilizando o FQDN que utiliza o nome da VM, juntamente com o sufixo DNS de "internal.cloudapp.net". No entanto, neste caso, resolução de nomes só for bem sucedida se a instância de função tem o nome VM definido no [esquema de função (ficheiro. cscfg)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Instâncias de função que tem de efetuar a resolução de nome de VMs na outra rede virtual (utilizando o FQDN do **internal.cloudapp.net** sufixo) tem de fazê-lo através de servidores DNS personalizados reencaminhamento entre as duas redes virtuais, conforme descrito em nesta secção.
>

![Rede intervirtual DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Ao utilizar a resolução de nome fornecidos pelo Azure, um sufixo DNS interno (`*.internal.cloudapp.net`) é fornecido para cada VM através do Azure DHCP. Este sufixo permite que a resolução de nome de anfitrião dado que os registos de nome do anfitrião estão no *internal.cloudapp.net* zona. Quando utilizar a sua própria solução de resolução de nome, este sufixo não é fornecido para VMs porque-interfere com outras arquiteturas DNS (por exemplo, cenários de associados a um domínio). Em vez disso, o Azure oferece um marcador de posição não está em funcionamento (*reddog.microsoft.com*).

Se for necessário, é possível determinar o sufixo DNS interna utilizando o PowerShell ou a API:

* Para as redes virtuais em modelos de implementação do Resource Manager, o sufixo está disponível através de [cartão de interface de rede](virtual-network-network-interface.md) recursos ou através do [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) cmdlet.
* Modelos de implementação clássica, o sufixo está disponível através de [obter API de implementação](https://msdn.microsoft.com/library/azure/ee460804.aspx) chamar ou através do [Get-AzureVM-depurar](/powershell/module/azure/get-azurevm) cmdlet.

Se as consultas de reencaminhamento para o Azure não conforme as suas necessidades, terá de fornecer a sua própria solução DNS. A solução DNS tem de:

* Fornecer a resolução de nome de anfitrião adequado, através de [DDNS](virtual-networks-name-resolution-ddns.md), por exemplo. Tenha em atenção que se utilizar DDNS poderá ter de desativar a eliminação de registos DNS como concessões DHCP do Azure são longos e eliminação poderá remover DNS regista prematuramente. 
* Resolução recursiva adequadas para permitir a resolução de nomes de domínio externo.
* Estar acessível (TCP e UDP na porta 53) dos clientes que tem e de conseguir aceder à internet.
* Estar protegida contra acesso a partir da internet, a mitigar ameaças colocadas por agentes externos.

> [!NOTE]
> Para melhor desempenho, ao utilizar VMs do Azure como servidores DNS, IPv6 devem ser desativados e um [IP público de nível de instância](virtual-networks-instance-level-public-ip.md) deve ser atribuído a cada VM de servidor DNS. Para análise de desempenho adicionais e otimizações ao utilizar o Windows Server como o servidor DNS, consulte [nome desempenho de resolução de um recursivo de DNS de Windows Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Aplicações Web
Se precisar de efetuar a resolução do nome da sua aplicação Web do App Service ligada a uma rede virtual, para as VMs na mesma rede virtual, em seguida, para além de configurar um servidor DNS personalizado que tem um reencaminhador DNS que reencaminha consultas para o Azure (virtual IP 168.63.129.16) , também terá de efetuar os seguintes passos:
* Ativar a integração de rede virtual para a aplicação Web do App Service, não se ainda tiver feito, conforme descrito em [integrar a sua aplicação com uma rede virtual](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* No portal do Azure, para o plano de serviço aplicacional que aloja a aplicação Web, selecione **sincronização rede** em **redes**, **integração de rede Virtual**, como mostrado a seguir imagem:

    ![Resolução de nomes de rede virtual de aplicações Web](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Resolução do nome da sua aplicação de Web do serviço de aplicação associada a uma rede virtual, para as VMs numa rede virtual diferente requer a utilização de servidores DNS personalizados em ambas as redes virtuais, da seguinte forma:
* Configure um servidor DNS na sua rede virtual de destino numa VM que também pode reencaminhar consultas para resolução recursiva do Azure (virtual IP 168.63.129.16). Um reencaminhador DNS de exemplo está disponível no [Galeria de modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) e [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Configure um reencaminhador DNS na rede virtual de origem, numa VM. Configure esta reencaminhador DNS para reencaminhar consultas para o servidor DNS na sua rede virtual de destino.
* Configure o servidor DNS de origem nas definições da sua origem da rede virtual.
* Ativar a integração de rede virtual para a sua aplicação Web do App Service ligar à rede virtual de origem, siga as instruções no [integrar a sua aplicação com uma rede virtual](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* No portal do Azure, para o plano de serviço aplicacional que aloja a aplicação web, selecione **sincronização rede** em **redes**, **integração de rede Virtual**. 

## <a name="specifying-dns-servers"></a>Especificar os servidores DNS
Ao utilizar os seus próprios servidores DNS, o Azure oferece a capacidade de especificar vários servidores DNS, por rede virtual ou por interface de rede (Resource Manager) / (clássica) do serviço em nuvem. Servidores DNS especificados para uma interface de rede/serviço de nuvem obter precedência através de servidores DNS especificados para a rede virtual.

> [!NOTE]
> Propriedades de ligação de rede, tais como o servidor DNS IPs, não deve ser editada diretamente no VMs do Windows, pode obter apagadas durante o serviço heal quando o adaptador de rede virtual obtém substituído. 
> 
> 

Ao utilizar o modelo de implementação Resource Manager, os servidores DNS podem ser especificados no Portal, API/modelos: [rede Virtual](https://msdn.microsoft.com/library/azure/mt163661.aspx) e [interface de rede](https://msdn.microsoft.com/library/azure/mt163668.aspx), ou o PowerShell: [rede Virtual ](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) e [interface de rede](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Quando utilizar o modelo de implementação clássica, servidores DNS para a rede virtual podem ser especificados no Portal ou [o *configuração de rede* ficheiro](https://msdn.microsoft.com/library/azure/jj157100). Para serviços em nuvem, os servidores DNS são especificados através do [o *a configuração do serviço* ficheiro](https://msdn.microsoft.com/library/azure/ee758710) ou utilizando o PowerShell, com [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Se alterar as definições de DNS para uma máquina virtual/virtual de rede que já tenha sido implementado, terá de reiniciar cada VM afetado para que as alterações entrem em vigor.
> 
> 

## <a name="next-steps"></a>Passos Seguintes

Modelo de implementação do Resource Manager:

* [Criar ou atualizar uma rede virtual](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Criar ou atualizar uma placa de interface de rede](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Modelo de implementação clássica:

* [Esquema de configuração do serviço do Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Esquema de configuração de rede virtual](https://msdn.microsoft.com/library/azure/jj157100)
* [Configurar uma rede Virtual, utilizando um ficheiro de configuração de rede](virtual-networks-using-network-configuration-file.md)
