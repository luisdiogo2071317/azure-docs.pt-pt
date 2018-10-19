---
title: O que é o DNS do Azure?
description: Descrição geral do serviço de alojamento de DNS no Microsoft Azure. Aloje o seu domínio no Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: f68281f71ed525798410fb80b0d0175d38050c40
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829924"
---
# <a name="what-is-azure-dns"></a>O que é o DNS do Azure?

O DNS do Azure é um serviço de alojamento dos domínios DNS que oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.

Não pode utilizar o DNS do Azure para comprar um nome de domínio. Por uma taxa anual, pode comprar um nome de domínio com as [Aplicações Web do Azure](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) ou uma entidade de registo de domínios de terceiros. Os domínios podem, então, ser alojados no DNS do Azure para a gestão de registos. Veja [Delegar um Domínio ao DNS do Azure](dns-domain-delegation.md) para obter detalhes.

As seguintes funcionalidades estão incluídas com o DNS do Azure:

## <a name="reliability-and-performance"></a>Desempenho e fiabilidade

Os domínios DNS no DNS do Azure estão alojados na rede global do Azure de servidores de nomes DNS. O DNS do Azure utiliza qualquer rede de transmissão de forma a que cada consulta DNS seja respondida pelo servidor DNS disponível mais próximo, para um desempenho rápido e de elevada disponibilidade para o seu domínio.

## <a name="security"></a>Segurança

O serviço DNS do Azure baseia-se no Azure Resource Manager, que disponibiliza funcionalidades como:

* [controlo de acesso baseado em funções](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control) - para controlar quem tem acesso a ações específicas para a sua organização.

* [registos de atividades](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs) - para monitorizar a forma como um utilizador na sua organização alterou um recurso ou encontrar um erro quando resolver um problema.

* [bloqueio de recurso](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) - para bloquear uma subscrição, grupo de recursos ou recursos, para impedir que outros utilizadores na sua organização eliminem ou modifiquem acidentalmente recursos importantes.

Para obter mais informações, veja [Como proteger zonas DNS e registos](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Facilidade de utilização

O serviço DNS do Azure pode gerir os registos DNS dos seus serviços do Azure e também fornecer o DNS para os seus recursos externos. O DNS do Azure está integrado no portal do Azure e utiliza as mesmas credenciais, contrato de suporte e faturação como os outros serviços do Azure. 

A faturação do DNS é baseada no número de zonas DNS alojadas no Azure e no número de consultas DNS recebidas. Para obter mais informações sobre preços, veja [Preços do DNS do Azure](https://azure.microsoft.com/pricing/details/dns/).

Os domínios e registos podem ser geridos com o portal do Azure, cmdlets do Azure PowerShell e a CLI do Azure de várias plataformas. As aplicações que exigem gestão automatizada de DNS podem ser integradas com o serviço através da API REST e SDKs.

## <a name="customizable-virtual-networks-with-private-domains"></a>Redes virtuais personalizáveis com domínios privados

O DNS do Azure também suporta domínios DNS privados, que se encontram atualmente em pré-visualização pública. Isto permite-lhe utilizar os seus próprios nomes de domínio personalizados nas suas redes virtuais privadas, em vez dos nomes apresentados pelo Azure atualmente disponíveis.

Para obter mais informações, veja [Utilizar o DNS do Azure para domínios privados](private-dns-overview.md).

## <a name="alias-records"></a>Registos de alias

O DNS do Azure suporta conjuntos de registos de alias. Pode utilizar um conjunto de registos de alias para fazer referência a um recurso do Azure, como um endereço IP público do Azure ou um perfil do Gestor de Tráfego. Se o endereço IP do recurso subjacente for alterado, o conjunto de registos de alias atualiza-se automaticamente durante a resolução de DNS. O conjunto de registo de alias aponta para a instância do serviço e esta está associada a um endereço IP. 

Além disso, pode agora apontar o seu apex ou domínio sem "www" (por exemplo, contoso.com) para um perfil de Gestor de Tráfego através de um registo de alias.

Para obter mais informações, consulte [Descrição geral do registo de alias do DNS do Azure](dns-alias.md).


## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre zonas DNS e registos em [Descrição geral das zonas DNS e registos](dns-zones-records.md).

* Saiba como criar uma zona no DNS do Azure em [Criar uma zona DNS](./dns-getstarted-create-dnszone-portal.md).

* Para ver as perguntas mais frequentes sobre o DNS do Azure, veja [FAQ sobre DNS do Azure](dns-faq.md).

