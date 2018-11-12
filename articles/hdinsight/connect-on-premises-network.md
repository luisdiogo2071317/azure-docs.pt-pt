---
title: Ligar o HDInsight à sua rede no local - Azure HDInsight
description: Saiba como criar um cluster de HDInsight numa rede Virtual do Azure e, em seguida, ligá-la à sua rede no local. Saiba como configurar a resolução de nome entre o HDInsight e a sua rede no local ao utilizar um servidor DNS personalizado.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: hrasheed
ms.openlocfilehash: d8ab61d99b617f21557b6f1ef476418e25bcee55
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016935"
---
# <a name="connect-hdinsight-to-your-on-premises-network"></a>Ligar o HDInsight à sua rede no local

Saiba como ligar o HDInsight à sua rede no local através de redes virtuais do Azure e um gateway de VPN. Este documento fornece informações de planeamento sobre:

* Utilizar o HDInsight numa rede Virtual do Azure que liga à sua rede no local.

* Configurar a resolução de nomes DNS entre a rede virtual e a sua rede no local.

* Configurar grupos de segurança de rede para restringir o acesso à internet para o HDInsight.

* Portas fornecidas pelo HDInsight na rede virtual.

## <a name="create-the-virtual-network-configuration"></a>Criar a configuração de rede Virtual

Utilize os seguintes documentos para saber como criar uma rede Virtual do Azure que está ligada à sua rede no local:
    
