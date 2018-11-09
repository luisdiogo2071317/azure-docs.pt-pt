---
title: O que é o DNS do Azure?
description: Descrição geral do serviço de alojamento de DNS no Microsoft Azure. Aloje o seu domínio no Microsoft Azure.
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: overview
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 9535ad73300406408a94a6f38f001e45ba7409a2
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093100"
---
# <a name="what-is-azure-dns"></a>O que é o DNS do Azure?

O DNS do Azure é um serviço de alojamento dos domínios DNS que oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.

Não pode utilizar o DNS do Azure para comprar um nome de domínio. Por uma taxa anual, pode comprar um nome de domínio com as [Aplicações Web do Azure](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app#buy-the-domain) ou uma entidade de registo de domínios de terceiros. Os domínios podem, então, ser alojados no DNS do Azure para a gestão de registos. Para obter mais informações, veja [Delegar um domínio ao DNS do Azure](dns-domain-delegation.md).

As seguintes funcionalidades estão incluídas com o DNS do Azure.

## <a name="reliability-and-performance"></a>Desempenho e fiabilidade

Os domínios DNS no DNS do Azure estão alojados na rede global do Azure de servidores de nomes DNS. O DNS do Azure utiliza a rede de transmissão. Cada consulta DNS é respondida pelo servidor DNS disponível mais próximo, para um desempenho rápido e de elevada disponibilidade para o seu domínio.

## <a name="security"></a>Segurança

 O DNS do Azure baseia-se no Azure Resource Manager, que disponibiliza funcionalidades como:

* [Controlo de acesso baseado em funções](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#access-control) para controlar quem tem acesso a ações específicas para a sua organização.

* [Registos de atividades](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#activity-logs) para monitorizar a forma como um utilizador na sua organização alterou um recurso ou encontrar um erro quando resolver um problema.

* [Bloqueio de recurso](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) para bloquear uma subscrição, grupo de recursos ou recurso. O bloqueio impede que outros utilizadores na sua organização eliminem ou modifiquem acidentalmente recursos críticos.

Para obter mais informações, veja [Como proteger zonas DNS e registos](dns-protect-zones-recordsets.md). 


## <a name="ease-of-use"></a>Facilidade de utilização

 O DNS do Azure pode gerir os registos DNS dos seus serviços do Azure e também conceder o DNS para os seus recursos externos. O DNS do Azure está integrado no portal do Azure e utiliza as mesmas credenciais, contrato de suporte e faturação como os outros serviços do Azure. 

A faturação do DNS é baseada no número de zonas DNS alojadas no Azure e no número de consultas DNS recebidas. Para obter mais informações sobre preços, veja [Preços do DNS do Azure](https://azure.microsoft.com/pricing/details/dns/).

Os domínios e registos podem ser geridos com o portal do Azure, cmdlets do Azure PowerShell e a CLI do Azure de várias plataformas. As aplicações que exigem gestão automatizada de DNS podem ser integradas com o serviço através da API REST e SDKs.

## <a name="customizable-virtual-networks-with-private-domains"></a>Redes virtuais personalizáveis com domínios privados

O DNS do Azure também suporta domínios DNS privados, com uma funcionalidade que se encontra atualmente em pré-visualização pública. Esta funcionalidade permite-lhe utilizar os seus próprios nomes de domínio personalizados nas suas redes virtuais privadas, em vez dos nomes apresentados pelo Azure atualmente disponíveis.

Para obter mais informações, veja [Utilizar o DNS do Azure para domínios privados](private-dns-overview.md).

## <a name="alias-records"></a>Registos de alias

O DNS do Azure suporta conjuntos de registos de alias. Pode utilizar um conjunto de registos de alias para fazer referência a um recurso do Azure, como um endereço IP público do Azure ou um perfil do Gestor de Tráfego do Azure. Se o endereço IP do recurso subjacente for alterado, o conjunto de registos de alias atualiza-se automaticamente durante a resolução de DNS. O conjunto de registo de alias aponta para a instância do serviço e esta está associada a um endereço IP. 

Além disso, pode agora apontar o seu apex ou domínio sem "www" para um perfil de Gestor de Tráfego através de um registo de alias. Um exemplo é contoso.com.

Para obter mais informações, consulte [Descrição geral do registo de alias do DNS do Azure](dns-alias.md).


## <a name="next-steps"></a>Passos seguintes

* Para saber mais sobre zonas DNS e registos, veja [Descrição geral das zonas DNS e registos](dns-zones-records.md).

* Para saber como criar uma zona no DNS do Azure, veja [Criar uma zona DNS](./dns-getstarted-create-dnszone-portal.md).

* Para ver as perguntas mais frequentes sobre o DNS do Azure, veja [FAQ sobre DNS do Azure](dns-faq.md).

