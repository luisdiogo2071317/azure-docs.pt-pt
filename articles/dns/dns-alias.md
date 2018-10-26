---
title: Registos descrição geral de alias de DNS do Azure
description: Descrição geral do suporte para registos de alias no DNS do Azure de Microsoft.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52b42e964e7abe207064aff49f7f8f27f8476ef4
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092847"
---
# <a name="azure-dns-alias-records-overview"></a>Registos descrição geral de alias de DNS do Azure

Registos de alias de DNS do Azure são qualificações num conjunto de registos de DNS. Eles podem fazer referência a outros recursos do Azure a partir de sua zona DNS. Por exemplo, pode criar um conjunto de registos de alias que faz referência a um endereço IP público do Azure em vez de um registo. Os pontos do conjunto de registos alias para um IP público do Azure endereços dinamicamente a instância de serviço. Como resultado, o conjunto de registos de alias perfeitamente atualiza automaticamente durante a resolução de DNS.

Um conjunto de registos de alias é suportado para os seguintes tipos de registos numa zona de DNS do Azure: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Registos de alias para os A ou AAAA tipos de registos para o Gestor de tráfego do Azure são suportados apenas para tipos de ponto final externo. Tem de fornecer o endereço IPv4 ou IPv6, conforme apropriado, para os pontos finais externos no Gestor de tráfego. O ideal é que utilizam IPs estáticos para o endereço.

## <a name="capabilities"></a>Capacidades

- **Ponto para um recurso IP público a partir de um conjunto de registos de A/AAAA de DNS.** Pode criar um conjunto de registos A/AAAA e torná-lo um conjunto de registos de alias para apontar para um recurso IP público.

- **Ponto para um perfil do Gestor de tráfego a partir de um conjunto de registos DNS A/AAAA/CNAME.** Pode apontar para o CNAME de um perfil do Gestor de tráfego de um conjunto de registos CNAME no DNS. Um exemplo é contoso.trafficmanager.net. Agora, também pode apontar para um perfil de Gestor de tráfego que tem pontos finais externos a partir de um conjunto na sua zona DNS de registos A ou AAAA.

   > [!NOTE]
   > Registos de alias para os A ou AAAA tipos de registos para o Gestor de tráfego são suportados apenas para tipos de ponto final externo. Tem de fornecer o endereço IPv4 ou IPv6, conforme apropriado, para os pontos finais externos no Gestor de tráfego. O ideal é que utilizam IPs estáticos para o endereço.
   
- **Aponte para outro conjunto de registos de DNS na mesma zona.** Os registos de alias podem fazer referência a outros conjuntos de registos do mesmo tipo. Por exemplo, um conjunto de registos CNAME de DNS pode ser um alias para outro conjunto de registos CNAME do mesmo tipo. Essa disposição é útil se desejar alguns conjuntos de registos para ser aliases e alguns não aliases.

## <a name="scenarios"></a>Cenários
Existem alguns cenários comuns para os registos de Alias.

### <a name="prevent-dangling-dns-records"></a>Impedir dangling registos DNS
 Dentro de zonas de DNS do Azure, os registos de alias podem ser utilizados para perto controlar o ciclo de vida dos recursos do Azure. Os recursos incluem um IP público ou um perfil do Gestor de tráfego. Um problema comum com registos DNS tradicionais é dangling registos. Este problema ocorre especialmente com A/AAAA ou CNAME tipos de registos. 

Com um registo de zona DNS tradicional, se o IP de destino ou CNAME já não existir, o registo de zona DNS não sabe. Como resultado, o registo tem de ser atualizado manualmente. Em algumas organizações, esta atualização manual não poderá ocorrer no tempo. Também pode ser problemático devido a separação de funções e níveis de permissão associada.

Por exemplo, uma função pode ter a autoridade para eliminar um CNAME ou endereço IP que pertence a uma aplicação. Mas não tem autoridade suficiente para atualizar o registo DNS que aponta para esses destinos. Ocorre um atraso de tempo entre quando o IP ou o CNAME é eliminado e o registo DNS que aponta para o mesmo é removido. Este atraso de tempo potencialmente causar uma falha para os utilizadores.

Registos de alias remover a complexidade associada este cenário. Eles ajudam a impedir dangling referências. Veja, por exemplo, um registo DNS que é qualificado como um registo de alias para apontar para um IP público ou um perfil do Gestor de tráfego. Se esses recursos subjacentes forem excluídos, o registo de alias DNS é removido ao mesmo tempo. Este processo torna-se de que os utilizadores nunca sofrem um período de indisponibilidade.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Atualizar as zonas DNS automaticamente quando o aplicativo IPs alterar

Este cenário é semelhante ao anterior. Talvez um aplicativo é movido, ou a máquina virtual subjacente é reiniciada. Um registo de alias, em seguida, atualiza automaticamente quando o endereço IP é alterado para o recurso IP público subjacente. Para evitar potenciais riscos de segurança, os usuários diretos de outro aplicativo que tem o endereço IP antigo.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Alojar aplicações com balanceamento de carga no vértice da zona

O protocolo DNS impede a atribuição de nada além de um registo A ou AAAA no vértice da zona. Um exemplo é contoso.com. Esta restrição apresenta um problema para os proprietários da aplicação que tenham aplicações com balanceamento de carga por trás do Gestor de tráfego. Não é possível apontar para o perfil do Gestor de tráfego do registo de apex de zona. Como resultado, os proprietários da aplicação tem de utilizar uma solução alternativa. Um redirecionamento na camada da aplicação tem de redirecionar do vértice da zona para outro domínio. Um exemplo é um redirecionamento de contoso.com para www.contoso.com. Essa organização apresenta um ponto único de falha para a função de redirecionamento.

Com os registros de alias, esse problema deixou de existir. Agora os proprietários de aplicativos podem apontar o respetivo registo de apex de zona para um perfil de Gestor de tráfego que tem pontos finais externos. Os proprietários de aplicativos podem apontar para o mesmo perfil de Gestor de tráfego, que é utilizado para qualquer outro domínio na sua zona DNS. Por exemplo, contoso.com e www.contoso.com podem apontar para o mesmo perfil de Gestor de tráfego. Esse é o caso, desde que o perfil do Gestor de tráfego tem apenas pontos finais externos configurados.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os registos de alias, veja os artigos seguintes:

- [Tutorial: Configurar um registo de alias para fazer referência a um endereço IP público do Azure](tutorial-alias-pip.md)
- [Tutorial: Configurar um registo de alias para oferecer suporte a nomes de domínio com o Gestor de tráfego do vértice](tutorial-alias-tm.md)
- [FAQ sobre DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
