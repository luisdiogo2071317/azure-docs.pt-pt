---
title: Utilizar o DNS do Azure para domínios privados | Microsoft Docs
description: Descrição geral do DNS privada que aloja o serviço no Microsoft Azure.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: 677fc66b66d6c17806a313f2fac3a15e8e1775ba
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="using-azure-dns-for-private-domains"></a>Utilizar o DNS do Azure para domínios privados
O sistema de nomes de domínio ou DNS, é responsável por traduzir (ou resolver) um nome de serviço para o endereço IP. O DNS do Azure é um serviço de alojamento de domínios DNS, fornecer a resolução do nome através da infraestrutura do Microsoft Azure.  Para além dos domínios DNS de acesso à internet, DNS do Azure também suporta agora privados domínios DNS como uma funcionalidade de pré-visualização.  
 
O DNS do Azure fornece um serviço DNS fiável e seguro para gerir e resolver os nomes de domínio numa rede virtual sem a necessidade de adicionar uma solução DNS personalizada. Zonas DNS privados permitem-lhe utilizar os seus próprios nomes de domínio personalizado em vez dos nomes fornecidos pelo Azure atualmente disponíveis.  Utilizando nomes de domínio personalizado ajuda-o a adaptarem-se na arquitetura de rede virtual para suit melhor as necessidades da sua organização. Proporciona uma resolução de nomes para VMs dentro de uma rede virtual e entre redes virtuais. Além disso, pode configurar os nomes de zonas com uma vista de divisão horizon - permitir uma privada e uma zona DNS pública partilhar o mesmo nome.

