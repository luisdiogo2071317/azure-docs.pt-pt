---
title: Utilizar o DNS do Azure para domínios privados | Microsoft Docs
description: Uma descrição geral do DNS privada que aloja o serviço no Microsoft Azure.
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
ms.openlocfilehash: 0ee3b18b7f874c4f6b7b2c9c559aa7e393ad7d8d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700609"
---
# <a name="use-azure-dns-for-private-domains"></a>Utilizar o DNS do Azure para domínios privados
O sistema de nomes de domínio ou DNS, é responsável por traduzir (ou resolver) um nome de serviço para o endereço IP. Um serviço de alojamento para domínios DNS, o DNS do Azure fornece a resolução do nome através da infraestrutura do Microsoft Azure. Para além de suporte para domínios DNS de acesso à internet, DNS do Azure agora também suporta privados domínios DNS como uma funcionalidade de pré-visualização. 
 
O DNS do Azure fornece um serviço DNS fiável e seguro para gerir e resolver os nomes de domínio numa rede virtual sem a necessidade de adicionar uma solução DNS personalizada. Ao utilizar privadas zonas DNS, pode utilizar os seus próprios nomes de domínio personalizado em vez dos nomes fornecidos pelo Azure atualmente disponíveis. Utilizando nomes de domínio personalizado ajuda-o a adaptarem-se na arquitetura de rede virtual para suit melhor as necessidades da sua organização. Fornece resolução de nomes de máquinas virtuais (VMs) dentro de uma rede virtual e entre redes virtuais. Além disso, pode configurar os nomes de zonas com uma vista de divisão horizon, que permite aos privado e uma zona DNS pública partilhar o mesmo nome.

