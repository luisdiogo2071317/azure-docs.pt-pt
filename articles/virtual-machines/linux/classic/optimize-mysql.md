---
title: Otimizar o desempenho do MySQL no Linux | Documentos da Microsoft
description: Saiba como otimizar o MySQL em execução numa máquina virtual do Azure (VM) em execução no Linux.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 0ba85e82824bc257869d9801f342bd6dbb0402d2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247454"
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Otimizar o desempenho do MySQL em VMs do Linux do Azure
Há muitos fatores que afetam o desempenho do MySQL no Azure, tanto na seleção de virtual hardware e a configuração de software. Este artigo se concentra na otimização de desempenho por meio de armazenamento, sistema e configurações de base de dados.

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [do Azure Resource Manager](../../../resource-manager-deployment-model.md) e clássico. Este artigo cobre a utilização do modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para obter informações sobre otimizações de VM do Linux com o modelo do Resource Manager, consulte [otimizar a sua VM do Linux no Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Utilize o RAID, numa máquina virtual do Azure
O armazenamento é o fator principal que afeta o desempenho da base de dados em ambientes de nuvem. Em comparação com um disco único, RAID pode fornecer um acesso mais rápido através de simultaneidade. Para obter mais informações, consulte [níveis RAID padrão](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Débito de disco e/s e tempo de resposta de e/s no Azure podem ser melhoradas através de RAID. Nossos testes de laboratório mostram que o débito de e/s de disco pode ser dobrado e tempo de resposta de e/s pode ser reduzido pela metade em média quando o número de discos RAID for dobrado (de duas a quatro, quatro para oito, etc.). Ver [apêndice A](#AppendixA) para obter detalhes.  

Além de e/s de disco, o desempenho do MySQL melhora quando aumenta o nível RAID.  Ver [apêndice B](#AppendixB) para obter detalhes.  

Talvez também queira considerar o tamanho do segmento. Em geral, quando tem um tamanho de bloco maior, obtém mais baixa sobrecarga, especialmente para escritas grandes. No entanto, quando o tamanho do segmento é demasiado grande, pode adicionar uma sobrecarga adicional que o impede de tirar partido do RAID. O tamanho atual do padrão é 512 KB, o que é comprovadamente ideal para ambientes de produção mais gerais. Ver [apêndice C](#AppendixC) para obter detalhes.   

Existem limites sobre quantos discos pode adicionar para tipos de máquina virtual diferente. Estes limites são detalhados no [tamanhos do serviço Máquina Virtual e na cloud do Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx). Precisará quatro discos de dados anexados a seguir o exemplo RAID neste artigo, embora pode optar por configurar o RAID com menos de discos.  

Este artigo pressupõe que já criou uma máquina virtual Linux e ter o MYSQL instalado e configurado. Para obter mais informações sobre como começar, veja como instalar o MySQL no Azure.  

### <a name="set-up-raid-on-azure"></a>Configurar o RAID no Azure
Os passos seguintes mostram como criar RAID no Azure com o portal do Azure. Pode também configurar o RAID com scripts do PowerShell do Windows.
Neste exemplo, vamos configurar o RAID 0 com quatro discos.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Adicionar um disco de dados à sua máquina virtual
No portal do Azure, aceda ao dashboard e selecione a máquina virtual ao qual pretende adicionar um disco de dados. Neste exemplo, a máquina virtual é mysqlnode1.  

<!--![Virtual machines][1]-->

Clique em **discos** e, em seguida, clique em **anexar novo**.

![Máquinas virtuais adicionar disco](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Crie um novo disco de 500 GB. Certifique-se de que **preferência de Cache do anfitrião** está definida como **nenhum**.  Quando tiver terminado, clique em **OK**.

![Anexar disco vazio](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Esta ação adiciona um disco vazio à sua máquina virtual. Repita este passo três vezes mais para que tenha quatro discos de dados para o RAID.  

Pode ver as unidades adicionadas na máquina virtual ao consultar o registo de mensagens de kernel. Por exemplo, para ver isso no Ubuntu, utilize o seguinte comando:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>Criar RAID com os discos adicionais
Os passos seguintes descrevem como [configurar o RAID de software no Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Se estiver a utilizar o sistema de ficheiros XFS, execute os passos seguintes depois de ter criado o RAID.
>
>

Para instalar XFS no Debian, Ubuntu ou e menta de Linux, utilize o seguinte comando:  

    apt-get -y install xfsprogs  

Para instalar XFS em RHEL, CentOS ou Fedora, utilize o seguinte comando:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Configurar um novo caminho de armazenamento
Utilize o seguinte comando para configurar um novo caminho de armazenamento:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>Copiar os dados originais para o novo caminho de armazenamento
Utilize o seguinte comando para copiar dados para o novo caminho de armazenamento:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Modificar permissões para que possa acessar MySQL (leitura e escrita) o disco de dados
Utilize o comando seguinte para modificar as permissões:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Ajustar o algoritmo de agendamento de e/s de disco
Linux implementa quatro tipos de algoritmos de agendamento de e/s:  

* Algoritmo NOOP (não operação)
* Algoritmo de dentro do prazo (prazo)
* Algoritmo de colocação em fila completamente justo (CFQ)
* Algoritmo de período de orçamento (Anticipatory)  

Pode selecionar diferentes Agendadores de e/s em diferentes cenários para otimizar o desempenho. Num ambiente de acesso totalmente aleatório, não existe uma diferença significativa entre os algoritmos de CFQ e dentro do prazo para o desempenho. Recomendamos que defina o ambiente de base de dados MySQL para o prazo para a estabilidade. Se houver muita e/s sequenciais, CFQ pode reduzir o desempenho de e/s de disco.   

Para SSD e outros equipamentos, NOOP ou prazo pode alcançar um desempenho melhor do que o agendador padrão.   

Antes do kernel 2.5, o algoritmo de agendamento para a e/s é dentro do prazo. CFQ começando com o kernel 2.6.18, tornou-se o algoritmo de agendamento para a e/s.  Pode especificar esta definição no momento da inicialização de kernel ou dinamicamente modificar esta definição quando o sistema está em execução.  

O exemplo seguinte demonstra como verificar e definir o agendador padrão para o algoritmo NOOP da família de distribuição Debian.  

### <a name="view-the-current-io-scheduler"></a>Ver o agendador de e/s atual
Para ver o scheduler, execute o seguinte comando:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Verá a seguinte saída, que indica o programador atual:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>Alterar o dispositivo atual (/ desenvolvimento/sda) do algoritmo de agendamento de e/s
Execute os seguintes comandos para alterar o dispositivo atual:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Não é útil definir este para /dev/sda sozinho. Tem de ser definido em todos os discos de dados onde reside a base de dados.  
>
>

Deverá ver o resultado seguinte, que indica que esse grub.cfg foi reconstruída com êxito e que o agendador padrão foi atualizado para NOOP:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Para a família de distribuição do Red Hat, precisa apenas o seguinte comando:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Configurar definições de operações de arquivo do sistema
Uma melhor prática é desativar o *atime* funcionalidade de registo no sistema de ficheiros. Atime é a hora do último acesso de arquivo. Sempre que um ficheiro for acessado, o sistema de ficheiros regista o carimbo de hora no registo. No entanto, essas informações raramente são usadas. Pode desativá-lo, se não precisa dela, que irá reduzir o tempo de acesso total do disco.  

Para desativar o registo de atime, terá de modificar fstab de /etc/ de ficheiro de configuração de sistema de ficheiros e adicionar as **noatime** opção.  

Por exemplo, edite o ficheiro de /etc/fstab. de vim, adicionando o noatime conforme mostrado no exemplo a seguir:  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Em seguida, voltar a montar o sistema de ficheiros com o seguinte comando:  

    mount -o remount /RAID0

Teste o resultado modificado. Ao modificar o ficheiro de teste, o tempo de acesso não é atualizado. Os exemplos seguintes mostram o código de aparência antes e após a modificação.

Antes:        

![Antes da modificação de acesso de código][5]

Depois:

![Após a modificação de acesso de código][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Aumentar o número máximo de identificadores do sistema para a simultaneidade elevada
A MySQL é um banco de dados de alta simultaneidade. O número predefinido de identificadores em simultâneo é 1024 para Linux, que nem sempre é suficiente. Utilize os seguintes passos para aumentar os identificadores em simultâneo máximos do sistema para oferecer suporte a alta simultaneidade do MySQL.

### <a name="modify-the-limitsconf-file"></a>Modifique o arquivo limits.conf.
Para aumentar os identificadores de em simultâneo máximos permitidos, adicione as seguintes linhas de quatro no ficheiro /etc/security/limits.conf. Observe que a 65536 é o número máximo que o sistema consegue suportar.   

    * nofile de forma recuperável 65536
    * disco rígido nofile 65536
    * nproc de forma recuperável 65536
    * disco rígido nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Atualize o sistema para os novos limites
Para atualizar o sistema, execute os seguintes comandos:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Certifique-se de que os limites são atualizados no momento da inicialização
Coloca os seguintes comandos de inicialização no arquivo /etc/rc.local, portanto, a mesma terá efeito no momento da inicialização.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>Otimização de base de dados MySQL
Para configurar o MySQL no Azure, pode utilizar a mesma estratégia de ajuste de desempenho que utilizar numa máquina no local.  

As regras de otimização de e/s principais são:   

* Aumente o tamanho da cache.
* Reduza o tempo de resposta de e/s.  

Para otimizar as definições do servidor MySQL, pode atualizar o arquivo de cnf, que é o ficheiro de configuração padrão para computadores cliente e servidor.  

Os seguintes itens de configuração são os principais fatores que afetam o desempenho do MySQL:  

* **innodb_buffer_pool_size**: O conjunto de memória intermédia contém dados em buffer e o índice. Isto é geralmente definido para 70 por cento da memória física.
* **innodb_log_file_size**: Este é o tamanho do log de retrabalho. Utilizar registos de Refazer para garantir que operações de escrita são rápidos, fiáveis e recuperáveis após uma pane. Isso é definido para 512 MB, irá dar-lhe bastante espaço para operações de escrita de registo.
* **max_connections**: por vezes, aplicativos não fechar conexões corretamente. Um valor maior fornecerá o servidor mais tempo para reciclar ligações inativo. O número máximo de ligações é de 10 000, mas o máximo recomendado é de 5000.
* **Innodb_file_per_table**: esta definição ativa ou desativa a capacidade de InnoDB para armazenar tabelas em arquivos separados. Ative a opção para garantir que várias operações de administração avançadas podem ser aplicadas de forma eficiente. Do ponto de vista do desempenho, ele pode acelerar a transmissão de espaço da tabela e otimizar o desempenho de gestão debris. A definição recomendada para esta opção está ON.</br></br>
Do MySQL 5.6, a predefinição é Diante, portanto, é necessária nenhuma ação. Para versões anteriores, a predefinição está OFF. A definição deve ser alterada antes dos dados são carregados, uma vez que são afetadas apenas tabelas recém-criado.
* **innodb_flush_log_at_trx_commit**: O valor predefinido é 1, com o âmbito definido como 0 ~ 2. O valor predefinido é a opção mais adequada para autónomo MySQL DB. A definição de 2 permite que a maioria dos integridade de dados e é adequada para mestre no Cluster do MySQL. A definição de 0 permite que a perda de dados, que pode afetar a confiabilidade (em alguns casos com melhor desempenho) e é adequado para subordinado no Cluster do MySQL.
* **Innodb_log_buffer_size**: O buffer de log permite que as transações sejam executados sem ter de liberar o registo para o disco antes de confirmar as transações. No entanto, se houver o campo de texto ou de objeto binário grande, a cache será consumida rapidamente e e/s de disco frequentes será acionada. É melhor aumentar o tamanho do buffer se Innodb_log_waits Estado variável não for 0.
* **query_cache_size**: A melhor opção é desabilitá-lo desde o início. Defina query_cache_size como 0 (esta é a predefinição no MySQL 5.6) e usar outros métodos para acelerar as consultas.  

Ver [apêndice D](#AppendixD) para obter uma comparação de desempenho antes e depois a otimização.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Ativar o registo de consulta lenta MySQL para analisar o afunilamento do desempenho
O log de consulta lenta MySQL pode ajudá-lo a identificar as consultas lentas para MySQL. Depois de ativar o registo de consulta lenta MySQL, pode utilizar ferramentas do MySQL, como **mysqldumpslow** para identificar o afunilamento do desempenho.  

Por predefinição, esta não está ativada. Ativar o registo de consulta lenta, poderá consumir alguns recursos de CPU. Recomendamos que ative esta temporariamente para resolução de problemas de afunilamentos de desempenho. Para ativar o registo de consulta lenta:

1. Modifique o ficheiro de cnf adicionando as seguintes linhas ao final:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Reinicie o servidor MySQL.

        service  mysql  restart

3. Verifique se a definição é entrarem em vigor, utilizando o **mostrar** comando.

![Diante de lento--registo de consultas][7]   

![Resultados de lento--registo de consultas][8]

Neste exemplo, pode ver que a funcionalidade de consulta lenta foi ativada. Em seguida, pode utilizar o **mysqldumpslow** ferramenta para determinar os afunilamentos de desempenho e otimizar o desempenho, por exemplo, adicionar índices.

## <a name="appendices"></a>Apêndices
Seguem-se os dados de teste de desempenho de exemplo produzidos num ambiente de laboratório de destino. Eles fornecem em segundo plano geral sobre a tendência de dados de desempenho com diferente abordagens de otimização do desempenho. Os resultados podem variar em diferentes versões de produto ou de ambiente.

### <a name="AppendixA"></a>Apêndice A  
**Desempenho de disco (IOPS), com diferentes níveis RAID**

![IOPS de disco com diferentes níveis RAID][9]

**Comandos de teste**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> A carga de trabalho de teste usa 64 threads, tentando alcançar o limite superior de RAID.
>
>

### <a name="AppendixB"></a>Apêndice B  
**Comparação de desempenho (débito) do MySQL com diferentes níveis RAID**   
(Sistema de ficheiros XFS)

![Comparação de desempenho do MySQL com diferentes níveis RAID][10]  
![Comparação de desempenho do MySQL com diferentes níveis RAID][11]

**Comandos de teste**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Comparação de desempenho (OLTP) de MySQL com diferentes níveis RAID**  
![Comparação de desempenho (OLTP) de MySQL com diferentes níveis RAID][12]

**Comandos de teste**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Apêndice C   
**Comparação de desempenho (IOPS) de disco para tamanhos de bloco diferentes**  
(Sistema de ficheiros XFS)

![][13]

**Comandos de teste**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Os tamanhos de ficheiro utilizados para este tipo de teste são 30 GB e 1 GB, respetivamente, com o RAID 0 (4 discos) XFS sistema de ficheiros.

### <a name="AppendixD"></a>Apêndice D  
**Comparação de desempenho (débito) do MySQL antes e após a otimização**  
(Sistema de ficheiros XFS)

![Comparação de desempenho (débito) do MySQL antes e após a otimização][14]

**Comandos de teste**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**A definição de configuração para o padrão e a otimização é o seguinte:**

| Parâmetros | Predefinição | Otimização |
| --- | --- | --- |
| **innodb_buffer_pool_size** |Nenhuma |7 GB |
| **innodb_log_file_size** |5 MB |512MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Para mais detalhadas [parâmetros de configuração de otimização](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), consulte a [instruções oficiais do MySQL](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Ambiente de teste**  

| Hardware | Detalhes |
| --- | --- |
| CPU |AMD Opteron processador 4171 HE / 4 núcleos |
| Memória |14 GB |
| Disco |10 GB/disco |
| SO |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

