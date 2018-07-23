---
title: FAQ sobre DNS do Azure | Documentos da Microsoft
description: Perguntas freqüentes sobre o DNS do Azure
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: victorh
ms.openlocfilehash: 747b2e2499a9bafcf7a7b03bc2ce144828c55c75
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172505"
---
# <a name="azure-dns-faq"></a>FAQ sobre DNS do Azure

## <a name="about-azure-dns"></a>Sobre o DNS do Azure

### <a name="what-is-azure-dns"></a>O que é o DNS do Azure?

O sistema de nomes de domínio ou DNS, é responsável pela tradução (ou resolver) um nome de Web site ou serviço para o endereço IP. O DNS do Azure é um serviço de alojamento dos domínios DNS que fornece resolução de nomes usando a infra-estrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.

Domínios DNS no DNS do Azure estão alojados numa rede global do Azure de servidores de nomes DNS. Esta opção utiliza Anycast de rede para que cada consulta DNS for atendida pelo servidor DNS disponível mais próximo. O DNS do Azure fornece um desempenho rápido e de elevada disponibilidade para o seu domínio.

O serviço de DNS do Azure baseia-se no Azure Resource Manager. Assim, se beneficia de funcionalidades do Gestor de recursos, tais como o controlo de acesso baseado em funções, registos de auditoria e bloqueios de recursos. Os domínios e registos podem ser geridos através do portal do Azure, cmdlets do PowerShell do Azure e a CLI do Azure de várias plataformas. Aplicações que requerem gestão automática de DNS podem integrar com o serviço através da REST API e SDKs.

### <a name="how-much-does-azure-dns-cost"></a>Quanto custa o DNS do Azure?

O modelo de faturação do DNS do Azure baseia-se no número de zonas DNS alojadas no DNS do Azure e o número de consultas DNS recebidas. Descontos são oferecidos com base na utilização.