![Descrição geral do DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Benefícios

O DNS do Azure fornece as seguintes vantagens:

* **Remove a necessidade de soluções DNS personalizadas**. Anteriormente, muitos clientes criar soluções personalizadas de DNS para gerir zonas DNS na sua rede virtual. Agora pode efetuar a gestão de zona DNS ao utilizar a infraestrutura do Azure nativa, o que remove o fardo de criar e gerir soluções DNS personalizadas.

* **Utilizar todos os tipos de registos DNS comuns**. O DNS do Azure suporta registos A, AAAA, CNAME, MX, NS, PTR, SOA, SRV e TXT.

* **Gestão de registo de nome de anfitrião automática**. Juntamente com os registos DNS personalizados de alojamento, o Azure mantém automaticamente os registos de nome de anfitrião para as VMs nas redes virtuais especificadas. Neste cenário, pode otimizar os nomes de domínio que sem necessitar de utilizar para criar soluções personalizadas de DNS ou modificação das aplicações.

* **Resolução de nome de anfitrião entre redes virtuais**. Ao contrário dos nomes de anfitrião fornecidos pelo Azure, privadas zonas DNS podem ser partilhadas entre redes virtuais. Esta capacidade simplifica a cenários de rede em vários locais e de deteção do serviço, como o peering de rede virtual.

* **Experiência de utilizador e de ferramentas familiares**. Para reduzir a curva de aprendizagem, esta nova oferta utiliza bem estabelecidas ferramentas de DNS do Azure (PowerShell, os modelos Azure Resource Manager e a API REST).

* **Suporte de DNS dividido horizon**. Com o DNS do Azure, pode criar zonas com o mesmo nome que resolve para respostas diferentes de dentro de uma rede virtual e a internet pública. Um cenário típico no DNS de divisão horizon consiste em fornecer uma versão dedicada de um serviço para utilização no interior da rede virtual.

* **Disponível em todas as regiões do Azure**. A funcionalidade de privada zonas DNS do Azure está disponível em todas as regiões do Azure na nuvem pública do Azure. 


## <a name="capabilities"></a>Capacidades

O DNS do Azure fornece as seguintes capacidades:
 
* **O registo automático de máquinas virtuais de uma única rede virtual que está ligada a uma zona privada como uma rede virtual do registo**. As máquinas virtuais estão a registado (adicionada) para a zona privada como registos a apontar para as respetivas IPs privados. Quando uma máquina virtual no registo de uma rede virtual é eliminada, Azure automaticamente também remove o DNS correspondente registos da zona privada ligada. 

  > [!NOTE]
  > Por predefinição, redes virtuais do registo também a agir como redes virtuais de resolução, na medida em que a resolução de DNS em relação a zona funciona a partir de qualquer uma das máquinas virtuais dentro da rede virtual do registo. 

* **Resolução direta de DNS é suportada entre redes virtuais que estejam ligadas à zona privada como redes virtuais resolução**. Para resolução de DNS de rede entre virtual, não há nenhum dependência explícita, de modo a que as redes virtuais em modo de peering entre si. No entanto, os clientes querer elemento redes virtuais para outros cenários (por exemplo, o tráfego HTTP).

* **Pesquisa inversa de DNS é suportada no âmbito de rede virtual**. Pesquisa inversa de DNS para um IP privado dentro da rede virtual atribuída a uma zona privada irá devolver o FQDN que inclui o nome do anfitrião/registo, bem como o nome da zona como o sufixo. 


## <a name="limitations"></a>Limitações

O DNS do Azure está sujeita às seguintes limitações:

* Rede virtual do registo de uma só é permitida por zona privada.
* Até 10 resolução, redes virtuais são permitidas por zona privada.
* Uma rede virtual específica pode ser ligada a apenas uma zona privada como uma rede virtual do registo.
* Uma rede virtual específica pode ser associada a até 10 zonas privadas como uma rede virtual de resolução.
* Se não for especificada uma rede virtual do registo, os registos DNS para as VMs de rede virtual que estão registados para a zona privada não são visíveis ou recuperável do Azure Powershell e APIs de CLI do Azure, mas os registos VM, de facto, estão registados e será Resolva com êxito.
* Inversa de DNS de funciona apenas para o espaço IP privados na rede virtual do registo.
* Inversa de DNS para um IP privado que não está registado na zona privada (por exemplo, um IP privado para uma máquina virtual numa rede virtual que está ligada como uma rede virtual resolução privado zona) devolve *internal.cloudapp.net* como o sufixo DNS. No entanto, não é resolvível este sufixo. 
* A rede virtual tem de estar em branco (ou seja, não existem registos VM existem) quando-inicialmente (ou seja, pela primeira vez) ligações a uma zona privada como um registo ou da resolução de rede virtual. No entanto, a rede virtual pode, em seguida, não pode estar vazio para ligar futuras como um registo ou da resolução de rede virtual, para outras zonas privadas. 
* Neste momento, o reencaminhamento condicional não é suportado (por exemplo, para ativar a resolução entre redes do Azure e OnPrem). Para obter informações sobre como os clientes podem tenha em consideração este cenário através de outros mecanismos, consulte [a resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Para perguntas e respostas sobre privadas zonas no DNS do Azure comuns, incluindo comportamento específico de resolução e registo DNS pode esperar para determinados tipos de operações, consulte [FAQ](./dns-faq.md#private-dns).  


## <a name="pricing"></a>Preços

A funcionalidade de zonas DNS privada é gratuitamente durante a pré-visualização pública. Durante a disponibilidade geral, a funcionalidade oferecem um baseada na utilização modelo de preços semelhante de DNS do Azure existente da oferta. 


## <a name="next-steps"></a>Passos Seguintes

Saiba como criar uma zona privada no DNS do Azure utilizando [Azure PowerShell](./private-dns-getstarted-powershell.md) ou [CLI do Azure](./private-dns-getstarted-cli.md).

Ler sobre algumas comuns [cenários de zona privada](./private-dns-scenarios.md) que podem ser realizados com privada zonas no DNS do Azure.

Para perguntas e respostas sobre privadas zonas no DNS do Azure comuns, incluindo um comportamento específico pode esperar para determinados tipos de operações, consulte [FAQ](./dns-faq.md#private-dns). 

Saiba mais sobre registos e zonas DNS, visitando [DNS zonas e as regista descrição geral](dns-zones-records.md).

Saiba mais sobre algumas das outras principais [capacidades de rede](../networking/networking-overview.md) do Azure. 

