---
title: "Principais relatórios da Verizon | Microsoft Docs"
description: "Pode ver os padrões de utilização para a CDN utilizando os seguintes relatórios: largura de banda, os dados transferidos, pedidos com êxito, os Estados da Cache, rácio de acertos na Cache, IPV4/IPV6 dados transferidos."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: fa828bfa736d677fb4881e5cc2628c0e03eb8749
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="core-reports-from-verizon"></a>Relatórios de núcleos da Verizon

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Através da Verizon Core relatórios através do portal de gerir para perfis da Verizon, pode ver os padrões de utilização para a CDN com os seguintes relatórios:

* Largura de Banda
* Dados transferidos
* Acertos
* Estados da cache
* Rácio de acertos na cache
* Transferida de dados de IPv4/IPV6

## <a name="accessing-verizon-core-reports"></a>Ao aceder aos relatórios de núcleos da Verizon
1. No painel de perfil da CDN, clique o **gerir** botão.
   
    ![Botão de gerir do perfil de CDN](./media/cdn-reports/cdn-manage-btn.png)
   
    É aberto o portal de gestão do CDN.
2. Coloque o cursor sobre o **análise** separador, em seguida, coloque o cursor sobre o **Core relatórios** flyout. Clique num relatório no menu.
   
    ![Portal de gestão de CDN - menu de relatórios de núcleo](./media/cdn-reports/cdn-core-reports.png)

3. Para cada relatório, selecione um intervalo de datas do **intervalo de datas** lista. Pode selecionar um intervalo de datas predefinidos, tais como **hoje** ou **esta semana**, ou pode selecionar **personalizada** e introduzir um intervalo de datas manualmente clicando os ícones de calendário. 

4. Depois de selecionar um intervalo de datas, clique em **aceda** para gerar o relatório. 

4. Se pretender exportar os dados no formato de Excel, clique no ícone de Excel acima o **aceda** botão.

## <a name="bandwidth"></a>Largura de Banda
O relatório de largura de banda é composta por uma tabela de gráfico e os dados que indica a utilização de largura de banda da CDN para HTTP e HTTPS durante um período de tempo específico, em Mbps. Pode ver a utilização de largura de banda em todos os POPs ou para um determinado POP. Este relatório permite-lhe ver os picos de tráfego e a distribuição para os POPs.

Do **Edge nós** lista, selecione **todos os nós de limite** para ver o tráfego de todos os nós ou selecione uma região específica.

O relatório é atualizado a cada cinco minutos.

