---
title: FAQ de DNS do Azure | Microsoft Docs
description: Perguntas mais frequentes sobre o DNS do Azure
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: f07f914ccf8ea6df216e3f571e38d7628b2d7fb6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-dns-faq"></a>Azure DNS FAQ

## <a name="about-azure-dns"></a>Sobre o DNS do Azure

### <a name="what-is-azure-dns"></a>O que é o DNS do Azure?

O sistema de nomes de domínio ou DNS, é responsável por traduzir (ou resolver) um nome de Web site ou serviço para o endereço IP. O DNS do Azure é um serviço de alojamento de domínios DNS, fornecer a resolução do nome utilizando a infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.

Domínios DNS no DNS do Azure estão alojados numa rede de global do Azure dos servidores de nome DNS. Esta opção utiliza Anycast de rede para que cada consulta DNS é respondida pelo servidor DNS disponível mais próximo. O DNS do Azure fornece desempenho rápido e elevada disponibilidade para o seu domínio.

O serviço DNS do Azure baseia-se no Azure Resource Manager. Como tal, beneficia das funcionalidades do Gestor de recursos, tais como o controlo de acesso baseado em funções, os registos de auditoria e bloqueio de recurso. Os domínios e registos podem ser geridos através do portal do Azure, os cmdlets do PowerShell do Azure e a CLI do Azure de várias plataformas. As aplicações que requerem gestão automática de DNS podem integrar com o serviço através da REST API e SDKs.

### <a name="how-much-does-azure-dns-cost"></a>Quanto custo DNS do Azure?

O modelo de faturação do DNS do Azure é com base no número de zonas DNS alojadas no DNS do Azure e o número de consultas DNS que recebem. Descontos são fornecidos com base na utilização.

