---
title: Registos descrição geral de alias de DNS do Azure
description: Descrição geral do suporte para registos de alias no DNS do Azure de Microsoft.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 05a6a1700de2b8b334b40d9efd9b8d79e9e7241f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957577"
---
# <a name="azure-dns-alias-records-overview"></a>Registos descrição geral de alias de DNS do Azure

Registos de alias DNS do Azure são a qualificação por um conjunto de registos DNS que permite que faça referência a outros recursos do Azure a partir de sua zona DNS. Por exemplo, pode criar um conjunto de registros de alias que faz referência a um endereço IP público do Azure em vez disso, um registo. Uma vez que o seu conjunto de registros de alias aponta para uma instância de serviço de endereço IP público do Azure dinamicamente, o conjunto de registos de alias atualiza automaticamente sem problemas durante a resolução de DNS.

Um conjunto de registos de alias é suportado para os seguintes tipos de registos numa zona de DNS do Azure: A, AAAA e CNAME. 

> [!NOTE]
> Registos de alias para os A ou AAAA tipos de registos para o Gestor de tráfego só são suportados para tipos de ponto final externo. Tem de fornecer o endereço IPv4 ou IPv6 (IPs estáticos Idealmente) conforme apropriado para pontos finais externos no Gestor de tráfego.

## <a name="capabilities"></a>Capacidades

- **Apontar para um recurso de IP público a partir de um conjunto de registos de A/AAAA de DNS**. Pode criar um conjunto de registos A/AAAA e torná-lo um conjunto de registos de alias para apontar para um recurso de IP público.
- **Aponte para um perfil do Gestor de tráfego de um conjunto de registos DNS A/AAAA/CNAME**. Além de poder apontar para o CNAME de um perfil do Gestor de tráfego (por exemplo: contoso.trafficmanager.net) de um conjunto de registros DNS CNAME, agora também pode apontar para um perfil de Gestor de tráfego que tem pontos finais externos, a partir de um conjunto de registros A ou AAAA no DNS zona.
   > [!NOTE]
   > Registos de alias para os A ou AAAA tipos de registos para o Gestor de tráfego só são suportados para tipos de ponto final externo. Tem de fornecer o endereço IPv4 ou IPv6 (IPs estáticos Idealmente) conforme apropriado para pontos finais externos no Gestor de tráfego.
- **Aponte para outro conjunto de registos de DNS na zona mesmo**. Registos de alias podem fazer referência a outros conjuntos de registos do mesmo tipo. Por exemplo, pode ter um conjunto de registros de DNS CNAME ser um alias para outro conjunto de registros CNAME do mesmo tipo. Isto é útil se quiser ter alguns conjuntos de registros ser aliases e alguns como não aliases em termos de comportamento.

## <a name="scenarios"></a>Cenários
Existem alguns cenários comuns para os registos de Alias:

### <a name="prevent-dangling-dns-records"></a>Impedir dangling registos DNS
De dentro de zonas de DNS do Azure, os registos de alias podem ser utilizados para perto controlar o ciclo de vida de recursos do Azure como um perfil de IP público e o Gestor de tráfego. Um dos problemas comuns com registos DNS tradicionais é "registos dangling", especialmente com A/AAAA ou CNAME tipos de registos. 

Com um registo de zona DNS tradicional, se o IP de destino ou CNAME já não existir, o registo de zona DNS não tem conhecimento do fato e tem de ser atualizado manualmente. Em algumas organizações, esta atualização manual não pode ocorrer no tempo ou pode ser problemática devido a separação de funções e níveis de permissão associada.

Por exemplo, a função que tem autoridade para eliminar um CNAME ou endereço IP que pertençam a um aplicativo pode não ter autoridade suficiente para atualizar o registo DNS que aponta para esses destinos. Como resultado, poderá ocorrer um atraso de tempo entre quando o IP ou o CNAME é eliminado e o registo DNS que aponta para ela for removido, o que poderia potencialmente causar uma falha para os utilizadores finais.

Registos de alias remova a complexidade associada este cenário e ajudar a impedir que tais referências dangling. Quando um registo DNS é qualificado como um registo de alias para apontar para um IP público ou um perfil do Gestor de tráfego, e se esses recursos subjacentes são eliminados, o registo de alias DNS também é removido ao mesmo tempo. Isto garante que os usuários finais nunca sofrer um período de indisponibilidade.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Atualizar as zonas DNS automaticamente quando o aplicativo IPs alterar

Semelhante ao cenário anterior, se um aplicativo for movido, ou se a máquina virtual subjacente for reiniciada, um registo de alias é atualizado automaticamente quando o endereço IP é alterado para o recurso de IP público subjacente. Se os utilizadores finais são direcionados para outro aplicativo que tem o endereço IP antigo, pode evitar potenciais riscos de segurança.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Alojar as aplicações com balanceamento de carga no vértice da zona

O protocolo DNS impede a atribuição de nada além de um registo A ou AAAA no vértice da zona (por exemplo: contoso.com). Isso apresenta um problema para os proprietários de aplicativos que têm de carga com balanceamento de aplicativos por trás de um Gestor de tráfego, como não foi possível apontar para o perfil do Gestor de tráfego do registo de apex de zona. Como resultado, os proprietários dos aplicativos eram forçados a usar uma solução alternativa. Por exemplo, um redirecionamento na camada da aplicação para redirecionar do vértice da zona para outro domínio (a partir do contoso.com para www.contoso.com). Isso apresenta um ponto único de falha em termos da funcionalidade de redirecionamento.

Com os registros de alias, esse problema deixou de existir. Os proprietários da aplicação podem direcionar respetivo registo de apex de zona para um perfil de Gestor de tráfego que tem pontos finais externos. Com esta capacidade, os proprietários de aplicativos podem apontar para o mesmo perfil de Gestor de tráfego, que é utilizado para qualquer outro domínio na sua zona DNS. Por exemplo, contoso.com e www.contoso.com podem ambos apontar para o mesmo perfil de Gestor de tráfego, desde que o perfil do Gestor de tráfego tem apenas pontos finais externos configurados.

## <a name="next-steps"></a>Passos Seguintes

Para Saiba mais sobre os registos de alias, veja os artigos de follwing:

- [Tutorial: Configurar um registo de alias para fazer referência a um endereço IP público do Azure](tutorial-alias-pip.md)
- [Tutorial: Configurar um registo de alias para oferecer suporte a nomes de domínio com o Gestor de tráfego do vértice](tutorial-alias-tm.md)
- [FAQ sobre DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
