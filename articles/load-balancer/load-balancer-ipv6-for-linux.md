---
title: Configurar DHCPv6 para VMs com Linux | Microsoft Docs
description: Como configurar DHCPv6 para VMs com Linux.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: "IPv6, o Balanceador de carga do azure, pilha dupla, ip público, ipv6 nativo, móveis, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b46c2107dcfda5f02407e08daf08bd42d722dfda
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="configure-dhcpv6-for-linux-vms"></a>Configurar DHCPv6 para VMs com Linux

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Algumas imagens de máquinas virtuais Linux no Azure Marketplace tem Dynamic Host Configuration Protocol versão 6 (DHCPv6) configurado por predefinição. Para suportar IPv6, tem de ser configurado DHCPv6 da distribuição de SO Linux que está a utilizar. As várias distribuições de Linux configurar DHCPv6 numa variedade de formas porque utilizam diferentes pacotes.

> [!NOTE]
> Imagens do SUSE Linux e CoreOS recentes no Azure Marketplace tem sido previamente configuradas com DHCPv6. Não existem alterações adicionais são necessárias quando utiliza estas imagens.

Este documento descreve como ativar DHCPv6 para que a máquina virtual do Linux obtém um endereço IPv6.

> [!WARNING]
> Ao incorretamente editar ficheiros de configuração de rede, que pode perder o acesso à rede para a VM. Recomendamos que teste as alterações de configuração nos sistemas de não produção. As instruções neste artigo foram testadas nas versões mais recentes das imagens de Linux no Azure Marketplace. Para obter instruções mais detalhadas, consulte a documentação para a sua própria versão do Linux.

## <a name="ubuntu"></a>Ubuntu

1. Editar o */etc/dhcp/dhclient6.conf* de ficheiros e adicione a seguinte linha:

        timeout 10;

2. Edite a configuração de rede para a interface eth0 com a seguinte configuração:

   * No **Ubuntu 12.04 e 14.04**, edite o */etc/network/interfaces.d/eth0.cfg* ficheiro. 
   * No **Ubuntu 16.04**, edite o */etc/network/interfaces.d/50-cloud-init.cfg* ficheiro.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Renove o endereço IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Editar o */etc/dhcp/dhclient6.conf* de ficheiros e adicione a seguinte linha:

        timeout 10;

2. Editar o */etc/network/interfaces* de ficheiros e adicione a seguinte configuração:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Renove o endereço IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS e Oracle Linux

1. Editar o */etc/sysconfig/network* de ficheiros e adicione o seguinte parâmetro:

        NETWORKING_IPV6=yes

2. Editar o */etc/sysconfig/network-scripts/ifcfg-eth0* de ficheiros e adicionar os dois parâmetros seguintes:

        IPV6INIT=yes
        DHCPV6C=yes

3. Renove o endereço IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 e openSUSE 13

Linux do recente SUSE Server Enterprise (SLES) e as imagens de openSUSE no Azure tem sido previamente configuradas com DHCPv6. Não existem alterações adicionais são necessárias quando utiliza estas imagens. Se tiver uma VM com base numa imagem SUSE anterior ou personalizada, efetue o seguinte:

1. Instalar o `dhcp-client` do pacote, se necessário:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Editar o */etc/sysconfig/network/ifcfg-eth0* de ficheiros e adicione o seguinte parâmetro:

        DHCLIENT6_MODE='managed'

3. Renove o endereço IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 e openSUSE bissexto

As imagens recentes SLES e openSUSE no Azure tem sido previamente configuradas com DHCPv6. Não existem alterações adicionais são necessárias quando utiliza estas imagens. Se tiver uma VM com base numa imagem SUSE anterior ou personalizada, efetue o seguinte:

1. Editar o */etc/sysconfig/network/ifcfg-eth0* de ficheiros e substitua o `#BOOTPROTO='dhcp4'` parâmetro com o seguinte valor:

        BOOTPROTO='dhcp'

2. Para o */etc/sysconfig/network/ifcfg-eth0* ficheiro, adicione o seguinte parâmetro:

        DHCLIENT6_MODE='managed'

3. Renove o endereço IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recentes CoreOS imagens no Azure tem sido previamente configuradas com DHCPv6. Não existem alterações adicionais são necessárias quando utiliza estas imagens. Se tiver uma VM com base numa imagem de CoreOS anterior ou personalizada, efetue o seguinte:

1. Editar o */etc/systemd/network/10_dhcp.network* ficheiro:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Renove o endereço IPv6:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