* [Com o Portal do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Utilizar o Azure PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Utilizar a CLI do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Configurar a resolução de nome

Para permitir que o HDInsight e os recursos da rede associados a um para se comunicar por nome, tem de efetuar as seguintes ações:

* Crie um servidor DNS personalizado na rede Virtual do Azure.

* Configure a rede virtual para utilizar o servidor DNS personalizado em vez da predefinição resolvedor recursivo de Azure.

* Configure o reencaminhamento entre o servidor DNS personalizado e o seu servidor DNS no local.

Esta configuração permite que o comportamento seguinte:

* Pedidos para nomes de domínio completamente qualificado que têm o sufixo DNS __para a rede virtual__ são reencaminhados para o servidor DNS personalizado. O servidor DNS personalizado, em seguida, encaminha estes pedidos para o Resolvedor de recursiva do Azure, que retorna o endereço IP.

* Todos os outros pedidos são reencaminhados para o servidor DNS no local. Até mesmo solicitações de recursos de internet público, como o microsoft.com são reencaminhadas para o servidor DNS no local para resolução de nomes.

No diagrama seguinte, o verde linhas são pedidos de recursos que terminam no sufixo DNS da rede virtual. Linhas azuis são pedidos de recursos na rede no local ou na internet pública.

![Diagrama de como são resolvidos os pedidos DNS na configuração utilizada neste documento](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Criar um servidor DNS personalizado

> [!IMPORTANT]
> Tem de criar e configurar o servidor DNS antes de instalar HDInsight numa rede virtual.

Para criar uma VM do Linux que utiliza a [vincular](https://www.isc.org/downloads/bind/) software DNS, utilize os seguintes passos:

> [!NOTE]
> Os passos seguintes utilizam o [portal do Azure](https://portal.azure.com) para criar uma Máquina Virtual do Azure. Para outras formas de criar uma máquina virtual, veja os documentos seguintes:
>
> * [Criar VM - CLI do Azure](../virtual-machines/linux/quick-create-cli.md)
> * [Criar VM - PowerShell do Azure](../virtual-machines/linux/quick-create-portal.md)

1. Partir do [portal do Azure](https://portal.azure.com), selecione __+__, __computação__, e __Ubuntu Server 16.04 LTS__.

    ![Criar uma máquina virtual do Ubuntu](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Na secção __Informações Básicas__, introduza as seguintes informações:

    * __Nome__: um nome amigável que identifique esta máquina virtual. Por exemplo, __DNSProxy__.
    * __Nome de utilizador__: O nome da conta do SSH.
    * __Chave pública SSH__ ou __palavra-passe__: O método de autenticação para a conta SSH. Recomendamos a utilização de chaves públicas, como eles são mais seguros. Para obter mais informações, consulte a [criar e utilizar chaves SSH para VMs do Linux](../virtual-machines/linux/mac-create-ssh-keys.md) documento.
    * __Grupo de recursos__: selecione __utilizar existente__e, em seguida, selecione o grupo de recursos que contém a rede virtual que criou anteriormente.
    * __Localização__: selecione a mesma localização que a rede virtual.

    ![Configuração básica da máquina virtual](./media/connect-on-premises-network/vm-basics.png)

    Deixe as outras entradas com os valores predefinidos e, em seguida, selecione __OK__.

3. Partir do __escolher um tamanho__ secção, selecione o tamanho da VM. Para este tutorial, selecione a opção de menor e mais baixo custo. Para continuar, utilize o __selecione__ botão.

4. Partir do __definições__ secção, introduza as seguintes informações:

    * __Rede virtual__: selecione a rede virtual que criou anteriormente.

    * __Sub-rede__: selecione a sub-rede de predefinição para a rede virtual. Fazer __não__ selecione a sub-rede utilizada pelo gateway de VPN.

    * __Conta de armazenamento de diagnóstico__: selecione uma conta de armazenamento existente ou crie um novo.

    ![Definições de rede virtual](./media/connect-on-premises-network/virtual-network-settings.png)

    Deixe as outras entradas com o valor predefinido, em seguida, selecione __OK__ para continuar.

5. Partir do __Compra__ secção, selecione a __Compra__ botão para criar a máquina virtual.

6. Quando a máquina virtual tiver sido criada, sua __descrição geral__ secção é apresentada. Na lista à esquerda, selecione __propriedades__. Guardar a __endereço IP público__ e __endereço IP privado__ valores. Será utilizado na secção seguinte.

    ![Endereços IP públicos e privados](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Instalar e configurar o enlace (software DNS)

1. Utilize SSH para ligar para o __endereço IP público__ da máquina virtual. O exemplo seguinte liga a uma máquina virtual em 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Substitua `sshuser` com a conta de utilizador SSH que especificou ao criar o cluster.

    > [!NOTE]
    > Existem diversas formas de obter o `ssh` utilitário. No Linux, Unix e macOS, ele é fornecido como parte do sistema operativo. Se estiver a utilizar o Windows, considere uma das seguintes opções:
    >
    > * [Azure Cloud Shell](../cloud-shell/quickstart.md)
    > * [Bash no Ubuntu no Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Para instalar o enlace, utilize os seguintes comandos a partir da sessão SSH:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Para configurar o enlace para reencaminhar pedidos de resolução de nome ao seu servidor DNS no local, utilize o seguinte texto como conteúdo do `/etc/bind/named.conf.options` ficheiro:

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Substitua os valores no `goodclients` secção com o intervalo de endereços IP da rede virtual e rede no local. Esta secção define os endereços que aceita os pedidos deste servidor DNS.
    >
    > Substitua a `192.168.0.1` entrada no `forwarders` secção com o endereço IP do servidor DNS no local. Esta entrada encaminha os pedidos DNS para o seu servidor DNS no local para a resolução.

    Para editar este ficheiro, utilize o seguinte comando:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Para guardar o ficheiro, utilize __Ctrl + X__, __Y__e, em seguida __Enter__.

4. A partir da sessão SSH, utilize o seguinte comando:

    ```bash
    hostname -f
    ```

    Este comando devolve um valor semelhante ao seguinte texto:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    O `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` texto é o __sufixo DNS__ para esta rede virtual. Guarde este valor, porque vai ser utilizado mais tarde.

5. Para configurar o enlace para resolver os nomes DNS para recursos dentro da rede virtual, utilize o seguinte texto como conteúdo do `/etc/bind/named.conf.local` ficheiro:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]
    > Tem de substituir o `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` com o sufixo DNS que obteve anteriormente.

    Para editar este ficheiro, utilize o seguinte comando:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Para guardar o ficheiro, utilize __Ctrl + X__, __Y__e, em seguida __Enter__.

6. Para iniciar a ligação, utilize o seguinte comando:

    ```bash
    sudo service bind9 restart
    ```

7. Para verificar essa associação pode resolver os nomes de recursos na sua rede no local, utilize os seguintes comandos:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Substitua `dns.mynetwork.net` com o nome de domínio completamente qualificado (FQDN) de um recurso na sua rede no local.
    >
    > Substitua `10.0.0.4` com o __endereço IP interno__ do seu servidor DNS personalizado na rede virtual.

    A resposta parece ser semelhante ao seguinte texto:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Configurar a rede virtual para utilizar o servidor DNS personalizado

Para configurar a rede virtual para utilizar o servidor DNS personalizado em vez do resolvedor recursivo do Azure, utilize os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com), selecione a rede virtual e, em seguida, selecione __servidores DNS__.

2. Selecione __personalizados__e introduza o __endereço IP interno__ do servidor DNS personalizado. Por fim, selecione __guardar__.

    ![Defina o servidor DNS personalizado para a rede](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-the-on-premises-dns-server"></a>Configurar o servidor DNS no local

Na secção anterior, configurou o servidor DNS personalizado para encaminhar pedidos para o servidor DNS no local. Em seguida, tem de configurar o servidor DNS no local para reencaminhar pedidos para o servidor DNS personalizado.

Para obter passos específicos sobre como configurar seu servidor DNS, consulte a documentação para o seu software de servidor DNS. Procure os passos sobre como configurar uma __reencaminhador condicional__.

Um reencaminhamento condicional apenas reencaminha os pedidos para um sufixo DNS específico. Neste caso, deve configurar um encaminhador para o sufixo DNS da rede virtual. Pedidos para este sufixo devem ser reencaminhados para o endereço IP do servidor DNS personalizado. 

O texto seguinte é um exemplo de uma configuração de reencaminhador condicional para o **vincular** software DNS:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

Para obter informações sobre como utilizar o DNS no **Windows Server 2016**, consulte a [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) documentação...

Assim que tiver configurado o servidor DNS no local, pode utilizar `nslookup` da rede no local para verificar que pode resolver os nomes na rede virtual. O exemplo seguinte 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Este exemplo utiliza o servidor DNS no local em 196.168.0.4 para resolver o nome do servidor DNS personalizado. Substitua o endereço IP para o servidor DNS no local. Substitua o `dnsproxy` endereço com o nome de domínio completamente qualificado do servidor DNS personalizado.

## <a name="optional-control-network-traffic"></a>Opcional: Controlar o tráfego de rede

Pode utilizar grupos de segurança de rede (NSG) ou rotas definidas pelo utilizador (UDR) para controlar o tráfego de rede. Os NSGs permitem-lhe filtrar o tráfego de entrada e saído e permitir ou negar o tráfego. As UDRs permitem-lhe controlar a forma como o tráfego flui entre os recursos da rede virtual, a internet e a rede no local.

> [!WARNING]
> HDInsight requer acesso de entrada de endereços IP específicos na cloud do Azure e o acesso sem restrições de saída. Quando utilizar NSGs ou UDRs para controlar o tráfego, tem de efetuar os seguintes passos:

1. Localize os endereços IP para a localização que contém a sua rede virtual. Para obter uma lista de IPs necessária por localização, consulte [endereços IP necessários](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).

2. Para os endereços IP identificados na etapa 1, permitir o tráfego de entrada desse IP endereços.

   * Se estiver a utilizar __NSG__: permitir __entrada__ tráfego na porta __443__ para endereços de IP.
   * Se estiver a utilizar __UDR__: definir o __próximo salto__ tipo da rota para __Internet__ para endereços de IP.

Para obter um exemplo de utilização do Azure PowerShell ou a CLI do Azure para criar NSGs, consulte a [estender o HDInsight com redes virtuais do Azure](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) documento.

## <a name="create-the-hdinsight-cluster"></a>Criar o cluster do HDInsight

> [!WARNING]
> Tem de configurar o servidor DNS personalizado antes de instalar HDInsight na rede virtual.

Utilize os passos no [criar um cluster do HDInsight com o portal do Azure](./hdinsight-hadoop-create-linux-clusters-portal.md) documento para criar um cluster do HDInsight.

> [!WARNING]
> * Durante a criação do cluster, tem de escolher a localização que contém a sua rede virtual.
>
> * Na __definições avançadas__ parte da configuração, tem de selecionar a rede virtual e a sub-rede que criou anteriormente.

## <a name="connecting-to-hdinsight"></a>Ligar ao HDInsight

A maioria dos documentação no HDInsight assume que tem acesso ao cluster através da internet. Por exemplo, que pode ligar ao cluster em https://CLUSTERNAME.azurehdinsight.net. Este endereço utiliza o gateway público, que não está disponível se tiver utilizado os NSGs ou UDRs para restringir o acesso a partir da internet.

Também faz referência a alguma documentação `headnodehost` quando ligar ao cluster a partir de uma sessão SSH. Este endereço só está disponível a partir de nós num cluster e não pode ser utilizado nos clientes ligados através da rede virtual.

Para ligar diretamente ao HDInsight através da rede virtual, utilize os seguintes passos:

1. Para descobrir os nomes de domínio completamente qualificado interno de nós de cluster do HDInsight, utilize um dos seguintes métodos:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. Para determinar a porta que um serviço está disponível em, veja a [portas utilizadas pelo serviços do Hadoop no HDInsight](./hdinsight-hadoop-port-settings-for-services.md) documento.

    > [!IMPORTANT]
    > Alguns serviços alojados nos nós principais são apenas o Active Directory num nó de cada vez. Se tentar acessar um serviço num nó principal e este falhar, mude para o outro nó principal.
    >
    > Por exemplo, Ambari só está ativo no nó principal um cada vez. Se tentar acessar Ambari num nó principal e retorna um erro 404, em seguida, ele é executado no nó principal.

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre como utilizar o HDInsight numa rede virtual, consulte [expandir HDInsight ao utilizar redes virtuais do Azure](./hdinsight-extend-hadoop-virtual-network.md).

* Para obter mais informações sobre redes virtuais do Azure, consulte a [descrição geral da rede Virtual do Azure](../virtual-network/virtual-networks-overview.md).

* Para obter mais informações sobre grupos de segurança de rede, consulte [grupos de segurança de rede](../virtual-network/security-overview.md).

* Para obter mais informações sobre as rotas definidas pelo utilizador, consulte [rotas definidas pelo utilizador e reencaminhamento IP](../virtual-network/virtual-networks-udr-overview.md).