![Relatório de largura de banda](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Dados transferidos
Este relatório é composto por uma tabela de gráfico e os dados que indica a utilização de tráfego da CDN para HTTP e HTTPS durante um período de tempo específico, em GB. Pode ver a utilização de tráfego em todos os POPs ou para um determinado POP. Este relatório permite-lhe ver os picos de tráfego e a distribuição entre POPs.

Do **Edge nós** lista, selecione **todos os nós de limite** para ver o tráfego de todos os nós ou selecione uma região específica.

O relatório é atualizado a cada cinco minutos.

![Dados transferidos relatório](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Pedidos com êxito (códigos de estado)
Este relatório descreve a distribuição de códigos de estado de pedido para o seu conteúdo. Todos os pedidos para conteúdo, gera um código de estado HTTP. O código de estado descreve como edge POPs processada o pedido. Por exemplo, um código de estado 2xx indica que o pedido foi servido com êxito para um cliente, enquanto um código de estado 4xx indica que ocorreu um erro. Para obter mais informações sobre códigos de estado HTTP, consulte [códigos de estado de lista de HTTP](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

![Relatório de pedidos](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Estados da cache
Este relatório descreve a distribuição de acertos na cache e pedidos sem êxito de cache para pedidos de cliente. Porque o desempenho mais rápido resulta de acertos na cache, pode otimizar velocidades de entrega de dados, minimizando a pedidos de sem êxito na cache e de acertos na cache expirado. 

Para reduzir a cache de pedidos sem êxito, configurar o servidor de origem para minimizar a utilização dos seguintes procedimentos: 
 * `no-cache`cabeçalhos de resposta
 * Cadeia de consulta a colocação em cache, a menos que estritamente necessária  
 * Códigos de resposta não colocáveis

Para reduzir acertos na cache expirada, defina um recurso `max-age` para um período de tempo para minimizar o número de pedidos para o servidor de origem.

![Relatório de Estados de cache](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Estados de cache principal incluem:
* TCP_HIT: Servido a partir do servidor edge. O objeto foi na cache e não foi excedido o respetiva idade máxima.
* TCP_MISS: Servido a partir do servidor de origem. O objeto não foi na cache e a resposta foi novamente para a origem.
* TCP_EXPIRED _MISS: servido a partir do servidor de origem após a revalidação de origem. O objeto foi na cache, mas tinha excedeu a respetiva idade máxima. Uma revalidação com origem resultou no objeto de cache a ser substituído por uma nova resposta da origem.
* TCP_EXPIRED _HIT: servidos limite após a revalidação de origem. O objeto foi na cache, mas tinha excedeu a respetiva idade máxima. Uma revalidação com o servidor de origem resultou no objeto de cache que está a ser não modificado.

### <a name="full-list-of-cache-statuses"></a>Obter uma lista completa dos Estados de cache
* TCP_HIT - este estado é comunicado quando um pedido é servido diretamente a partir do POP ao cliente. Um recurso é servido imediatamente a partir de um POP quando é colocado em cache no POP mais próximo para o cliente e tem um válido time-to-live (TTL). TTL é determinado pelos seguintes cabeçalhos de resposta:
  
  * Cache-Control: s-maxage
  * Controlo de cache: idade máxima
  * Expira em
* TCP_MISS: Este estado indica que uma versão em cache do elemento pedida não foi encontrada no POP mais próximo para o cliente. O elemento é pedido a partir de um servidor de origem ou de um servidor de proteção da origem. Se o servidor de origem ou o servidor de proteção da origem devolve um recurso, é fornecido ao cliente e colocadas em cache no cliente e o servidor edge. Caso contrário, um código de estado não 200 (por exemplo, 403 proibido ou 404 não encontrado) é devolvido.
* TCP_EXPIRED_HIT: Este estado é comunicado quando um pedido que tenha como destino um elemento com um valor de TTL expirado servido diretamente a partir do POP ao cliente. Por exemplo, quando o elemento da idade máxima expirou. 
  
   Um pedido de expirada normalmente resulta num pedido revalidação para o servidor de origem. Para um Estado TCP_EXPIRED_HIT ocorrer, o servidor de origem tem de indicar que uma versão mais recente do elemento não existe. Esta situação resulta normalmente numa atualização de Cache-Control e cabeçalhos de expira o elemento.
* TCP_EXPIRED_MISS: Este estado é comunicado quando uma versão mais recente de um ativo em cache expirada é servida a partir de POP ao cliente. Este estado ocorre quando o valor de TTL para um ativo em cache expirou (por exemplo, expirado idade máxima) e o servidor de origem devolve uma versão mais recente esse recurso. Esta nova versão do elemento é fornecida para o cliente em vez da versão em cache. Além disso, este é colocado em cache no servidor edge e o cliente.
* CONFIG_NOCACHE: Este estado indica que uma configuração específica do cliente limite POP impediu o elemento que está a ser colocados em cache.
* NENHUM - este estado indica que não foi efetuada uma verificação de conteúdo de actualização da cache.
* TCP_CLIENT_REFRESH_MISS: Este estado é comunicado quando um cliente HTTP, por exemplo, um browser, força uma extremidade POP para obter uma nova versão de um recurso obsoleto do servidor de origem. Por predefinição, os servidores de impedem que um cliente HTTP forçar os servidores edge para obter uma nova versão do elemento do servidor de origem.
* TCP_PARTIAL_HIT: Este estado é comunicado quando um pedido de intervalo de byte resulta num acessos para um ativo parcialmente em cache. O intervalo de bytes pedido imediatamente é servido a partir de POP ao cliente.
* UNCACHEABLE: Este estado é comunicado quando um recurso `Cache-Control` e `Expires` cabeçalhos indicam que este deve não ser colocadas em cache num POP ou pelo cliente HTTP. São servidos estes tipos de pedidos do servidor de origem.

## <a name="cache-hit-ratio"></a>Rácio de acertos na cache
Este relatório indica a percentagem de pedidos em cache que foram enviadas diretamente a partir da cache.

O relatório fornece os seguintes detalhes:

* O conteúdo solicitado foi colocado em cache no POP mais próximo para o autor do pedido.
* O pedido foi servido diretamente a partir do limite da nossa rede.
* O pedido não obrigava a revalidação com o servidor de origem.

O relatório não incluem:

* Pedidos que são negados devido a opções de filtragem de país.
* Pedidos de recursos cujos cabeçalhos indicam que eles devem não ser colocados em cache. Por exemplo, `Cache-Control: private`, `Cache-Control: no-cache`, ou `Pragma: no-cache` cabeçalhos impedem que um recurso que está a ser colocados em cache.
* Pedidos de intervalo de byte para o conteúdo parcialmente em cache.

A fórmula é: (TCP_ ACESSOS / (TCP_ ACESSOS + TCP_MISS)) * 100

![Relatório de rácio de acertos na cache](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Transferir os dados de IPv4/IPV6
Este relatório mostra a distribuição de utilização de tráfego no IPV4 vs IPV6.

![Transferir os dados de IPv4/IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

## <a name="considerations"></a>Considerações
Relatórios só podem ser gerados dentro os últimos 18 meses.

