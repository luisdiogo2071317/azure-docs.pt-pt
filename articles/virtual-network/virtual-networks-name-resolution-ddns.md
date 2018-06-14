---
title: Através do DNS dinâmico para registar os nomes de anfitrião no Azure | Microsoft Docs
description: Saiba como configurar o DNS dinâmico para registar os nomes de anfitrião no seus próprios servidores DNS.
services: dns
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: subsarma
ms.openlocfilehash: bbbce45b7c321fd4934374c76f2a4421b125d46f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31600959"
---
# <a name="use-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Utilizar o DNS dinâmico para registar os nomes de anfitrião no servidor DNS

[O Azure oferece resolução de nomes](virtual-networks-name-resolution-for-vms-and-role-instances.md) para máquinas virtuais (VM) e instâncias de função. Quando a resolução do nome tem de exceder as capacidades fornecidas por predefinição do Azure DNS, pode fornecer os seus próprios servidores DNS. Utilizar os seus próprios servidores DNS dá-lhe a capacidade para personalizar a sua solução DNS para se adequarem às suas necessidades específicas. Por exemplo, precisa de aceder a recursos no local através do seu controlador de domínio do Active Directory.

Quando os servidores DNS personalizados estão alojados as VMs do Azure, pode reencaminhar consultas de nome de anfitrião para a mesma rede virtual no Azure, para resolver os nomes de anfitrião. Se não pretender utilizar esta opção, pode registar os nomes de anfitrião VM no seu servidor DNS com o DNS dinâmico (DDNS). Azure não tem as credenciais para criar diretamente os registos nos servidores DNS, pelo que disposições alternativas, muitas vezes, são necessários. Siga alguns cenários comuns, com alternativas:

## <a name="windows-clients"></a>Clientes Windows
A tentativa de clientes do Windows não-associados a um domínio protegidas DDNS atualizações quando estes arrancam ou quando muda do endereço IP. O nome DNS é o nome de anfitrião e o sufixo DNS primário. Azure deixa o sufixo DNS primário em branco, mas pode definir o sufixo na VM, através de [interface de utilizador](https://technet.microsoft.com/library/cc794784.aspx) ou [PowerShell](/powershell/module/dnsclient/set-dnsclient).

Os clientes do Windows associados a um domínio registar os respetivos endereços IP com o controlador de domínio utilizando o DDNS segura. O processo de associação de domínio define o sufixo DNS primário no cliente e cria e mantém a relação de confiança.

## <a name="linux-clients"></a>Clientes do Linux
Clientes Linux, geralmente, não registar-se com o servidor DNS no arranque, assumem que o servidor de DHCP não-lo. Servidores DHCP do Azure têm as credenciais para registar registos no seu servidor DNS. Pode utilizar uma ferramenta chamada `nsupdate`, que está incluído no pacote de enlace, para enviar DDNS atualizações. Porque o protocolo DDNS está padronizado, pode utilizar `nsupdate` , mesmo quando não utilizar enlace no servidor DNS.

Pode utilizar os hooks que são fornecidos pelo cliente de DHCP para criar e manter a entrada de nome de anfitrião no servidor DNS. Durante o ciclo de DHCP, o cliente executa os scripts no */etc/dhcp/dhclient-exit-hooks.d/*. Pode utilizar os hooks para registar o novo endereço IP com `nsupdate`. Por exemplo:

```bash
#!/bin/sh
requireddomain=mydomain.local

# only execute on the primary nic
if [ "$interface" != "eth0" ]
then
    return
fi

# When you have a new IP, perform nsupdate
if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
   [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
then
   host=`hostname`
   nsupdatecmds=/var/tmp/nsupdatecmds
     echo "update delete $host.$requireddomain a" > $nsupdatecmds
     echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
     echo "send" >> $nsupdatecmds

     nsupdate $nsupdatecmds
fi
```

Também pode utilizar o `nsupdate` de atualizações de comando para efetuar DDNS segura. Por exemplo, quando estiver a utilizar um servidor DNS de enlace, um par de chaves públicas-privadas é [gerado](http://linux.yyz.us/nsupdate/). O servidor DNS [configurado](http://linux.yyz.us/dns/ddns-server.html) com a parte pública da chave, por isso que as TI podem verificar a assinatura no pedido. Para fornecer o par de chaves para `nsupdate`, utilize o `-k` opção, para os DDNS pedido ser assinados de atualização.

Quando estiver a utilizar um servidor DNS do Windows, pode utilizar a autenticação Kerberos com o `-g` parâmetro `nsupdate`, mas não está disponível na versão Windows `nsupdate`. Para utilizar o Kerberos, utilize `kinit` ao carregar as credenciais. Por exemplo, pode carregar as credenciais de um [keytab ficheiro](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)), em seguida, `nsupdate -g` escolherá as credenciais, da cache.

Se for necessário, pode adicionar um sufixo de procura DNS para as suas VMs. O sufixo DNS especificado no */etc/resolv.conf* ficheiro. A maioria das Linux distros gerir automaticamente o conteúdo deste ficheiro, por isso, normalmente, não é possível editá-lo. No entanto, pode substituir o sufixo, utilizando o cliente DHCP `supersede` comando. Para substituir o sufixo, adicione a seguinte linha para o */etc/dhcp/dhclient.conf* ficheiro:

```
supersede domain-name <required-dns-suffix>;
```