Para obter mais informações, consulte o [DNS do Azure, página de preços](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Qual é o SLA do DNS do Azure?

Azure garante que os pedidos de DNS válidos irão receber uma resposta de, pelo menos, um servidor de nome de DNS do Azure, pelo menos, 99,99% de tempo.

Para obter mais informações, consulte o [página SLA de DNS do Azure](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>O que é uma 'zona DNS'? É o mesmo que um domínio de DNS? 

Um domínio é um nome exclusivo no sistema de nome de domínio, por exemplo, "contoso.com".

Uma zona DNS é utilizada para alojar os registos DNS para um determinado domínio. Por exemplo, o domínio "contoso.com" pode conter vários registos DNS, tais como "mail.contoso.com" (para um servidor de correio) e "www.contoso.com" (para um web site). Estes registos seriam alojados na zona DNS "contoso.com".

É um nome de domínio *apenas um nome*, enquanto que uma zona DNS é um recurso de dados que contém os registos DNS para um nome de domínio. O DNS do Azure permite-lhe alojar uma zona DNS e gerir os registos de DNS para um domínio no Azure. Também fornece servidores de nome DNS para responder às consultas DNS da Internet.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Necessito de comprar um nome de domínio de DNS para utilizar o DNS do Azure? 

Não necessariamente.

Não necessita de comprar um domínio para alojar uma zona DNS no DNS do Azure. Pode criar uma zona DNS a qualquer momento sem ter um nome de domínio. Consultas DNS para esta zona só irão resolver se este é direcionado para os servidores de nome de DNS do Azure atribuídos à zona.

Terá de adquirir o nome de domínio se de que pretende associar a sua zona DNS para a hierarquia DNS global – esta ligação permite consultas DNS a partir de qualquer lugar do mundo, para localizar a sua zona DNS e de ser respondidas com os registos DNS.

## <a name="azure-dns-features"></a>Funcionalidades DNS do Azure

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>DNS do Azure suporta a ativação de pós-falha de tráfego com base no DNS, encaminhamento ou o ponto final?

Ativação pós-falha de encaminhamento e o ponto final de tráfego com base no DNS são fornecidos pelo Gestor de tráfego do Azure. Traffic Manager do Azure é um serviço Azure separado que pode ser utilizado em conjunto com o DNS do Azure. Para obter mais informações, consulte o [descrição geral do Gestor de tráfego](../traffic-manager/traffic-manager-overview.md).

O DNS do Azure só suporta o alojamento de domínios DNS 'estáticos', em que cada consulta DNS para um determinado registo DNS recebe sempre a mesma resposta DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>DNS do Azure suporta o registo de nome de domínio?

Não. O DNS do Azure não suporta atualmente a compra de nomes de domínio. Se pretende adquirir domínios, terá de utilizar uma entidade de registo de nome de domínio de terceiros. A entidade de registo normalmente cobra uma pequena taxa anual. Os domínios, em seguida, podem ser alojados no DNS do Azure para a gestão de registos DNS. Consulte [delegar um domínio ao DNS do Azure](dns-domain-delegation.md) para obter mais detalhes.

Compra de domínio é uma funcionalidade que está a ser controlada no registo de segurança do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>DNS do Azure suporta DNSSEC?

Não. O DNS do Azure não suporta atualmente DNSSEC.

DNSSEC é uma funcionalidade que está a ser controlada no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>DNS do Azure suporta transferências de zona (transações AXFR/IXFR)?

Não. O DNS do Azure não suporta atualmente transferências de zona. Zonas DNS podem ser [importado para o DNS do Azure utilizando a CLI do Azure](dns-import-export.md). Registos DNS, em seguida, podem ser geridos através de [portal de gestão do DNS do Azure](dns-operations-recordsets-portal.md), nosso [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [cmdlets do PowerShell](dns-operations-recordsets.md), ou [Ferramenta CLI](dns-operations-recordsets-cli.md).

Transferência de zona é uma funcionalidade que está a ser controlada no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>DNS do Azure suporta URL redireciona?

Não. Serviços do URL de redirecionamento não são um serviço de DNS - funcionam ao nível do HTTP em vez do nível DNS. Alguns fornecedores DNS para agrupar um URL de redirecionamento serviço como parte da sua oferta geral. Isto não é atualmente suportado pelo DNS do Azure.

Funcionalidade redirecionar o URL está registada no registo de segurança de DNS do Azure. Pode utilizar o site de comentários para [registar o suporte para esta funcionalidade](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Utilizar o DNS do Azure

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Posso coalojar um domínio DNS do Azure e o outro fornecedor DNS a utilizar?

Sim. O DNS do Azure suporta domínios de alojamento conjunta com outros serviços DNS.

Para tal, tem de modificar os registos NS para o domínio (o controlo que Fornecedores recebem DNS consulta para o domínio) para que apontem para os servidores de nomes de ambos os fornecedores. Pode modificar estes registos NS em três locais: no DNS do Azure, no outro fornecedor e na zona principal (normalmente configuradas através de entidade de registo de nome de domínio). Para obter mais informações sobre delegação de DNS, consulte [delegação de domínio DNS](dns-domain-delegation.md).

Além disso, tem de garantir que os registos DNS para o domínio são sincronizados entre os dois fornecedores DNS. O DNS do Azure não suporta atualmente transferências de zona DNS. Registos DNS têm de ser sincronizados com o [portal de gestão do DNS do Azure](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [cmdlets do PowerShell](dns-operations-recordsets.md), ou [Ferramenta CLI](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>É necessário delegar o meu domínio para todos os quatro servidores de nome de DNS do Azure?

Sim. O DNS do Azure atribui quatro servidores de nomes para cada zona DNS, para uma maior resiliência e isolamento de falhas. Para se qualificar para o SLA de DNS do Azure, terá de delegar o seu domínio para todos os servidores de nome de quatro.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Quais são os limites de utilização para o DNS do Azure?

Os limites predefinidos seguintes aplicam-se ao utilizar o DNS do Azure:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Posso mover uma zona DNS do Azure entre os grupos de recursos ou entre subscrições?

Sim. Zonas DNS podem ser movidas entre grupos de recursos ou entre subscrições.

Não há nenhum impacto sobre em consultas DNS quando move uma zona DNS. Os servidores de nomes atribuídos à zona permaneçam a mesma e consultas DNS são processadas como habitualmente durante todo.

Para obter mais informações e instruções sobre como mover zonas DNS, consulte [mover recursos para um novo grupo de recursos ou subscrição](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Quanto tempo demora para que as alterações DNS em vigor?

Novas zonas DNS e registos DNS são normalmente refletidos em servidores de nome DNS do Azure rapidamente, dentro de alguns segundos.

As alterações para registos DNS existentes poderá demorar um pouco mais, mas ainda devem ser refletidas nos servidores de nomes DNS do Azure dentro de 60 segundos. Neste caso, a DNS a colocação em cache fora do DNS do Azure (por clientes DNS e resoluções de recursivo DNS) também pode afetar o tempo decorrido para uma alteração DNS ser eficaz. Esta duração da cache pode ser controlada através da propriedade Time-To-Live (TTL) de cada conjunto de registos.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Como pode proteger os meus zonas DNS contra eliminação acidental?

O DNS do Azure é gerido com o Azure Resource Manager e vantagens do controlo de acesso funcionalidades que o Azure Resource Manager fornece. Controlo de acesso baseado em funções pode ser utilizado para controlar quais os utilizadores tem de leitura ou de acesso de escrita para zonas DNS e conjuntos de registos. Bloqueios de recursos podem ser aplicados para impedir a modificação acidental ou eliminação de zonas DNS e conjuntos de registos.

Para obter mais informações, consulte [Protecting zonas de DNS e registos](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Como configurar os registos SPF no DNS do Azure?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Resolver Nameservers de DNS do Azure através de IPv6? 

Sim. Nameservers de DNS do Azure são pilha dupla (ter endereços IPv4 e IPv6). Para localizar o endereço IPv6 de nameservers o DNS do Azure atribuídos à sua zona DNS, pode utilizar uma ferramenta como nslookup (por exemplo, `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Como definir a segurança de nome de domínio um internacional (IDN) no DNS do Azure?

Nomes de domínio internacional (IDNs) funcionam com codificação cada através do nome DNS '[punycode](https://en.wikipedia.org/wiki/Punycode)'. Consultas DNS são efetuadas utilizando estes nomes com codificação punycode.

Pode configurar os nomes de domínio internacional (IDNs) no DNS do Azure convertendo primeiro o nome da zona ou nome do conjunto de registos para punycode. O DNS do Azure não suporta a conversão incorporada do punycode atualmente.

## <a name="private-dns"></a>Privada DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>DNS do Azure suporta domínios 'private'?
Suporte para domínios 'private' é implementado utilizando a funcionalidade de zonas privada.  Esta funcionalidade está atualmente disponível na pré-visualização pública.  Zonas privadas são geridas utilizando as ferramentas do mesmas como zonas de DNS do Azure para a internet, mas apenas estão resolvíveis nas redes virtuais especificadas.  Consulte o [descrição geral](private-dns-overview.md) para obter mais detalhes.

Neste momento, zonas de privada não são suportadas no portal do Azure. 

Para obter informações sobre outras opções de DNS internas no Azure, consulte [resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>O que é a diferença entre a rede virtual do registo e a rede virtual de resolução no contexto das zonas privadas? 
Pode ligar redes virtuais a uma zona privada DNS de duas formas - como uma rede virtual do registo ou como uma rede virtual de resolução. Em ambos os casos, as máquinas virtuais na rede virtual será capazes de resolver com êxito em relação a registos na zona privada. No entanto, se especificar uma rede virtual como uma rede virtual do registo, o Azure automaticamente registar registos DNS (registo dinâmico) para a zona para as máquinas virtuais na rede virtual. Além disso, quando uma máquina virtual no registo de uma rede virtual é eliminada, o Azure também automaticamente remover o registo DNS correspondente da zona privada ligada. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Zonas de DNS privado do Azure irá funcionar em regiões do Azure?
Sim. Zonas privada é suportada para a resolução DNS entre redes virtuais em regiões do Azure, mesmo sem peering explicitamente as redes virtuais, desde que as redes virtuais são especificadas como redes virtuais de resolução para a zona privada. Os clientes podem ter as redes virtuais para ser executado o peering tráfego TCP/HTTP para o fluxo a partir de uma região para outro. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>É a conectividade à Internet de redes virtuais necessárias para zonas privada?
Não. Zonas privada trabalhar em conjunto com redes virtuais e permita que os clientes de gerir domínios para máquinas virtuais ou outros recursos dentro e entre redes virtuais. É necessária para a resolução de nome sem conectividade internet. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>A zona privada mesmo pode ser utilizada para várias redes virtuais para a resolução? 
Sim. Os clientes podem associar uma única zona privada até 10 redes virtuais de resolução.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Pode uma rede virtual que pertença a uma subscrição diferente ser adicionada como uma rede virtual de resolução para uma zona privada? 
Sim, desde que o utilizador tem permissão de operação de escrita em ambas as redes virtuais, bem como a zona DNS privada. Tenha em atenção que a permissão de escrita pode ser atribuída a várias funções RBAC. Por exemplo, a função de clássico RBAC de contribuinte de rede tem permissões de escrita para redes virtuais. Para obter mais informações sobre funções do RBAC, consulte [controlo de acesso baseado em funções](../active-directory/role-based-access-control-what-is.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Serão os registos DNS registado automaticamente máquina virtual numa zona privada automaticamente eliminados quando as máquinas virtuais são eliminadas pelo cliente?
Sim. Se eliminar uma máquina virtual dentro de uma rede virtual do registo, iremos irá eliminar automaticamente os registos DNS que foram registados para o horário devido a esta que está a ser uma rede virtual do registo. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Um registo de máquina de virtual registado automaticamente numa zona privada (a partir de uma rede virtual do registo) pode ser eliminado manualmente? 
Não. Neste momento, os registos DNS de máquina virtual que são registados automaticamente numa zona privada a partir de uma rede virtual do registo não são visíveis ou editável pelos clientes. Pode, contudo, substituir (substituir) deste tipo registados automaticamente registos DNS com um DNS criado manualmente registam na zona. Consulte o seguinte pergunta e resposta que aborda este.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>O que acontece quando tentarmos criar manualmente um novo registo DNS para uma zona privada que tenha o mesmo nome de anfitrião como uma máquina virtual existente (automaticamente registada) numa rede virtual registo? 
Se tentar criar manualmente um novo registo DNS para uma zona privada que tenha o mesmo nome de anfitrião como uma máquina virtual (automaticamente registada) existente numa rede virtual do registo, iremos irá permitir novo registo DNS substituir automaticamente registado registo de máquina virtual. Além disso, se subsequentemente tentar eliminar este criados manualmente o registo DNS da zona, a eliminação terá êxito e o registo automático acontecerá novamente (o registo DNS será recriado automaticamente na zona), por isso, desde que a máquina virtual ainda existe e tem um IP privado anexados ao mesmo. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>O que acontece quando é desassociar uma rede virtual do registo de uma zona privada? Seriam possível remover os registos de registado automaticamente máquinas virtuais da rede virtual da zona, bem como?
Sim. Se desassociar uma rede virtual do registo (atualização da zona DNS para remover a rede virtual do registo associada) de uma zona privada, o Azure irá remover quaisquer registos automaticamente registado máquina virtual da zona. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>O que acontece quando é eliminar uma rede virtual registo (ou de resolução) que está ligada a uma zona privada? Que poderemos ter de atualizar manualmente a zona privada para a rede virtual de rede virtual como um registo (ou resolução) de ligação de anulação da zona?
Sim. Quando elimina uma rede virtual de registo (ou de resolução) sem desassociá-lo a partir de uma privada zona pela primeira vez, Azure irá permitir a operação de eliminação com êxito, mas a rede virtual não é automaticamente desassociada da sua zona privada se houver. Terá de desativar manualmente a rede virtual da zona privada. Por este motivo, recomenda-primeiro desassociar a rede virtual da sua zona privada antes de eliminá-lo.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>Seria resolução DNS a utilizar a predefinição FQDN (internal.cloudapp.net) continuarão a funcionar mesmo quando uma zona privados (por exemplo: contoso. local) está ligado a uma rede virtual? 
Sim. Funcionalidade de zonas privada não substitui as resoluções DNS predefinido utilizando a zona internal.cloudapp.net fornecidos pelo Azure e é oferecida como uma capacidade adicional ou melhoramento. Para ambos os casos (se a entidade confiadora na internal.cloudapp.net fornecidos pelo Azure ou na sua própria zona privada) é recomendada a consulta para utilizar o FQDN da zona que pretende resolver contra. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Seria alterado o sufixo DNS em máquinas virtuais dentro de uma rede virtual ligada para que a zona privada? 
Não. Neste momento, o sufixo DNS nas máquinas virtuais na sua rede virtual ligado permanecerá como o sufixo de fornecidos pelo Azure predefinido ("*. internal.cloudapp.net"). No entanto manualmente pode alterar este sufixo DNS nas suas máquinas virtuais para que a zona privada. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Existem algumas limitações para zonas privada durante esta pré-visualização?
Sim. Durante a pré-visualização pública, existem as seguintes limitações:
* 1 redes virtuais de registo por zona de privada
* Redes virtuais de até 10 resolução por zona de privada
* Uma determinada rede virtual só pode ser associada a uma zona de privada como uma rede virtual do registo
* Uma determinada rede virtual pode ser associada a até 10 privada zonas como uma rede virtual de resolução
* Se não for especificada uma rede virtual do registo, os registos DNS para as VMs de rede virtual que estão registados para a zona privada não serão visíveis ou recuperável do Powershell/CLI/APIs, mas os registos VM, de facto, estão registados e irão resolver com êxito
* DNS inversa só irá funcionar para o espaço de IP privado na rede virtual do registo
* Inversa de DNS para um IP privado que não está registado na zona privados (por exemplo: IP privado para uma máquina virtual numa rede virtual que está ligada como uma rede virtual de resolução para uma zona privada) irá devolver "internal.cloudapp.net" como o sufixo DNS, no entanto Este sufixo não será resolvível.   
* Rede virtual tem de estar em branco (revertidos nenhuma máquina virtual com um NIC anexado) quando inicialmente (revertidos a primeira) de ligação a uma zona privada como o registo ou de resolução de rede virtual. No entanto, a rede virtual pode, em seguida, não pode estar vazio para ligar futuras como um registo ou da resolução de rede virtual, para outras zonas privadas. 
* Neste momento, reencaminhamento condicional não é suportado, por exemplo para ativar a resolução entre redes do Azure e OnPrem. Para obter documentação sobre como os clientes podem tenha em consideração este cenário através de outros mecanismos, consulte [resolução de nomes para VMs e instâncias de função](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Existem quotas ou limites de zonas ou registos para zonas privada?
Não existem nenhum separados limites no número de zonas permitidas por subscrição ou o número de conjuntos de registos por zona para zonas privada. Pública e privada zonas contam para os limites DNS gerais conforme documentado [aqui](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Existe suporte de Portal para zonas privada?
Zonas de privada que já são criados através de mecanismos de não-Portal (API/PowerShell/CLI/SDKs) vai estar visíveis no portal do Azure, mas os clientes serão não é possível criar novas zonas privadas ou gerir as associações com redes virtuais. Além disso, para as redes virtuais associadas como redes virtuais do registo, os registos VM automaticamente registados não serão visíveis no Portal. 

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre o DNS do Azure](dns-overview.md)
<br>
[Saiba mais sobre como utilizar o DNS do Azure para domínios privados](private-dns-overview.md)
<br>
[Saiba mais sobre os registos e zonas DNS](dns-zones-records.md)
<br>
[Introdução ao DNS do Azure](dns-getstarted-portal.md)

