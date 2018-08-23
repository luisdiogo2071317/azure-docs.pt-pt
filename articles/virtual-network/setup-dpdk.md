---
title: DPDK numa VM Linux do Azure | Documentos da Microsoft
description: Saiba como configurar DPDK numa máquina virtual Linux.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: 205a1e399eadd268ffaa390a7ebb4397fda9feff
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444658"
---
# <a name="setup-dpdk-in-a-linux-virtual-machine"></a>Configuração DPDK numa máquina virtual do Linux

Dados plano Development Kit (DPDK) no Azure oferece uma arquitetura de processamento de pacotes de espaço num mais rápida do utilizador para aplicações intensivas em termos de desempenho que se ignorar a pilha de rede de kernel da máquina virtual.

O pacote típico processamento usando a pilha de rede de kernel é controlado por interrupção. Sempre que a interface de rede recebe pacotes de entrada, há uma interrupção de kernel para processar a mudança de pacote e o contexto do espaço de kernel para o espaço do usuário. DPDK elimina a alternância de contexto e a interrupção controlado por método em prol de uma implementação de espaço de utilizador com drivers de modo de consulta para processamento de pacotes rápida.

DPDK é composta por conjunto de bibliotecas de espaço de utilizador fornecer acesso a recursos, tais como o hardware, núcleos lógicos, gerenciamento de memória de nível inferior e consultar os drivers no modo para placas de interface de rede.

DPDK pode ser executado em máquinas virtuais do Azure, que suporta várias distribuições de sistema operativo. DPDK fornece uma diferenciação de chave de desempenho na função de rede motriz implementações de virtualização, na forma de aplicações virtuais de rede (NVA) como um virtual router, firewall, VPN, Balanceador de carga, núcleos de pacote evoluído e denial-of-service ( Aplicações de DDoS).

## <a name="benefit"></a>Vantagem

**Superior pacotes por segundo (PPS)**: ignorando o kernel e ter o controle de pacotes no espaço do usuário reduz a contagem de ciclo, eliminando a alternância de contexto e melhora a taxa de pacotes processados por segundo em máquinas de virtuais de Linux do Azure.


## <a name="supported-operating-systems"></a>Sistemas operativos suportados

São suportadas as seguintes distribuições da galeria do Azure:

| SO Linux     | Versão de kernel        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-Azure     |
| Ubuntu 18.04 | 4.15.0-1015-Azure     |
| SLES 15      | 4.12.14-5.5-Azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Suporte de kernel personalizado**