Para obter mais informações, consulte a [DNS do Azure, página de preços](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Qual é o SLA do DNS do Azure?

O Azure garante que os pedidos DNS válidos receberão uma resposta de, pelo menos, um servidor de nome de DNS do Azure, pelo menos, 99,99% do tempo.

Para obter mais informações, consulte a [página do SLA de DNS do Azure](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>O que é uma "zona DNS"? É o mesmo que um domínio de DNS? 

Um domínio é um nome exclusivo no sistema de nome de domínio, por exemplo, "contoso.com".


Uma zona DNS é utilizada para alojar os registos DNS para um determinado domínio. Por exemplo, o domínio "contoso.com" pode conter vários registos DNS, como "mail.contoso.com" (para um servidor de email) e "www.contoso.com" (para um web site). Estes registos seriam alojados no DNS zona "contoso.com".

É um nome de domínio *apenas um nome*, enquanto que uma zona DNS é um recurso de dados que contém os registos DNS para um nome de domínio. O DNS do Azure permite-lhe alojar uma zona DNS e gerir os registos de DNS para um domínio no Azure. Ele também fornece servidores de nomes DNS para responder a consultas DNS da Internet.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Necessito de comprar um nome de domínio de DNS para utilizar o DNS do Azure? 

Não necessariamente.

Não necessita de comprar um domínio para alojar uma zona DNS no DNS do Azure. Pode criar uma zona DNS a qualquer momento sem ter um nome de domínio. Consultas DNS para esta zona resolverá apenas se eles são direcionados para os servidores de nomes de DNS do Azure atribuídos à zona.

Tem de comprar o nome de domínio, se pretender ligar a zona DNS para a hierarquia DNS global – este tipo de ligação permite que as consultas DNS de qualquer lugar do mundo para localizar a sua zona DNS e obtenham a resposta com os registos DNS.

## <a name="azure-dns-features"></a>Recursos DNS do Azure

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>O DNS do Azure suporta a ativação pós-falha de encaminhamento ou ponto final de tráfego baseado em DNS?

Ativação pós-falha de encaminhamento e o ponto final de tráfego baseado em DNS são fornecidos pelo Gestor de tráfego do Azure. O Gestor de tráfego do Azure é um serviço do Azure separado que pode ser utilizado em conjunto com o DNS do Azure. Para obter mais informações, consulte a [descrição geral do Gestor de tráfego](../traffic-manager/traffic-manager-overview.md).

O DNS do Azure só suporta o alojamento dos domínios DNS 'static', em que cada consulta DNS para um determinado registo DNS recebe sempre a mesma resposta DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>O DNS do Azure suporta o registo de nome de domínio?

Não. O DNS do Azure não suporta atualmente a compra de nomes de domínio. Se quiser comprar domínios, terá de utilizar uma entidade de registo de nome de domínio de terceiros. A entidade de registo, normalmente, os custos de uma pequena taxa anual. Os domínios, em seguida, podem ser hospedados no DNS do Azure para a gestão de registos DNS. Ver [delegar um domínio ao DNS do Azure](dns-domain-delegation.md) para obter detalhes.

Compra de domínio é um recurso que está a ser controlado no registo de segurança do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>O DNS do Azure suporta DNSSEC?

Não. O DNS do Azure não suporta atualmente DNSSEC.

DNSSEC é uma funcionalidade que está a ser controlada no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>O DNS do Azure suporta transferências de zona (AXFR/IXFR)?

Não. Atualmente, o DNS do Azure não suporta transferências de zona. Zonas DNS podem ser [importados para o DNS do Azure com a CLI do Azure](dns-import-export.md). Registos DNS, em seguida, podem ser geridos através da [portal de gestão do DNS do Azure](dns-operations-recordsets-portal.md), nossa [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [cmdlets do PowerShell](dns-operations-recordsets.md), ou [Ferramenta CLI](dns-operations-recordsets-cli.md).

Transferência de zona é um recurso que está a ser controlado no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>O DNS do Azure suporta redirecionamentos de URL?

Não. Serviços de redirecionamento de URL não estão, na verdade, um serviço DNS - funcionam no nível HTTP, em vez de nível do DNS. Alguns fornecedores DNS para reunir um URL de redirecionamento do serviço como parte da sua oferta geral. Isto não é atualmente suportado pelo DNS do Azure.

Funcionalidade de redirecionamento de URLs está registada no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>O DNS do Azure suporta ASCII estendido codificação (8 bits) conjunto para o conjunto de registros TXT?

Sim. O DNS do Azure suporta o conjunto de codificação ASCII estendido para conjuntos de registros TXT, se utilizar a versão mais recente das APIs REST do Azure, SDKs, PowerShell e CLI (versões mais antigas que 2017-10-01 ou fazer SDK 2.1 suporta o conjunto de ASCII estendido). Por exemplo, se o utilizador fornece uma cadeia de caracteres como o valor para um registo TXT com o expandida caráter \128 ASCII (por exemplo: "abcd\128efgh"), DNS do Azure irá utilizar o valor de byte desse caractere (que é 128) na representação interna. No momento da resolução de DNS também este valor de byte será devolvido na resposta. Observe também que "abc" e "\097\098\099" são intercambiáveis como diz respeito a resolução. 

Seguimos [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) zona regras de escape de mestre de formato de ficheiro para registos TXT. Por exemplo, "\" agora escapes, na verdade, tudo por de RFC. Se especificar "A\B" como o valor de registo TXT, ele será representado e resolver como apenas "AB". Se realmente deseja o registo TXT ter "A\B" com uma resolução, terá de escape o "\" novamente, ou seja especificar como "A\\B". 

Tenha em atenção que este suporte não está atualmente disponível para registos TXT criados a partir do Portal do Azure. 

## <a name="using-azure-dns"></a>Utilizar o DNS do Azure

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Posso coalojar um domínio através do DNS do Azure e de outro fornecedor DNS?

Sim. O DNS do Azure suporta domínios co-hospedagem com outros serviços DNS.

Para tal, terá de modificar os registos NS para o domínio (qual controle quais fornecedores recebem DNS consulta para o domínio) para que apontem para os servidores de nomes de ambos os fornecedores. Pode modificar estes registos NS em três locais: no DNS do Azure, o outro fornecedor e na zona principal (normalmente configuradas através da entidade de registo de nome de domínio). Para obter mais informações sobre delegação de DNS, consulte [delegação de domínio DNS](dns-domain-delegation.md).

Além disso, terá de garantir que os registos DNS para o domínio estejam em sincronia entre os dois provedores DNS. Atualmente, o DNS do Azure não suporta transferências de zona DNS. Registos DNS têm de ser sincronizados com o [portal de gestão do DNS do Azure](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [cmdlets do PowerShell](dns-operations-recordsets.md), ou [Ferramenta CLI](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>É necessário delegar meu domínio para todos os quatro servidores de nomes de DNS do Azure?

Sim. O DNS do Azure atribui quatro servidores de nomes para cada zona DNS, de isolamento de falhas e uma maior resiliência. Para se qualificar para o SLA de DNS do Azure, terá de delegar o domínio para todos os quatro servidores de nomes.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Quais são os limites de utilização para o DNS do Azure?

Os limites predefinidos seguintes aplicam-se ao utilizar o DNS do Azure:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Posso mover uma zona DNS do Azure entre grupos de recursos ou entre subscrições?

Sim. Zonas DNS podem ser movidas entre grupos de recursos ou entre subscrições.

Não há nenhum impacto em consultas de DNS ao mover uma zona DNS. Os servidores de nomes atribuídos à zona permanecem os mesmos e consultas DNS são processadas como habitualmente durante todo.

Para obter mais informações e instruções sobre como mover zonas DNS, consulte [mover recursos para um novo grupo de recursos ou subscrição](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Quanto tempo demora para alterações DNS entrem em vigor?

Novas zonas DNS e registos DNS são geralmente se reflete nos servidores de nome DNS do Azure rapidamente, dentro de alguns segundos.

As alterações aos registos DNS existentes podem demorar um pouco mais, mas ainda devem ser refletidas nos servidores de nome DNS do Azure em 60 segundos. Neste caso, a cache de DNS fora do DNS do Azure (por clientes DNS e resoluções de recursivo DNS) também pode influenciar o tempo necessário para uma alteração DNS ser eficaz. Esta duração de cache pode ser controlada usando a propriedade do Time-To-Live (TTL) de cada conjunto de registos.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Como posso proteger meus zonas DNS eliminações acidentais?

O DNS do Azure é gerido com o Azure Resource Manager e benefícios do controle de acesso de recursos que o Azure Resource Manager fornece. Controlo de acesso baseado em funções pode ser utilizado para controlar quais os utilizadores tem de leitura ou acesso de escrita para zonas DNS e conjuntos de registos. Bloqueios de recursos podem ser aplicados para impedir a modificação acidental ou eliminação de zonas DNS e conjuntos de registos.

Para obter mais informações, consulte [Proteger zonas e registos DNS](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Como configurar registros SPF no DNS do Azure?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Servidores de nomes do Azure DNS resolver através de IPv6? 

Sim. Servidores de nomes de DNS do Azure são de pilha dupla (ter endereços IPv4 e IPv6). Para localizar o endereço IPv6 para os servidores de nomes do DNS do Azure atribuídos à sua zona DNS, pode usar uma ferramenta como a nslookup (por exemplo, `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Como posso definir no nome de domínio um internacional (IDN) no DNS do Azure?

Nomes de domínio internacionais (IDNs) funcionam por encoding cada nome DNS com '[punycode](https://en.wikipedia.org/wiki/Punycode)'. Consultas DNS são feitas com esses nomes com codificação punycode.

Pode configurar nomes de domínio internacionais (IDNs) no DNS do Azure, primeiro, convertendo o nome da zona ou nome do conjunto de registos para punycode. O DNS do Azure não suporta atualmente a conversão incorporada em punycode.

## <a name="private-dns"></a>DNS privado

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>O DNS do Azure suporta domínios "privados"?
Suporte para domínios de "privados" é implementado usando o recurso de zonas privadas.  Esta funcionalidade está atualmente disponível em pré-visualização pública.  Zonas privadas são geridas com as mesmas ferramentas como zonas de DNS do Azure para a internet, mas apenas é resolvidos a partir de suas redes virtuais especificados.  Consulte a [descrição geral](private-dns-overview.md) para obter detalhes.

Neste momento, as zonas privadas não são suportadas no portal do Azure. 

Para obter informações sobre outras opções de DNS internas no Azure, consulte [resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>O que é a diferença entre a rede virtual de registo e de rede virtual de resolução no contexto de zonas privadas? 
Pode ligar redes virtuais a uma zona DNS privada de duas formas – como uma rede virtual de registo ou como uma rede virtual de resolução. Em ambos os casos, as máquinas virtuais na rede virtual será capazes de resolver com êxito em relação a registos na zona privada. No entanto, se especificar uma rede virtual como uma rede virtual de registo, o Azure automaticamente registar registos DNS (registo dinâmico) para a zona para as máquinas virtuais na rede virtual. Além disso, quando uma máquina virtual no registo de uma rede virtual é excluída, o Azure também automaticamente remover o registo DNS correspondente da zona privada ligada. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>As zonas privadas do DNS do Azure irá funcionar entre regiões do Azure?
Sim. As zonas privadas é suportada para a resolução DNS entre redes virtuais em regiões do Azure, mesmo sem peering explicitamente as redes virtuais, desde que as redes virtuais são especificadas como redes virtuais de resolução para a zona privada. Os clientes podem precisar as redes virtuais para ser executado o peering para o tráfego TCP/HTTP para o fluxo de uma região para outra. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>É a conectividade à Internet de redes virtuais necessárias para as zonas privadas do?
Não. As zonas privadas do funcionam em conjunto com as redes virtuais e permitir que os clientes a gerir domínios para máquinas virtuais ou outros recursos dentro e entre redes virtuais. Sem conectividade internet é necessária para resolução de nomes. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>A mesma zona privada pode ser utilizada para várias redes virtuais para a resolução? 
Sim. Os clientes podem associar até 10 redes virtuais de resolução com uma única zona privada.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Pode uma rede virtual que pertence a uma subscrição diferente ser adicionada como uma rede virtual de resolução a uma zona privada? 
Sim, desde que o utilizador tem permissão de operação de escrita na tanto as redes virtuais, bem como a zona DNS privada. Tenha em atenção que a permissão de escrita pode ser atribuída a várias funções do RBAC. Por exemplo, a função RBAC de contribuinte de rede clássica tem permissões de escrita para redes virtuais. Para obter mais informações sobre funções RBAC, veja [controlo de acesso baseado em funções](../role-based-access-control/overview.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Os registos DNS da máquina de virtual automaticamente registado numa zona privada sejam eliminados automaticamente quando as máquinas virtuais são eliminadas pelo cliente?
Sim. Se eliminar uma máquina virtual dentro de uma rede virtual de registo, podemos irá eliminar automaticamente os registos DNS que foram registados para a zona devido a esta rede virtual de um registo a ser. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Um registo de máquina de virtual automaticamente registrados numa zona privada (a partir de uma rede virtual de registo) pode ser eliminado manualmente? 
Não. Neste momento, os registos DNS de máquina virtual que são automaticamente registrados numa zona privada de uma rede virtual de registo não são visíveis ou editável pelos clientes. No entanto, pode substituir (substituir) esses registros DNS registados automaticamente com um DNS criado manualmente registram na zona. Consulte as seguintes perguntas e respostas que soluciona isso.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>O que acontece quando tentar criar manualmente um novo registo DNS para uma zona privada que tem o mesmo nome do anfitrião como uma máquina virtual existente (automaticamente registada) numa rede virtual de registo? 
Se está tentando criar manualmente um novo registo DNS para uma zona privada que tem o mesmo nome do anfitrião como uma máquina virtual (automaticamente registada) existente numa rede virtual de registo, podemos permitirá que o novo registo DNS substituir automaticamente registado registo de máquina virtual. Além disso, se, em seguida, tentar eliminar este criados manualmente o registo DNS da zona, a eliminação será concluída com êxito e o registo automático irá ocorrer novamente (o registo DNS será recriado automaticamente na zona), desde que a máquina virtual ainda existe e tem um IP privado ligados ao mesmo. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>O que acontece quando podemos desassociar a uma rede virtual de registo de uma zona privada? Os registos de máquina de virtual registado automaticamente da rede virtual serão removidos da zona também?
Sim. Se desassociar uma rede virtual de registo (atualização a zona DNS para remover a rede virtual de registo associada) a partir de uma zona privada, o Azure remove quaisquer registos de máquina de virtual registado automaticamente da zona. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>O que acontece quando vamos eliminar um registo (ou resolução) da rede virtual que está ligada a uma zona privada? Temos de atualizar manualmente a zona privada para a rede virtual de rede virtual como um registo (ou resolução) de ligação de anulação da zona?
Sim. Quando eliminar uma rede virtual de registo (ou resolução) sem desassociá-lo a partir de uma privada zona em primeiro lugar, Azure permitirá que a operação de eliminação com êxito, mas a rede virtual não é automaticamente desassociada da sua zona privada se houver. Terá de desassociar manualmente a rede virtual a partir da zona privada. Por esse motivo, recomenda-se para desassocie primeiro a rede virtual da sua zona privada antes de o eliminar.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>Seria a resolução de DNS usando o portão FQDN (internal.cloudapp.net) continuarão a funcionar mesmo quando uma zona privada (por exemplo: contoso. local) está ligada a uma rede virtual? 
Sim. Funcionalidade zonas privadas não substitui as resoluções DNS padrão utilizam a zona de internal.cloudapp.net fornecida pelo Azure e é oferecida como uma capacidade adicional ou um aprimoramento. Para ambos os casos (se a entidade confiadora no internal.cloudapp.net fornecida pelo Azure ou em sua própria zona privada) recomenda-se para utilizar o FQDN da zona que pretende resolver contra. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>O sufixo DNS em máquinas virtuais numa rede virtual ligada deve ser alterado para que a zona privada? 
Não. Neste momento, o sufixo DNS nas máquinas virtuais na sua rede virtual ligado permanecerá como o sufixo predefinido de fornecida pelo Azure ("*. internal.cloudapp.net"). No entanto manualmente pode alterar este sufixo DNS nas suas máquinas virtuais para que a zona privada. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Existem limitações para zonas privadas do durante esta pré-visualização?
Sim. Durante a pré-visualização pública, existem as seguintes limitações:
* 1 registo de redes virtuais por zona privada
* Redes virtuais de resolução de 10 até por zona privada
* Uma determinada rede virtual só pode ser associada a uma zona privada como uma rede virtual de registo
* Uma determinada rede virtual pode ser associada a até 10 as zonas privadas do que uma rede virtual de resolução
* Se for especificada uma rede virtual de registo, os registos DNS para as VMs da rede virtual que estão registados para a zona privada não será visível ou recuperável do Powershell/CLI/APIs, mas os registos VM, de fato, estão registados e serão resolvido com êxito
* DNS inverso só irá funcionar para o espaço de IP privado na rede virtual de registo
* DNS inverso para um IP privado que não está registado na zona privada (por exemplo: IP privado para uma máquina virtual numa rede virtual que é ligada como a uma rede virtual de resolução para uma zona privada) irá devolver "internal.cloudapp.net" como o sufixo DNS, no entanto Este sufixo não será resolvido.   
* Rede virtual tem de estar em branco (ou seja, Não existem máquinas virtuais com uma NIC anexada) quando inicialmente (ou seja, pela primeira vez) de ligação a uma zona privada que o registo ou resolução de rede virtual. No entanto, a rede virtual, em seguida, pode estar vazio para a ligação futuras como um registo ou resolução de rede virtual para outras zonas privadas. 
* Neste momento, reencaminhamento condicional não é suportado, por exemplo para habilitar a resolução entre redes do Azure e o local. Para obter documentação sobre como os clientes podem obter este cenário através de outros mecanismos, consulte [resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Existem quotas ou limites de zonas ou registos para as zonas privadas do?
Não há nenhum limite separado no número de zonas permitidas por subscrição ou número de conjuntos de registos por zona, para as zonas privadas. Públicos e as zonas privadas do contam para os limites DNS globais conforme documentado [aqui](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Existe suporte de Portal para zonas privadas do?
As zonas privadas já criadas por meio de mecanismos de não-Portal (API/PowerShell/CLI/SDKs) serão visíveis no portal do Azure, mas os clientes serão não é possível criar novas zonas privadas ou gerir as associações com as redes virtuais. Além disso, para as redes virtuais associadas como registo de redes virtuais, os registos VM automaticamente registados não serão visíveis no Portal. 

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre o DNS do Azure](dns-overview.md)
<br>
[Saiba mais sobre como utilizar o DNS do Azure para domínios privados](private-dns-overview.md)
<br>
[Saiba mais sobre zonas e registos DNS](dns-zones-records.md)
<br>
[Introdução ao DNS do Azure](dns-getstarted-portal.md)