![Descrição geral do DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Benefícios

* **Remove a necessidade de soluções DNS personalizadas.** Anteriormente, muitos clientes criar soluções personalizadas de DNS para gerir zonas DNS na sua rede virtual.  Gestão de zona DNS pode agora ser efetuada utilizando a infraestrutura nativa do Azure, que elimina o fardo de criar e gerir soluções DNS personalizadas.

* **Utilize todos os tipos de registos DNS comuns.**  O DNS do Azure suporta registos A, AAAA, CNAME, MX, NS, PTR, SOA, SRV e TXT.

* **Gestão de registo de nome de anfitrião automática.** Juntamente com os registos DNS personalizados de alojamento, o Azure mantém automaticamente os registos de nome de anfitrião para as VMs nas redes virtuais especificadas.  Isto permite-lhe otimizar os nomes de domínio que sem necessitar de utilizar para criar soluções personalizadas de DNS ou modificar a aplicação.

* **Resolução de nome de anfitrião entre redes virtuais.** Ao contrário dos nomes de anfitrião fornecidos pelo Azure, privadas zonas DNS podem ser partilhadas entre redes virtuais.  Esta capacidade simplifica a deteção de rede em vários locais e serviço cenários, como o peering de rede virtual.

* **Ferramentas familiares e experiência de utilizador.** Para reduzir a curva de aprendizagem, esta nova oferta utiliza as ferramentas de DNS do Azure já bem estabelecidas (PowerShell, modelos do Resource Manager, REST API).

* **DNS dividido horizon suporta.** O DNS do Azure permite-lhe criar zonas com o mesmo nome que resolve para respostas diferentes de dentro de uma rede virtual e a Internet pública.  Um cenário típico no DNS de divisão horizon consiste em fornecer uma versão dedicada de um serviço para utilização no interior da rede virtual.

* **Disponível em todas as regiões do Azure.** Zonas de DNS privado do Azure está disponível em todas as regiões do Azure na nuvem pública do Azure. 


## <a name="capabilities"></a>Capacidades 
* O registo automático de máquinas virtuais de uma única rede virtual ligado a uma zona privada como uma rede virtual do registo. As máquinas virtuais serão registados (adicionada) para a zona privada como registos a apontar para as respetivas IPs privados. Além disso, quando uma máquina virtual no registo de uma rede virtual é eliminada, o Azure também automaticamente remover o registo DNS correspondente da zona privada ligada. Tenha em atenção de que redes virtuais do registo também por predefinição atuam como redes virtuais de resolução em que a resolução de DNS em relação a zona será trabalhar a partir de qualquer uma das máquinas virtuais dentro da rede virtual do registo. 
* Reencaminhar suportada nas redes virtuais que estejam ligadas à zona privada como redes virtuais de resolução de resolução DNS. Para a rede virtual entre resolução de DNS, está sem dependência explícita que as redes virtuais ser executado o peering entre si. No entanto, os clientes podem pretendem elemento redes virtuais para outros cenários (ex: tráfego HTTP).
* Pesquisa inversa de DNS suportada dentro do âmbito VNET. Pesquisa inversa de DNS para um IP privado dentro da rede virtual atribuída a uma zona privada irá devolver o FQDN que inclui o nome do anfitrião/registo, bem como o nome da zona como o sufixo. 


## <a name="limitations"></a>Limitações
* 1 rede virtual de registo por zona de privada
* Até 10 redes virtuais de resolução por zona de privada
* Uma determinada rede virtual só pode ser associada a uma zona de privada como uma rede virtual do registo
* Uma determinada rede virtual pode ser associada a até 10 privada zonas como uma rede virtual de resolução
* Se não for especificada uma rede virtual do registo, os registos DNS para as VMs de rede virtual que estão registados para a zona privada não serão visíveis ou recuperável do Powershell/CLI/APIs, mas os registos VM, de facto, estão registados e irão resolver com êxito
* DNS inversa só irá funcionar para o espaço de IP privado na rede virtual do registo
* Inversa de DNS para um IP privado que não está registado na zona privada (ex: IP privado para uma máquina virtual numa rede virtual que está ligada como uma rede virtual de resolução para uma zona privada) irá devolver "internal.cloudapp.net" como o sufixo DNS, no entanto este sufixo não será resolvível.   
* Rede virtual tem de estar em branco (revertidos Não existem registos VM) quando inicialmente (revertidos a primeira) de ligação a uma zona privada como o registo ou de resolução de rede virtual. No entanto, a rede virtual pode, em seguida, não pode estar vazio para ligar futuras como um registo ou da resolução de rede virtual, para outras zonas privadas. 
* Neste momento, reencaminhamento condicional não é suportado, por exemplo para ativar a resolução entre redes do Azure e OnPrem. Para obter documentação sobre como os clientes podem tenha em consideração este cenário através de outros mecanismos, consulte [resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

Também recomendamos que leia também no [FAQ](./dns-faq.md#private-dns) para algumas comuns perguntas e respostas no privada zonas no DNS do Azure, incluindo o comportamento específico de resolução e registo DNS que pode esperar para determinados tipos de operações. 


## <a name="pricing"></a>Preços

Zonas DNS privada é gratuitamente durante a pré-visualização pública. Durante a disponibilidade geral, esta funcionalidade irá utilizar um baseada na utilização modelo de preços semelhante para o DNS do Azure existente da oferta. 


## <a name="next-steps"></a>Passos Seguintes

Saiba como criar uma zona privada no DNS do Azure utilizando o [PowerShell](./private-dns-getstarted-powershell.md) ou [CLI](./private-dns-getstarted-cli.md).

Leia sobre alguns [Cenários de Zona Privada](./private-dns-scenarios.md) comuns que podem ser conseguidos com as Zonas Privadas no DNS do Azure.

Ler sobre o [FAQ](./dns-faq.md#private-dns) para algumas comuns perguntas e respostas no privada zonas no DNS do Azure, incluindo um comportamento específico que pode esperar para determinados tipos de operações. 

Saiba mais sobre registos e zonas DNS, visitando: [DNS zonas e as regista descrição geral](dns-zones-records.md).

Saiba mais sobre algumas das outras principais [capacidades de rede](../networking/networking-overview.md) do Azure.

