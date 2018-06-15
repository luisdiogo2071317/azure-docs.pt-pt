---
title: Configurar a replicação de cluster HBase em redes virtuais do Azure | Microsoft Docs
description: Saiba como configurar a replicação do HBase de uma versão do HDInsight para outro para o balanceamento de carga, elevada disponibilidade, a migração de período de indisponibilidade de zero e atualizações e recuperação após desastre.
services: hdinsight,virtual-network
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: jgao
ms.openlocfilehash: 56b2b5ae9d3e4a0e682ec3dd47cd5cc30ebf6d58
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077331"
---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Configurar a replicação de cluster HBase em redes virtuais do Azure

Saiba como configurar a replicação do HBase dentro de uma rede virtual, ou entre duas redes virtuais no Azure.

A replicação de cluster utiliza uma metodologia de push de origem. Um cluster HBase pode ser uma origem ou destino ou pode satisfazer ambas as funções de uma só vez. A replicação é assíncrona. O objetivo da replicação é a consistência eventual. Quando a origem recebe uma edição para uma família de coluna quando a replicação está ativada, a edição é propagada para todos os clusters de destino. Quando os dados são replicados de um cluster para outro, o cluster de origem e de todos os clusters que já tenham consumido os dados são controlados, para evitar os ciclos de replicação.

Neste tutorial, configurou uma replicação de destino de origem. Para outras topologias de cluster, consulte o [guia de referência Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).

Seguem-se HBase casos de utilização de replicação para uma única rede virtual:

* Balanceamento de carga. Por exemplo, pode executar análises ou as tarefas do MapReduce no cluster de destino e inserir dados no cluster de origem.
* A adicionar a elevada disponibilidade.
* Migrar dados de um cluster HBase para outro.
* Atualizar um cluster de Azure HDInsight a partir de uma versão para outro.

Seguem-se HBase casos de utilização de replicação para duas redes virtuais:

* Configurar a recuperação após desastre.
* Balanceamento de carga e a aplicação de criação de partições.
* A adicionar a elevada disponibilidade.

