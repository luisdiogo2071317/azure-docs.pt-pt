---
title: Executar um cluster MariaDB (MySQL) no Azure | Documentos da Microsoft
description: Criar um MariaDB + Galera MySQL de cluster em máquinas virtuais do Azure
services: virtual-machines-linux
documentationcenter: ''
author: sabbour
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: 2cdc58a827f696d62e6240b90202ee04ce371d07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426858"
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>Cluster MariaDB (MySQL): tutorial do Azure
> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [do Azure Resource Manager](../../../resource-manager-deployment-model.md) e clássico. Este artigo abrange o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Azure Resource Manager.

> [!NOTE]
> Cluster MariaDB empresarial está agora disponível no Azure Marketplace. A nova oferta irá implementar automaticamente um cluster MariaDB Galera sobre o Azure Resource Manager. Deve usar a nova oferta do [do Azure Marketplace](https://azure.microsoft.com/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Este artigo mostra-lhe como criar uma com vários mestres [Galera](http://galeracluster.com/products/) cluster de [MariaDBs](https://mariadb.org/en/about/) (uma substituição para soltar robusta, escalável e fiável para o MySQL) para trabalhar num ambiente de elevada disponibilidade no Azure máquinas virtuais.

## <a name="architecture-overview"></a>Descrição geral da arquitetura
Este artigo descreve como concluir os seguintes passos:

- Crie um cluster de três nós.
- Separe os discos de dados do disco do SO.
- Crie os discos de dados na definição de RAID-0/repartidos para aumentar o IOPS.
- Utilize o Azure Load Balancer para balancear a carga para os três nós.
- Para minimizar o trabalho repetitivo, criar uma imagem VM que contém MariaDB + Galera e utilizá-lo para criar o cluster VMs.

![Arquitetura do sistema](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Este tópico utiliza a [CLI do Azure](../../../cli-install-nodejs.md) ferramentas, por isso, certifique-se para baixá-los e ligue-os à sua subscrição do Azure, de acordo com as instruções. Se precisar de uma referência para os comandos disponíveis na CLI do Azure, consulte a [referência de comandos da CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Também terá [criar uma chave SSH para autenticação] e anote a localização do ficheiro. pem.
>
>

## <a name="create-the-template"></a>Criar o modelo
### <a name="infrastructure"></a>Infraestrutura
1. Crie um grupo de afinidade para conter os recursos em conjunto.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
1. Crie uma rede virtual

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
1. Crie uma conta de armazenamento para alojar todos os nossos discos. Não deve colocar mais de 40 discos muito utilizados na mesma conta de armazenamento para evitar atingir o limite de conta de armazenamento IOPS 20.000. Neste caso, está bem abaixo desse limite, pelo que irá armazenar tudo na mesma conta para manter a simplicidade.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
1. Localize o nome da imagem de máquina virtual CentOS 7.

        azure vm image list | findstr CentOS
   A saída será algo como `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Utilize esse nome no passo seguinte.
1. Criar o modelo de VM e substituir /path/to/key.pem com o caminho onde armazenou a chave SSH. pem gerado.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
1. Anexe quatro discos de dados de 500 GB para a VM para utilização na configuração do RAID.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
1. Utilizar o SSH para iniciar sessão para o modelo de VM que criou no mariadbhatemplate.cloudapp.net:22 e ligar com a sua chave privada.

### <a name="software"></a>Software
1. Obter a raiz.

        sudo su

1. Instale o suporte RAID:

    a. Instale mdadm.

              yum install mdadm

    b. Crie a configuração de RAID0/stripe com um sistema de ficheiros de EXT4.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Crie o diretório de ponto de montagem.

              mkdir /mnt/data
    d. Obter o UUID do dispositivo RAID recém-criado.

              blkid | grep /dev/md0
    e. Edite /etc/fstab.

              vi /etc/fstab
    f. Adicionar o dispositivo para ativar a montagem automática no reinício, substituindo o UUID com o valor obtido a partir do anterior **blkid** comando.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Monte a partição de novo.

              mount /mnt/data

1. Instale MariaDB.

    a. Crie o ficheiro de MariaDB.repo.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Preencha o arquivo de repositório com o seguinte conteúdo:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Para evitar conflitos, remova o sufixo existente e mariadb bibliotecas.

           yum remove postfix mariadb-libs-*
    d. Instale MariaDB com Galera.

           yum install MariaDB-Galera-server MariaDB-client galera

1. Mova o diretório de dados do MySQL para o dispositivo de bloco RAID.

    a. Copie o diretório atual do MySQL para a nova localização e remover o diretório antigo.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Defina permissões para o novo diretório em conformidade.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Crie um symlink que aponta o diretório antigo para a nova localização na partição de RAID.

           ln -s /mnt/data/mysql /var/lib/mysql

1. Uma vez [SELinux interfere com as operações de cluster](http://galeracluster.com/documentation-webpages/configuration.html#selinux), é necessário para desabilitá-lo para a sessão atual. Editar `/etc/selinux/config` desabilitá-lo para reinícios subsequentes.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
1. Valide execuções do MySQL.

   a. Inicie o MySQL.

           service mysql start
   b. Proteger a instalação do MySQL, definir a palavra-passe de raiz, remover utilizadores anónimos para desativar o início de sessão remoto raiz e remover a base de dados de teste.

           mysql_secure_installation
   c. Crie um utilizador na base de dados para operações de cluster e, opcionalmente, para as suas aplicações.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. Pare o MySQL.

            service mysql stop
1. Crie um marcador de posição de configuração.

   a. Edite a configuração do MySQL para criar um espaço reservado para as definições do cluster. Não substitua a **`<Variables>`** ou anule os comentários agora. Tal irá acontecer depois de criar uma VM a partir deste modelo.

            vi /etc/my.cnf.d/server.cnf
   b. Editar a **[galera]** secção e limpá-la.

   c. Editar a **[mariadb]** secção.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
1. Abra as portas necessárias na firewall utilizando FirewallD no CentOS 7.

   * MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Recarregar o firewall: `firewall-cmd --reload`

1. Otimize o sistema para o desempenho. Para obter mais informações, consulte [estratégia de otimização do desempenho](optimize-mysql.md).

   a. Edite o ficheiro de configuração do MySQL novamente.

            vi /etc/my.cnf.d/server.cnf
   b. Editar a **[mariadb]** secção e anexe o seguinte conteúdo:

   > [!NOTE]
   > Recomendamos que innodb\_buffer\_pool_size é 70 por cento da memória da VM. Neste exemplo, foi definida em 2.45 GB para o meio de VM do Azure com 3,5 GB de RAM.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
1. Parar o MySQL e desativar o serviço de MySQL seja executado na inicialização evitar a interrupção do cluster quando adicionar um nó para desaprovisionar a máquina.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
1. Capture a VM através do portal. (Atualmente, [emitir #1268 nas ferramentas da CLI do Azure](https://github.com/Azure/azure-xplat-cli/issues/1268) descreve o fato de que imagens capturadas pelas ferramentas de CLI do Azure não capturam os discos de dados anexados.)

    a. Encerre a máquina através do portal.

    b. Clique em **capturar** e especifique o nome da imagem como **mariadb-galera-image**. Forneça uma descrição e selecione "Posso ter executar waagent".
      
      ![Capturar a máquina virtual](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Criar o cluster
Crie três VMs com o modelo criado e, em seguida, configurar e iniciar o cluster.

1. Crie primeira CentOS 7 VM a partir da imagem de imagem de galera mariadb que criou, indicando as seguintes informações:

 - Nome da rede virtual: mariadbvnet
 - Sub-rede: mariadb
 - Tamanho da máquina: médio
 - Nome do serviço cloud: mariadbha (ou qualquer nome que desejar ser acedido através de mariadbha.cloudapp.net)
 - Nome da máquina: mariadb1
 - Nome de utilizador: azureuser
 - Acesso SSH: ativada
 - Transmitir o ficheiro. pem de certificado SSH e substituindo /path/to/key.pem com o caminho onde armazenou a chave SSH. pem gerado.

   > [!NOTE]
   > Os comandos seguintes são divididos em várias linhas por motivos de clareza, mas deve inserir cada um como uma linha.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
1. Crie duas máquinas virtuais mais ao ligá-las para o serviço de nuvem mariadbha. Altere o nome da VM e a porta SSH para uma porta única, não em conflito com outras VMs no mesmo serviço cloud.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  Para MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
1. Terá de obter o endereço IP interno de cada um dos três VMs para o passo seguinte:

    ![Obter o endereço IP](./media/mariadb-mysql-cluster/IP.png)
1. Utilize o SSH para iniciar sessão para as três VMs e edite o ficheiro de configuração em cada um deles.

        sudo vi /etc/my.cnf.d/server.cnf

    Anule os comentários **`wsrep_cluster_name`** e **`wsrep_cluster_address`** removendo o **#** no início da linha.
    Além disso, substitua **`<ServerIP>`** no **`wsrep_node_address`** e **`<NodeName>`** no **`wsrep_node_name`** com o IP da VM de endereços e dê um nome, respectivamente, e anule os comentários essas linhas também.
1. Início do cluster no MariaDB1 e deixar que ele seja executado na inicialização.

        sudo service mysql bootstrap
        chkconfig mysql on
1. Iniciar o MySQL no MariaDB2 e MariaDB3 e deixar que ele seja executado na inicialização.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>O cluster de balanceamento de carga
Quando tiver criado as VMs em cluster, adicionou-los para um conjunto de disponibilidade chamado clusteravset para garantir que colocam em diferentes domínios de falha e de atualização e que o Azure nunca faz manutenção em todas as máquinas de uma só vez. Esta configuração cumpre os requisitos para suportar o contrato de nível de serviço do Azure (SLA).

Utilize o Balanceador de carga do Azure para balancear pedidos entre os três nós.

Execute os seguintes comandos no seu computador com a CLI do Azure.

A estrutura de parâmetros de comando é: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

A CLI define o intervalo de sonda de Balanceador de carga para 15 segundos, que pode ser um pouco longos demais. Alterá-la no portal em **pontos de extremidade** para qualquer uma das VMs.

![Editar o ponto final](./media/mariadb-mysql-cluster/Endpoint.PNG)

Selecione **reconfigurar o conjunto com balanceamento de carga**.

![Reconfigurar o conjunto com balanceamento de carga](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Alteração **intervalo de sonda** como 5 segundos e guarde as alterações.

![Intervalo de sonda de alteração](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Validar o cluster
O trabalho pesado está feito. O cluster deve estar agora acessível em `mariadbha.cloudapp.net:3306`, que acessa os pedidos de Balanceador e a rota de carga entre as três VMs fluente e eficaz.

Utilize o seu cliente favorito do MySQL para ligar ou ligar a partir de uma das VMs para verificar se este cluster está a funcionar.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Em seguida, crie uma base de dados e preenchê-lo com alguns dados.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

A base de dados que criou retorna a tabela seguinte:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
Neste artigo, criou um MariaDB de três nós + Galera cluster de elevada disponibilidade no Azure virtual máquinas em execução CentOS 7. As VMs são carga balanceada com Balanceador de carga do Azure.

Talvez queira examinar [outra maneira de cluster MySQL no Linux](mysql-cluster.md) e as formas de [otimizar e testar o desempenho do MySQL em VMs do Linux do Azure](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating the template]:#creating-the-template
[Creating the cluster]:#creating-the-cluster
[Load balancing the cluster]:#load-balancing-the-cluster
[Validating the cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[criar uma chave SSH para autenticação]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