Consulte a [Patches para a criação de um kernel do Linux do Azure otimizados](https://github.com/microsoft/azure-linux-kernel) em qualquer versão de kernel de Linux não listada, ou para obter mais informações, contacte [ azuredpdk@microsoft.com ](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Suporte de região

Todas as regiões do Azure suportam DPDK.

## <a name="prerequisites"></a>Pré-requisitos

Funcionamento em rede acelerado tem de estar ativado na máquina virtual do Linux. A máquina virtual deve ter, pelo menos, duas interfaces de rede, com uma interface para a gestão. Saiba como [criar uma máquina virtual Linux com a aceleração de rede ativada](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>Instalar dependências DPDK

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>RHEL7.5/CentOS 7.5

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15"></a>SLES 15

**Kernel do Azure**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Padrão kernel**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="setup-virtual-machine-environment-once"></a>Configurar o ambiente de máquina virtual (uma vez)

1. [Transferir o mais recente DPDK](https://core.dpdk.org/download). Versão 18.02 ou superior é necessário para o Azure.
2. Primeiro, crie a configuração padrão com `make config T=x86_64-native-linuxapp-gcc`.
3. Ativar Mellanox PMDs a configuração gerada com `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
4. A compilação com `make`.
5. Instalar com `make install DESTDIR=<output folder>`.

# <a name="configure-runtime-environment"></a>Configurar o ambiente de tempo de execução

Execute os seguintes comandos uma vez, após reiniciar:

1. Hugepages

   * Configure hugepage ao executar o comando seguinte, uma vez para todas as numanodes:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   *  Criar um diretório para a montagem com `mkdir /mnt/huge`.
   *  Hugepages de montagem com `mount -t hugetlbfs nodev /mnt/huge`.
   *  Verificação hugepages estão reservados com `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Existe uma forma de modificar o ficheiro de grub para que as páginas enormes estão reservadas no arranque ao seguir a [instruções](http://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) para DPDK. É a instrução na parte inferior da página. Quando em execução numa máquina virtual Linux do Azure, modificar arquivos em /etc/config/grub.d em vez disso, para reservar hugepages entre reinícios.

2. Endereços MAC e IP: utilizar `ifconfig –a` para ver o endereço MAC e IP das interfaces de rede. O *VF* interface de rede e *NETVSC* interface de rede têm o mesmo endereço MAC, mas apenas o *NETVSC* interface de rede tem um endereço IP. Interfaces de VF estão em execução como interfaces de subordinado de NETVSC interfaces.

3. Endereços PCI

   * Descubra qual a utilizar para o endereço PCI *VF* com `ethtool -i <vf interface name>`.
   * Certifique-se de que testpmd acidentalmente não assuma o dispositivo de pci VF para *eth0*, se *eth0* tenha o accelerated networking ativada. Se aplicação DPDK acidentalmente está a ser utilizado sobre a interface de rede de gestão e provoca a perda da sua ligação de SSH, utilize a consola de série para interromper o aplicativo DPDK, ou para parar ou iniciar a máquina virtual.

4. Carga *ibuverbs* em cada reinício com `modprobe -a ib_uverbs`. Para apenas a 15 de SLES, também carregar *mlx4_ib* com `modprobe -a mlx4_ib`.

## <a name="failsafe-pmd"></a>PMD contra falhas

Tem de executar aplicativos de DPDK sobre a segurança contra falhas PMD exposto no Azure. Se a aplicação é executada diretamente através do PMD VF, não receberá **todos os** pacotes destinados à VM, uma vez que alguns pacotes aparecerá sobre a interface sintética. Executar através de segurança contra falhas que PMD garante que o aplicativo recebe todos os pacotes destinados a ele e também garante que o aplicativo continuará a ser executado no modo DPDK, mesmo que o VF é revogado quando o anfitrião está em manutenção. Para obter mais informações sobre segurança contra falhas PMD, consulte [biblioteca de drivers do modo de consulta isento de falhas](http://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Executar testpmd

Uso `sudo` antes do *testpmd* comando a ser executado no modo de raiz.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Básico: Verificação da funcionalidade de inicialização de adaptador de segurança contra falhas

1. Execute os seguintes comandos para iniciar um aplicativo de testpmd única porta:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Execute os seguintes comandos para iniciar um aplicativo de testpmd de porta dupla:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   Se executar com mais de 2 NICs, o `--vdev` argumento segue este padrão: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Depois de iniciar, executar `show port info all` para verificar as informações da porta. Deverá ver uma ou duas portas DPDK que são net_failsafe (não *net_mlx4*).
4.  Utilize `start <port> /stop <port>` para iniciar o tráfego.

Iniciar os comandos anteriores *testpmd* no modo interativo, que é recomendado, para experimentar alguns comandos testpmd.

### <a name="basic-single-sendersingle-receiver"></a>Básico: Recetor de remetente único ou um só

Os seguintes comandos periodicamente os pacotes por segundo estatísticas de impressão:

1. No lado do TX, E.u.a., execute o seguinte comando:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. No lado do RX, execute o seguinte comando:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Ao executar os comandos anteriores numa máquina virtual, altere *IP_SRC_ADDR* e *IP_DST_ADDR* no `app/test-pmd/txonly.c` para corresponder o endereço IP real das máquinas virtuais antes de compilar. Caso contrário, os pacotes são largados antes de atingir o recetor.

### <a name="advanced-single-sendersingle-forwarder"></a>Avançado: Reencaminhador de remetente único ou um só
Os seguintes comandos periodicamente os pacotes por segundo estatísticas de impressão:

1. No lado do TX, E.u.a., execute o seguinte comando:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. No lado do FWD, execute o seguinte comando:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Ao executar os comandos anteriores numa máquina virtual, altere *IP_SRC_ADDR* e *IP_DST_ADDR* no `app/test-pmd/txonly.c` para corresponder o endereço IP real das máquinas virtuais antes de compilar. Caso contrário, os pacotes são largados antes de atingir o reencaminhador. Não será possível ter uma terceira máquina receber tráfego reencaminhado, porque o *testpmd* reencaminhador não modifica os endereços de camada 3, a menos que faça algumas alterações de código.

## <a name="references"></a>Referências

* [Opções de EAL](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Comandos de Testpmd](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)