Pode replicar clusters utilizando [ação de script](../hdinsight-hadoop-customize-cluster-linux.md) scripts de [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de ter uma subscrição do Azure. Consulte [obtenha uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="set-up-the-environments"></a>Configurar os ambientes

Tem três opções de configuração:

- Dois clusters de HBase numa rede virtual do Azure.
- Dois clusters de HBase em duas redes virtuais diferentes na mesma região.
- Dois clusters de HBase em duas redes virtuais diferentes em duas regiões diferentes (georreplicação).

Este artigo abrange o cenário de georreplicação.

Para o ajudar a configurar os ambientes, criámos algumas [modelos Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Se preferir configurar os ambientes através da utilização de outros métodos, consulte:

- [Criar clusters do Hadoop no HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
- [Criar clusters de HBase na rede Virtual Azure](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>Configurar as duas redes virtuais em duas regiões diferentes

Para criar duas redes virtuais em duas regiões diferentes e a ligação VPN entre as VNets, selecione a imagem seguinte para criar o. O modelo é armazenado no [armazenamento de BLOBs público]] (https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json).

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Alguns dos valores no modelo hard-coded:

**VNet 1**

| Propriedade | Valor |
|----------|-------|
| Localização | EUA Oeste |
| Nome da VNet | &lt;ClusterNamePrevix >-vnet1 |
| Prefixo de espaço de endereço | 10.1.0.0/16 |
| Nome da sub-rede | sub-rede 1 |
| Prefixo de sub-rede | 10.1.0.0/24 |
| Nome da sub-rede (gateway) | GatewaySubnet (não é possível alterar) |
| Prefixo de sub-rede (gateway) | 10.1.255.0/27 |
| Nome do gateway | vnet1gw |
| Tipo de gateway | Vpn |
| Tipo de VPN do gateway | RouteBased |
| SKU de gateway | Básica |
| IP do gateway | vnet1gwip |

**VNet 2**

| Propriedade | Valor |
|----------|-------|
| Localização | EUA Leste |
| Nome da VNet | &lt;ClusterNamePrevix >-vnet2 |
| Prefixo de espaço de endereço | 10.2.0.0/16 |
| Nome da sub-rede | sub-rede 1 |
| Prefixo de sub-rede | 10.2.0.0/24 |
| Nome da sub-rede (gateway) | GatewaySubnet (não é possível alterar) |
| Prefixo de sub-rede (gateway) | 10.2.255.0/27 |
| Nome do gateway | vnet2gw |
| Tipo de gateway | Vpn |
| Tipo de VPN do gateway | RouteBased |
| SKU de gateway | Básica |
| IP do gateway | vnet1gwip |

## <a name="setup-dns"></a>Configuração DNS

Na última secção, o modelo cria uma máquina virtual de Ubuntu em cada uma das duas redes virtuais.  Nesta secção, instalar o enlace a duas máquinas virtuais DNS e, em seguida, configure o reencaminhamento de DNS nas duas máquinas virtuais.

Para instalar o enlace, yon tem de localizar o endereço IP público das duas máquinas virtuais DNS.

1. Abra o [Portal do Azure](https://portal.azure.com).
2. Abra a máquina virtual DNS, selecionando **grupos de recursos > [nome do grupo de recursos] > [vnet1DNS]**.  O nome do grupo de recursos é criado no último procedimento. Os nomes de máquina virtual DNS predefinido são *vnet1DNS* e *vnet2NDS*.
3. Selecione **propriedades** para abrir a página de propriedades da rede virtual.
4. Anote o **endereço IP público**e também verificar o **endereço IP privado**.  O endereço IP privado deverá ser **10.1.0.4** para vnet1DNS e **10.2.0.4** para vnet2DNS.  

Para instalar o enlace, utilize o seguinte procedimento:

1. Utilizar o SSH para ligar para o __endereço IP público__ da máquina virtual DNS. O exemplo seguinte estabelece ligação a uma máquina virtual em 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Substitua `sshuser` com a conta de utilizador SSH que especificou ao criar a máquina virtual DNS.

    > [!NOTE]
    > Existem várias formas de obter o `ssh` utilitário. Em Linux, Unix e macOS, é fornecido como parte do sistema operativo. Se estiver a utilizar o Windows, considere uma das seguintes opções:
    >
    > * [Shell de nuvem do Azure](../../cloud-shell/quickstart.md)
    > * [Bash no Ubuntu no Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Para instalar o enlace, utilize os seguintes comandos da sessão SSH:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Para configurar o enlace para reencaminhar pedidos de resolução de nome para o servidor DNS no local, utilize o seguinte texto como o conteúdo a `/etc/bind/named.conf.options` ficheiro:

    ```
    acl goodclients {
        10.1.0.0/16; # Replace with the IP address range of the virtual network 1
        10.2.0.0/16; # Replace with the IP address range of the virtual network 2
        localhost;
        localhost;
    };
    
    options {
        directory "/var/cache/bind";
        recursion yes;
        allow-query { goodclients; };

        forwarders {
            168.63.129.16 #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]
    > Substitua os valores no `goodclients` secção com o intervalo de endereços IP das duas redes virtuais. Esta secção define os endereços que aceita os pedidos deste servidor DNS.

    Para editar este ficheiro, utilize o seguinte comando:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Para guardar o ficheiro, utilize __Ctrl + X__, __Y__e, em seguida, __Enter__.

4. A partir de sessão SSH, utilize o seguinte comando:

    ```bash
    hostname -f
    ```

    Este comando devolve um valor semelhante para o seguinte texto:

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    O `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` texto é o __sufixo DNS__ para esta rede virtual. Guarde este valor, porque é utilizado mais tarde.

    Também pode descobrir o sufixo DNS do servidor DNS. Terá no próximo passo.

5. Para configurar o enlace para resolver nomes DNS para recursos dentro da rede virtual, utilize o seguinte texto como o conteúdo a `/etc/bind/named.conf.local` ficheiro:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]
    > Tem de substituir o `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` com o sufixo DNS da rede virtual. E o IP de reencaminhador é o endereço IP privado do servidor DNS na rede virtual.

    Para editar este ficheiro, utilize o seguinte comando:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Para guardar o ficheiro, utilize __Ctrl + X__, __Y__e, em seguida, __Enter__.

6. Para iniciar o enlace, utilize o seguinte comando:

    ```bash
    sudo service bind9 restart
    ```

7. Para verificar esse enlace pode resolver os nomes de recursos na rede virtual, utilize os seguintes comandos:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net 10.2.0.4
    ```

    > [!IMPORTANT]
    > Substitua `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` com o nome de domínio completamente qualificado (FQDN) da máquina virtual DNS na rede de outro.
    >
    > Substitua `10.2.0.4` com o __um endereço IP__ do servidor DNS personalizado na rede virtual.

    A resposta apresentado de forma semelhante para o seguinte texto:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Até agora, não é possível procurar o endereço IP da rede sem o endereço IP de servidor DNS especificado.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Configurar a rede virtual para utilizar o servidor DNS personalizado

Para configurar a rede virtual para utilizar o servidor DNS personalizado em vez do resolvedor recursivo do Azure, utilize os seguintes passos:

1. No [portal do Azure](https://portal.azure.com), selecione a rede virtual e, em seguida, selecione __servidores DNS__.

2. Selecione __personalizado__e introduza o __um endereço IP__ do servidor DNS personalizado. Por fim, selecione __guardar__.

6. Abra a máquina de virtual de servidor DNS vnet1 e clique em **reiniciar**.  Tem de reiniciar todas as máquinas virtuais na rede virtual para efetuar a configuração de DNS para o efeito.
7. Repita os passos de configurar o servidor DNS personalizado para a vnet2.

Para testar a configuração de DNS, pode ligar duas DNS nas máquinas virtuais utilizando o SSH e executar o ping do servidor DNS da rede virtual utilizando o respetivo nome de anfitrião. Se não funcionar, utilize o seguinte comando para verificar o estado DNS:

```bash
sudo service bind9 status
```

## <a name="create-hbase-clusters"></a>Criar clusters de HBase

Crie um cluster HBase em cada uma das duas redes virtuais com a seguinte configuração:

- **Nome do grupo de recursos**: utilizar o mesmo nome de grupo de recursos que criou redes virtuais.
- **Tipo de cluster**: HBase
- **Versão**: HBase 1.1.2 (HDI 3.6)
- **Localização**: utilizar a mesma localização que a rede virtual.  Por predefinição, é vnet1 *EUA oeste*, não sendo vnet2 *EUA Leste*.
- **Armazenamento**: criar uma nova conta de armazenamento para o cluster.
- **Rede virtual** (a partir de definições avançadas no portal): selecione vnet1 que criou no último procedimento.
- **Sub-rede**: O nome predefinido utilizado no modelo é **subnet1**.

Para garantir que o ambiente está configurado corretamente, tem de ser capaz de enviar um ping a headnode FQDN entre dois clusters.

## <a name="load-test-data"></a>Dados de teste de carga

Quando se replica um cluster, tem de especificar as tabelas que pretende replicar. Nesta secção, pode carrega alguns dados para o cluster de origem. Na secção seguinte, irá ativar a replicação entre dois clusters.

Para criar um **contactos** tabela e inserir alguns dados na tabela, siga as instruções apresentadas em [tutorial do HBase: introdução à utilização do Apache HBase no HDInsight](apache-hbase-tutorial-get-started-linux.md).

## <a name="enable-replication"></a>Ativar a replicação

Os passos seguintes descrevem como chamar o script de ação de script do portal do Azure. Para obter informações sobre como executar uma ação de script através do Azure PowerShell e a ferramenta de linha de comandos do Azure (CLI do Azure), consulte [clusters do HDInsight personalizar utilizando a ação de script](../hdinsight-hadoop-customize-cluster-linux.md).

**Para activar a replicação de HBase a partir do portal Azure**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Abra o cluster de HBase de origem.
3. No menu do cluster, selecione **ações de Script**.
4. Na parte superior da página, selecione **submeter novo**.
5. Selecione ou introduza as seguintes informações:

  1. **Nome**: introduzir **Activar replicação**.
  2. **Festa de Script URL**: introduzir **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.
  3.  **Cabeça**: Certifique-se de que esta opção está seleccionada. Desmarque os outros tipos de nó.
  4. **Parâmetros de**: os seguintes parâmetros de amostra activar a replicação para todas as tabelas existentes e, em seguida, copie todos os dados do cluster de origem para o cluster de destino:

          -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
    >[!note]
    >
    > Utilize nome de anfitrião em vez do FQDN do nome DNS de cluster de origem e de destino.

6. Selecione **Criar**. O script pode demorar algum tempo para executar, especialmente quando utiliza o **- copydata** argumento.

Argumentos necessários:

|Nome|Descrição|
|----|-----------|
|-s, - src-cluster | Especifica o nome DNS de cluster de HBase de origem. Por exemplo: hbsrccluster -s, de cluster – src = hbsrccluster |
|-d, – dst-cluster | Especifica o nome DNS de cluster de HBase de destino (réplica). Por exemplo: dsthbcluster -s, de cluster – src = dsthbcluster |
|-sp, – src-ambari-palavra-passe | Especifica a palavra-passe de administrador para Ambari no cluster de HBase de origem. |
|-ponto de distribuição, – dst-ambari-palavra-passe | Especifica a palavra-passe de administrador para Ambari no cluster de HBase de destino.|

Argumentos opcionais:

|Nome|Descrição|
|----|-----------|
|-su, – src-ambari-utilizador | Especifica o nome de utilizador de administrador para Ambari no cluster de HBase de origem. O valor predefinido é **admin**. |
|-du, – dst-ambari-utilizador | Especifica o nome de utilizador de administrador para Ambari no cluster de HBase de destino. O valor predefinido é **admin**. |
|-t, - lista de tabela | Especifica as tabelas replicadas. Por exemplo: – tabela lista = "tabela1; table2; table3". Se não especificar as tabelas, todas as tabelas de HBase existentes são replicadas.|
|-m, – máquina | Especifica o nó principal, onde é executada a ação de script. O valor é **hn1** ou **hn0**. Porque o **hn0** normalmente nó principal é busier, recomendamos que utilize **hn1**. Utilize esta opção se estiver a executar o script de $0 como uma ação de script do HDInsight portal ou do Azure PowerShell.|
|-cp, - copydata | Permite a migração dos dados existentes em tabelas em que a replicação está ativada. |
|-rpm, - replicar-phoenix-meta | Ativa a replicação em tabelas de sistema Phoenix. <br><br>*Utilize esta opção com cuidado.* Recomendamos que crie novamente as tabelas de Phoenix em clusters de réplica antes de utilizar este script. |
|-h, – ajuda | Mostra informações de utilização. |

O `print_usage()` secção o [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) tem uma explicação detalhada de parâmetros.

Depois da ação de script é implementada com êxito, pode utilizar o SSH para ligar ao cluster de HBase de destino e, em seguida, certifique-se de que os dados foram replicados.

### <a name="replication-scenarios"></a>Cenários de replicação

A lista seguinte mostra alguns casos de utilização geral e as respetivas definições de parâmetro:

- **Activar a replicação em todas as tabelas entre os dois clusters**. Este cenário não necessita de copiar ou migração de dados existentes nas tabelas e não utilize tabelas Phoenix. Utilize os seguintes parâmetros:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Activar a replicação em tabelas específicas**. Para ativar a replicação tabela1, table2 e table3, utilize os seguintes parâmetros:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Activar a replicação em tabelas específicas e copie os dados existentes**. Para ativar a replicação tabela1, table2 e table3, utilize os seguintes parâmetros:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Activar a replicação em todas as tabelas e replicar Phoenix metadados de origem para o destino**. O Phoenix a replicação de metadados não é perfeita. Utilizá-lo com cuidado. Utilize os seguintes parâmetros:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Copiar e migrar dados

Existem dois scripts de ação de script separado disponíveis para copiar ou migração de dados após a replicação está ativada:

- [Script para tabelas pequenas](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (tabelas que façam gigabytes alguns na cópia de tamanho e global é esperado para terminar em menos de uma hora)

- [Script para tabelas grandes](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (tabelas que sejam espera que a demorar mais do que uma hora para copiar)

Pode seguir o mesmo procedimento descrito [ativar a replicação](#enable-replication) para chamar a ação de script. Utilize os seguintes parâmetros:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

O `print_usage()` secção o [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) tem uma descrição detalhada dos parâmetros.

### <a name="scenarios"></a>Cenários

- **Copiar tabelas específicas (Teste1, Teste2 e test3) para todas as linhas editadas até agora (marca de hora actual)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Ou:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Copiar tabelas específicas com um intervalo de tempo especificado**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Desativar a replicação

Para desativar a replicação, utilize outro script de ação de script de [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh). Pode seguir o mesmo procedimento descrito [ativar a replicação](#enable-replication) para chamar a ação de script. Utilize os seguintes parâmetros:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

O `print_usage()` secção o [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) tem uma explicação detalhada de parâmetros.

### <a name="scenarios"></a>Cenários

- **Desactivar a replicação em todas as tabelas**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  ou

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Desactivar a replicação tabelas especificadas (tabela1, tabela2 e Tabela3)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a configurar a replicação do HBase dentro de uma rede virtual, ou entre duas redes virtuais. Para saber mais sobre o HBase e o HDInsight, consulte estes artigos:

* [Começar a trabalhar com Apache HBase na HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Descrição geral de HDInsight HBase](./apache-hbase-overview.md)
* [Criar clusters de HBase na rede Virtual Azure](./apache-hbase-provision-vnet.md)

