---
title: Noções sobre iDNS na pilha do Azure | Microsoft Docs
description: Noções sobre iDNS funcionalidades e capacidades na pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: scottnap
ms.openlocfilehash: 9123160f42adea57c28dff265bd5b5dbbcbb7918
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724263"
---
# <a name="introducing-idns-for-azure-stack"></a>Introdução ao iDNS para pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

iDNS é uma funcionalidade de rede de pilha do Azure que permite-lhe resolver nomes DNS externos (por exemplo, http://www.bing.com.) também permite-lhe registar nomes de rede virtual interna. Ao fazê-lo, pode resolver VMs na mesma rede virtual por nome em vez de endereço IP. Esta abordagem remove a necessidade de fornecer as entradas de servidor DNS personalizadas. Para mais informações sobre o DNS, consulte o [descrição geral do DNS do Azure](https://docs.microsoft.com/en-us/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>O que fazer iDNS?

Com iDNS na pilha do Azure, obtenha as seguintes capacidades, sem ter de especificar as entradas de servidor DNS personalizadas:

- Partilhado serviços de resolução de nome DNS para cargas de trabalho inquilinas.
- Serviço DNS autoritativo para a resolução do nome e o registo DNS na rede virtual de inquilino.
- Serviço DNS recursivo para a resolução de nomes de Internet de VMs inquilinas. Os inquilinos já não é necessário especificar as entradas de DNS personalizadas para resolver nomes de Internet (por exemplo, www.bing.com.)

Pode ainda traga a sua própria DNS e utilizar servidores DNS personalizados. No entanto, ao utilizar iDNS, pode resolver nomes de DNS da Internet e estabelecer ligação com as outras VMs na mesma rede virtual, não precisa de criação de entradas DNS personalizadas.

## <a name="what-doesnt-idns-do"></a>O que não iDNS?

Que iDNS não permite-lhe fazer, está a criar um registo DNS para um nome que pode ser resolvido a partir de fora da rede virtual.

No Azure, tem a opção de especificar uma etiqueta de nome DNS que está associada um endereço IP público. Pode escolher a etiqueta (prefixo), mas o Azure escolhe o sufixo, que se baseia na região na qual criar o endereço IP público.

![Exemplo de uma etiqueta de nome DNS](media/azure-stack-understanding-dns-in-tp2/image3.png)

Como mostra a imagem anterior, Azure irá criar um registo "A" no DNS para a etiqueta do nome DNS especificada na zona **westus.cloudapp.azure.com**. O prefixo e o sufixo são combinadas para compor um [nome de domínio completamente qualificado](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) que pode ser resolvido a partir em qualquer lugar na Internet pública.

Pilha do Azure só suporta iDNS para o registo do nome interno, pelo que não é possível efetuar o seguinte:

- Criar um registo DNS sob uma zona DNS alojada existente (por exemplo, local.azurestack.external.)
- Criar uma zona DNS (por exemplo, Contoso.com).
- Crie um registo na sua própria zona DNS personalizado.
- Suporta a compra de nomes de domínio.

## <a name="next-steps"></a>Passos Seguintes

[Utilizar o DNS na pilha do Azure](azure-stack-dns.md)
