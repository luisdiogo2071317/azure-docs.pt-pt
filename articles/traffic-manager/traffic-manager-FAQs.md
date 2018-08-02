---
title: Gestor de tráfego do Azure - FAQ | Documentos da Microsoft
description: Este artigo fornece respostas para perguntas freqüentes sobre o Gestor de tráfego
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: kumud
ms.openlocfilehash: bac3747f3f410e63454f543c035d7e04c20fac2a
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399182"
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>O Gestor de tráfego perguntas mais frequentes (FAQ)

## <a name="traffic-manager-basics"></a>Noções básicas do Gestor de tráfego

### <a name="what-ip-address-does-traffic-manager-use"></a>Que endereço IP utilizar o Gestor de tráfego?

Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), Gestor de tráfego funciona ao nível do DNS. Envia as respostas DNS para direcionar clientes para o ponto de extremidade de serviço apropriado. Os clientes, em seguida, ligar para o ponto final de serviço diretamente, não através do Gestor de tráfego.

Por conseguinte, o Gestor de tráfego não fornece um ponto final ou o endereço IP para a ligação para clientes. Se quiser o endereço IP estático para o seu serviço, que tem de ser configurado no serviço, não no Gestor de tráfego.

### <a name="what-types-of-traffic-can-be-routed-using-traffic-manager"></a>O que tipos de tráfego podem ser encaminhado através do Gestor de tráfego?
Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), um ponto de final do Gestor de tráfego pode ser qualquer internet voltada para o serviço hospedado dentro ou fora do Azure. Por conseguinte, o Gestor de tráfego pode encaminhar o tráfego que provém da internet pública para um conjunto de pontos de extremidade que é também com acesso à internet. Se tiver pontos finais que estão dentro de uma rede privada (por exemplo, uma versão interna do [Balanceador de carga do Azure](../load-balancer/load-balancer-overview.md#internalloadbalancer)) ou têm utilizadores garantindo DNS pedidos de tais redes internas, Gestor de tráfego não pode ser utilizado para esses tráfego.


### <a name="does-traffic-manager-support-sticky-sessions"></a>O Gestor de tráfego oferece suporte a sessões "adesivos"?

Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), Gestor de tráfego funciona ao nível do DNS. Utiliza as respostas de DNS para direcionar clientes para o ponto de extremidade de serviço apropriado. Os clientes ligam ao ponto final do serviço diretamente, não através do Gestor de tráfego. Por conseguinte, o Gestor de tráfego não vê o tráfego HTTP entre o cliente e o servidor.

Além disso, o endereço IP de origem da consulta DNS recebida pelo Gestor de tráfego pertence ao serviço DNS recursivo, não no cliente. Por conseguinte, o Gestor de tráfego não tem nenhuma forma para controlar os clientes individuais e não é possível implementar "adesivos" sessões. Esta limitação é comum a todos os sistemas de gestão de tráfego com base no DNS e não é específica para o Gestor de tráfego.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Por que estou a ver um erro HTTP ao utilizar o Gestor de tráfego?

Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), Gestor de tráfego funciona ao nível do DNS. Utiliza as respostas de DNS para direcionar clientes para o ponto de extremidade de serviço apropriado. Os clientes, em seguida, ligar para o ponto final de serviço diretamente, não através do Gestor de tráfego. Gestor de tráfego não não ver o tráfego HTTP entre cliente e servidor. Por conseguinte, qualquer erro HTTP que vê deve ser proveniente de seu aplicativo. Para o cliente ligar à aplicação, todos os passos de resolução DNS estão completos. Isso inclui qualquer interação com o Gestor de tráfego no fluxo de tráfego de aplicativo.

Investigação adicional, por conseguinte, deve se concentrar no aplicativo.

O cabeçalho de anfitrião HTTP enviado a partir do navegador do cliente é a fonte mais comuns dos problemas. Certifique-se de que a aplicação está configurada para aceitar o cabeçalho de anfitrião correto para o nome de domínio que está a utilizar. Para pontos de extremidade com o serviço de aplicações do Azure, consulte [configurar um nome de domínio personalizado para uma aplicação web no serviço de aplicações do Azure utilizando o Gestor de tráfego](../app-service/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>O que é o impacto no desempenho da utilização do Gestor de tráfego?

Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), Gestor de tráfego funciona ao nível do DNS. Uma vez que os clientes ligam diretamente para os pontos finais de serviço, não há nenhum impacto no desempenho incorrido quando utilizar o Gestor de tráfego assim que a ligação ser estabelecida.

Uma vez que o Gestor de tráfego integrado nas aplicações ao nível do DNS, ele exige uma pesquisa de DNS adicional para ser inserido na cadeia de resolução de DNS. O impacto do Gestor de tráfego em tempo de resolução DNS é mínimo. O Gestor de tráfego utiliza uma rede global de servidores de nomes e usa [anycast](https://en.wikipedia.org/wiki/Anycast) de rede para garantir que o DNS consultas são sempre encaminhadas para o servidor de nome disponível mais próximo. Além disso, a colocação em cache de respostas DNS significa que a latência DNS adicional gasta com o Gestor de tráfego aplica-se apenas a uma fração de sessões.

O método de desempenho encaminha o tráfego para o ponto de extremidade disponível mais próximo. O resultado líquido é que o impacto no desempenho geral associado com este método deve ser um mínimo. Qualquer aumento na latência DNS deverá ser compensado pela latência de rede mais baixa para o ponto final.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Os protocolos de aplicação posso utilizar com o Gestor de tráfego?

Conforme explicado [como funciona o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md), Gestor de tráfego funciona ao nível do DNS. Depois de concluída a pesquisa DNS, os clientes ligam ao ponto final da aplicação diretamente, não através do Gestor de tráfego. Por conseguinte, a ligação pode utilizar qualquer protocolo de aplicação. Se selecionar TCP como o monitorização, o Gestor de tráfego do protocolo monitorização de estado de funcionamento do ponto final pode ser feito sem utilizar quaisquer protocolos de aplicação. Se optar por ter o estado de funcionamento verificado com um protocolo de aplicação, o ponto final tem de ser capaz de responder a pedidos HTTP ou HTTPS obter.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Pode utilizar o Gestor de tráfego com um nome de domínio 'sem "www"'?

Não. As normas DNS não permitem CNAMEs de modo a coexistir com outros registos DNS, o mesmo nome. O vértice (ou raiz) de uma zona DNS sempre contém dois registos DNS já existentes; a SOA e os registos NS autoritativos. Isso significa que não pode ser criado um registo CNAME no vértice da zona sem violar as normas DNS.

O Gestor de tráfego necessita de um registo CNAME no DNS para mapear o nome DNS personalizado. Por exemplo, mapear `www.contoso.com` para o nome DNS de perfil do Gestor de tráfego `contoso.trafficmanager.net`. Além disso, o perfil do Gestor de tráfego retorna um segundo CNAME de DNS para indicar qual o cliente deve se conectar ao ponto de extremidade.

Para contornar este problema, recomendamos que utilize um redirecionamento HTTP para direcionar o tráfego do nome de domínio sem "www" para um URL diferente, que, em seguida, pode utilizar o Gestor de tráfego. Por exemplo, o domínio sem "www", "contoso.com" pode redirecionar os utilizadores para o CNAME "www.contoso.com" que aponta para o nome DNS do Gestor de tráfego.

Suporte completo para domínios sem "www" no Gestor de tráfego é rastreado no nosso registo de segurança do recurso. Pode registrar o suporte para este pedido de funcionalidade por [voto para o mesmo no nosso site de comentários da Comunidade](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>O Gestor de tráfego considerar o endereço de sub-rede do cliente quando o processamento de consultas DNS? 
Sim, além do endereço IP de origem da consulta DNS que recebe (que é normalmente o endereço IP do resolvedor de DNS), ao realizar pesquisas para métodos de encaminhamento geográfico e o desempenho, o Gestor de tráfego também considera o endereço de sub-rede do cliente se for incluído na consulta, o resolvedor que efetua o pedido em nome do utilizador final.  
Especificamente, [RFC 7871 – sub-rede de cliente em consultas de DNS](https://tools.ietf.org/html/rfc7871) que fornece um [mecanismo de extensão para o DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) que pode transmitir o endereço de sub-rede de cliente da resoluções de que o suportam.

### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>O que é o TTL de DNS e como afeta os meus utilizadores?

Quando uma consulta DNS que chegam no Gestor de tráfego, define um valor na resposta chamada time-to-live (TTL). Este valor, cuja unidade está em segundos, indica ao resoluções DNS downstream no quanto para essa resposta em cache. Enquanto resoluções DNS não são garantidas para colocar em cache neste resultado, a colocação em cache permite que sejam responder a consultas subseqüentes desativar o cache em vez de passar para servidores DNS do Gestor de tráfego. Isso afeta as respostas da seguinte forma:
- um valor de TTL superior reduz o número de consultas que direcionado para os servidores de DNS do Gestor de tráfego, que podem reduzir o custo de um cliente, uma vez que o número de consultas servidos é um uso faturável.
- um valor de TTL superior pode reduzir o tempo que demora para fazer uma pesquisa de DNS.
- um valor de TTL superior também significa que os dados não refletem as últimas informações de estado de funcionamento que o Gestor de tráfego tenha obtida por meio de seus agentes de pesquisa.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Alto ou baixo, pode definir o valor de TTL para respostas de Gestor de tráfego?

Pode definir, num por nível de perfil, o valor de TTL de DNS como tão baixo como 0 segundos e tão elevada como 2,147,483,647 segundos (em conformidade com o intervalo de máximo [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). Um valor de TTL de 0 significa que a jusante resoluções DNS não colocar em cache as respostas de consulta e todas as consultas devem contactar o DNS do Gestor de tráfego com os servidores para a resolução.

### <a name="how-can-i-understand-the-volume-of-queries-coming-to-my-profile"></a>Como posso compreender o volume de consultas a chegar ao meu perfil? 
Uma das métricas fornecidas pelo Gestor de tráfego é o número de consulta responderam por um perfil. Pode obter estas informações numa agregação de nível de perfil ou pode dividi-lo para ver o volume de consultas em que foram devolvidos os pontos de extremidade específicos. Além disso, pode configurar alertas para notificá-lo se o volume de resposta da consulta cruza as condições que definiu. Para obter mais detalhes, [Gestor de tráfego métricas e alertas](traffic-manager-metrics-alerts.md).

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Método de encaminhamento de tráfego do Gestor de tráfego geográfico

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Quais são alguns casos de utilização em que o encaminhamento geográfico é útil? 
Tipo de encaminhamento geográfico pode ser utilizado em qualquer cenário em que um cliente do Azure precisa distinguir os utilizadores com base em regiões geográficas. Por exemplo, usando o método de encaminhamento de tráfego geográfico, pode dar aos utilizadores de regiões específicas uma experiência de utilizador diferente daqueles partir de outras regiões. Outro exemplo é o cumprimento mandatos de soberania de dados locais que exigem que os utilizadores de uma região específica ser servidos apenas pelos pontos de extremidade nessa região.

### <a name="how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method"></a>Como posso decidir se deve a usar o método de encaminhamento desempenho ou método de encaminhamento geográfico? 
A principal diferença entre esses dois métodos de encaminhamento populares é que, no seu principal objetivo é enviar tráfego para o ponto final que pode fornecer a latência mais baixa para o chamador, ao passo que, em Geographic routing o objetivo principal é para impor uma geo do método de encaminhamento de desempenho cerca de seus chamadores para que possa roteá-los deliberadamente para um ponto de extremidade específico. A sobreposição ocorre uma vez que existe uma correlação entre o limite de proximidade geográfica e latências mais baixas, embora isso não é sempre verdadeiro. Poderá haver um ponto final de uma localização geográfica diferente que pode proporcionar uma melhor experiência de latência para o chamador e nesse caso o encaminhamento de desempenho irá enviar o utilizador para esse ponto final, mas encaminhamento geográfico será sempre enviá-los para o ponto final que mapeou para seus região geográfica. Para ainda mais a torná-lo a limpar, considere o exemplo a seguir - com Geographic encaminhamento pode fazer mapeamentos incomuns, tal como enviar todo o tráfego de Ásia para pontos de extremidade nos Estados Unidos e todo o tráfego dos EUA para pontos de extremidade na Ásia. Nesse caso, o encaminhamento geográfico deliberadamente fará exatamente o que tiver configurado para fazer e otimização de desempenho não é uma consideração. 
>[!NOTE]
>Poderão existir cenários em que poderá ter ambos os desempenho e capacidades de encaminhamento geográficas, para estes perfis aninhados de cenários podem ser a escolha ideal. Por exemplo, pode configurar um perfil de principal com o serviço de encaminhamento geográfico para onde enviar todo o tráfego da América do Norte para um perfil aninhado que tem pontos finais nos EUA e utilizar o encaminhamento para enviar essas tráfego para o melhor ponto final dentro desse conjunto do desempenho. 

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Quais são as regiões suportadas pelo Gestor de tráfego para encaminhamento geográfico? 
A hierarquia de país/região utilizada pelo Gestor de tráfego pode ser encontrada [aqui](traffic-manager-geographic-regions.md). Embora esta página é mantida atualizada com todas as alterações, através de programação também pode obter as mesmas informações utilizando o [API de REST do Gestor de tráfego do Azure](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Como é que o Gestor de tráfego determina a onde um usuário é a consulta de? 
O Gestor de tráfego analisa o IP de origem da consulta (isso provavelmente é um resolvedor DNS local, fazer a consulta em nome do utilizador) e utiliza um IP interno com o mapa de região para determinar a localização. Este mapa é atualizado de forma contínua para levar em conta as alterações na internet. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Garantia de que o Gestor de tráfego pode determinar a localização geográfica exata do utilizador em todos os casos corretamente?
Não, o Gestor de tráfego não pode garantir que a região geográfica que inferência a partir do endereço IP de origem de uma consulta DNS sempre corresponderá à localização do utilizador devido ao seguinte: 

- Em primeiro lugar, conforme descrito nas FAQ anteriores, o endereço IP de origem, vemos é que um resolvedor DNS, fazendo a pesquisa em nome do utilizador. Embora a localização geográfica de resolução de DNS seja um bom proxy para a localização geográfica do utilizador, também pode ser diferente, consoante os requisitos de espaço do serviço de resolução DNS e o serviço específico do resolvedor DNS optado por utilizar um cliente. Por exemplo, um cliente localizado na Malásia poderia especificar em utilização de definições do respetivo dispositivo, um serviço de resolução DNS cujo servidor DNS em Singapura poderá obter escolhido para lidar com as resoluções de consulta para esse utilizador/dispositivo. Nesse caso, o Gestor de tráfego só pode ver endereço IP do resolvedor que corresponde à localização Singapura. Além disso, veja as FAQ anteriores sobre suporte de endereço de sub-rede de cliente nesta página.

- Em segundo lugar, o Gestor de tráfego utiliza um mapa interno para fazer o endereço IP a tradução de região geográfica. Embora este mapa é validado e atualizado de forma contínua para aumentar a precisão e em conta a natureza em evolução da internet, ainda há a possibilidade de que as informações não são uma representação exata da localização geográfica de todas as a IP endereços.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Um ponto final tem de estar fisicamente localizados na mesma região que aquela que está configurado com para encaminhamento geográfico? 
Não, a localização do ponto de extremidade impõe sem restrições em que regiões podem ser mapeadas para o mesmo. Por exemplo, um ponto de extremidade na região do Azure – centro dos EUA pode ter todos os utilizadores direcionados para ele na Índia.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Pode atribuir regiões geográficas para pontos finais num perfil que não está configurado para fazer o encaminhamento geográfico? 

Sim, se o método de encaminhamento de um perfil não é geográfico, pode utilizar o [API de REST do Gestor de tráfego do Azure](https://docs.microsoft.com/rest/api/trafficmanager/) atribuir regiões geográficas para pontos de extremidade daquele perfil. No caso de perfis de tipo de encaminhamento não geográfica, esta configuração é ignorada. Se alterar um perfil de tal para o tipo de encaminhamento geográfico num momento posterior, o Gestor de tráfego pode utilizar esses mapeamentos.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Por que eu ganho um erro ao tentar alterar o método de encaminhamento de um perfil existente para Geographic?

Todos os pontos finais num perfil com encaminhamento geográfico tem de ter pelo menos uma região mapeada para este. Para converter um perfil existente para o tipo de encaminhamento geográfico, primeiro tem de associar regiões geográficas para todos os seus pontos de extremidade com o [API de REST do Gestor de tráfego do Azure](https://docs.microsoft.com/rest/api/trafficmanager/) antes de alterar o tipo de encaminhamento para geográfica. Se utilizar o portal, elimine primeiro os pontos finais, alterar o método de encaminhamento do perfil para geográfica e, em seguida, adicione os pontos de extremidade, juntamente com o seu mapeamento de região geográfica. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Por que motivo, recomendamos vivamente que os clientes criar perfis aninhados em vez de pontos finais sob um perfil com o serviço de encaminhamento geográfico ativada? 

Uma região pode ser atribuída a apenas um ponto final de um perfil se sua com o tipo de encaminhamento geográfico. Se esse ponto final não é um tipo aninhado com um perfil de subordinados anexado, se o ponto de extremidade vai mau estado de funcionamento, de tráfego Manager continua a enviar tráfego para o mesmo desde a alternativa de não enviar qualquer tráfego não é muito melhor. O Gestor de tráfego efetua uma ativação pós-falha não para outro ponto final, mesmo quando a região atribuída é um "elemento principal" da região atribuído para o ponto final que ficou em mau estado de funcionamento (por exemplo, se um ponto de extremidade que tenha Espanha região ficar danificado, que podemos fazê-lo não ativação pós-falha para outro ponto final Tem a região que Europa atribuída ao mesmo). Isso é feito para garantir que o Gestor de tráfego respeita os limites geográficos que um cliente tem a configuração no seu perfil. Para obter o benefício de ativação pós-falha para outro ponto final num ponto de extremidade fica em mau estado de funcionamento, recomenda-se que regiões geográficas ser atribuída a perfis aninhados com vários pontos de extremidade dentro da mesma, em vez de pontos de extremidade individuais. Dessa forma, se falhar um ponto de extremidade no perfil de subordinados aninhados, o tráfego pode ativação pós-falha para outro ponto final dentro o mesmo perfil de subordinados aninhados.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Existem restrições sobre a versão de API que suporta este tipo de encaminhamento?

Sim, apenas a versão de API 2017-03-01 e a mais recente suporta o encaminhamento geográfico escreva. Quaisquer versões mais antigas de API não podem ser utilizado para criar perfis de tipo de encaminhamento geográfico ou atribuir regiões geográficas para pontos de extremidade. Se uma versão de API mais antiga é utilizada para obter os perfis de uma subscrição do Azure, não for devolvida a qualquer perfil de tipo de encaminhamento geográfico. Além disso, ao usar as versões mais antigas de API, a qualquer perfil devolveu que tem pontos finais com uma atribuição de região geográfica, não tem a sua atribuição de região geográfica mostrada.

## <a name="real-user-measurements"></a>Medidas Reais de Utilizadores

### <a name="what-are-the-benefits-of-using-real-user-measurements"></a>Quais são as vantagens da utilização de medidas de utilizadores reais?
Quando utiliza o método de encaminhamento de desempenho, o Gestor de tráfego escolhe a melhor região do Azure para o utilizador final ligar ao inspecionar o IP de origem e a sub-rede de cliente de EDNS (se passado) e verificá-la em relação a análise da latência de rede o serviço mantém. Medidas de utilizadores reais melhora a isso para a sua base de utilizadores finais fazendo com que sua experiência de contribuir para esta tabela de latência, além de garantir que esta tabela abrange adequadamente as redes de utilizador final na qual os utilizadores finais liga para o Azure. Isso nos leva a uma maior precisão no encaminhamento dos seus utilizadores finais.

### <a name="can-i-use-real-user-measurements-with-non-azure-regions"></a>Pode utilizar medidas de utilizadores reais com regiões de não pertencente ao Azure?
Medidas de medições de utilizador real e relatórios sobre apenas a latência para chegar a regiões do Azure. Se estiver a utilizar o encaminhamento com o botão com base no desempenho com pontos finais alojados em regiões de não pertencente ao Azure, ainda poderá se beneficiar desta funcionalidade por ter maior latência obter informações sobre a região do Azure representativa tenha selecionado a ser associado este ponto final.

### <a name="which-routing-method-benefits-from-real-user-measurements"></a>Que método de encaminhamento beneficia de medições de utilizador Real?
As informações adicionais, obtidas por meio de medidas de utilizadores reais são aplicáveis apenas para perfis que utilizam o método de encaminhamento de desempenho. Tenha em atenção que a ligação de medidas de utilizadores reais está disponível em todos os perfis quando vê-lo através do portal do Azure.

### <a name="do-i-need-to-enable-real-user-measurements-each-profile-separately"></a>É necessário permitir medidas de utilizadores reais cada perfil separadamente?
Não, só tem de ativá-lo uma vez por subscrição e todas as informações de latência medido e comunicadas estão disponíveis para todos os perfis.

### <a name="how-do-i-turn-off-real-user-measurements-for-my-subscription"></a>Como desativar as medições de utilizador reais para a minha subscrição?
Pode parar a acumulação de encargos relacionados com medidas de utilizadores reais quando parar a recolha e o envio medidas de latência de back-a partir da sua aplicação de cliente. Por exemplo, quando a medida JavaScript incorporadas em páginas da web, pode deixar de utilizar esta funcionalidade ao remover o JavaScript ou desativando sua invocação quando a página é processada.

Também pode desativar a medidas de utilizadores reais, eliminando a sua chave. Depois de eliminar a chave, qualquer enviadas para o Gestor de tráfego com essa chave de medições são eliminadas.

### <a name="can-i-use-real-user-measurements-with-client-applications-other-than-web-pages"></a>Pode utilizar medidas de utilizadores reais com aplicações de cliente que não seja a páginas da web?
Sim, as medidas de utilizadores reais foi concebida para absorver dados recolhidos através de um tipo diferente de clientes do utilizador final. Estas FAQ será atualizado como obterem suporte a novos tipos de aplicativos de cliente.

### <a name="how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered"></a>Quantas medições efetuadas sempre que meu medidas de utilizadores reais ativada a página da web é processada?
Quando as medições de utilizador Real é utilizada com a medição JavaScript fornecido, processamento de cada página resulta em seis medidas que está a ser executadas. Estes são, em seguida, relatados ao serviço Gestor de tráfego. Tenha em atenção que lhe são cobrados esse recurso com base no número de medidas comunicou o serviço de Gestor de tráfego. Por exemplo, se o utilizador navega para fora de sua página da Web, enquanto estão a ser tomadas as medidas, mas antes de ele foi relatado, essas medidas não são consideradas para efeitos de faturação.

### <a name="is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage"></a>Existe um atraso antes de executar script de medidas de utilizadores reais em minha página da Web?
Não, não existe nenhum atraso programado antes do script for chamado.

### <a name="can-i-use-configure-real-user-measurements-with-only-the-azure-regions-i-want-to-measure"></a>Pode utilizar configurar medidas de utilizadores reais com apenas as regiões do Azure, desejo medir?
Não, cada vez que é invocado, o script de medidas de utilizadores reais mede a um conjunto de seis regiões do Azure, conforme determinado pelo serviço. Esta definição definida alterações entre diferentes invocações e quando um grande número de invocações de tais acontece, a cobertura de medida se expande entre diferentes regiões do Azure.

### <a name="can-i-limit-the-number-of-measurements-made-to-a-specific-number"></a>Pode limitar o número de medidas feita para um número específico?
A medida de JavaScript está embutido na sua página da Web e está controlo total sobre quando a início e fim de usá-lo. Desde que o serviço de Gestor de tráfego recebe um pedido para obter uma lista de regiões do Azure para ser medido, um conjunto de regiões são devolvidos.

### <a name="can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements"></a>Pode ver as medidas tomadas pelo meu aplicativo de cliente como parte de medidas de utilizadores reais?
Uma vez que a lógica de medida é executada a partir da sua aplicação de cliente, está no controle total sobre o que acontece, incluindo ver as medidas de latência. O Gestor de tráfego não comunica uma visualização agregada das medidas recebido sob a chave associada à sua subscrição.

### <a name="can-i-modify-the-measurement-script-provided-by-traffic-manager"></a>Pode modificar o script de medição fornecido pelo Gestor de tráfego?
Enquanto estiver no controle do que é incorporado na sua página web, podemos vivamente desestimulá-lo de efetuar quaisquer alterações no script de medição para garantir que ele mede e relata as latências corretamente.

### <a name="will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements"></a>Vai ser possível que outros possam ver a chave de que uso com medidas de utilizadores reais?
Ao incorporar o script de medição para uma página da web é possível que outros possam ver o script e a chave de medições de utilizador Real (RUM). Mas é importante saber que esta chave é diferente do seu id de subscrição e é gerada pelo Gestor de tráfego a ser utilizado apenas para essa finalidade. Saber a sua chave de executar não comprometerá a segurança da conta do Azure.

### <a name="can-others-abuse-my-rum-key"></a>Outras pessoas abusar minha chave executar?
Embora seja possível que outras pessoas para utilizar a sua chave para enviar informações erradas para o Azure tenha em atenção de que algumas medidas erradas não irão alterar o encaminhamento, uma vez que ele é levado em conta, juntamente com todas as outras medidas que recebemos. Se precisar de alterar as suas chaves, novamente é possível gerar a chave de ponto em que a chave antiga torna-se descartado.

###  <a name="do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages"></a>É necessário colocar a medição do JavaScript em todas as minhas páginas web?
Medidas de utilizadores reais oferece mais valor como o número de aumento de medidas. Tendo dito isso, é a decidir se precisa de colocá-lo em todas as suas páginas da web ou um, selecione poucos. A nossa recomendação é iniciar, colocando-o na sua página mais visitada em que um utilizador é esperado para se manter na página de cinco segundos ou mais.

### <a name="can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements"></a>Podem obter informações sobre os meus utilizadores finais ser identificadas pelo Gestor de tráfego se eu utilizar medidas de utilizadores reais?
Quando é utilizada a medição fornecida JavaScript, o Gestor de tráfego irá tem visibilidade para o endereço IP de cliente do utilizador final e o endereço IP de origem do resolvedor de DNS local que utilizam. Gestor de tráfego utiliza o endereço IP do cliente apenas depois de ter o texto para não ser capaz de identificar o utilizador final específico que enviou as medidas. 

### <a name="does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing"></a>É a página Web medição medidas de utilizadores reais precisa de estar a utilizar o Gestor de tráfego para o encaminhamento?
Não, ele não precisa de utilizar o Gestor de tráfego. O lado de encaminhamento do Gestor de tráfego opera em separado da parte de medição de utilizador Real e embora seja uma boa idéia ter ambos na mesma propriedade web, não precisam de ser.

### <a name="do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements"></a>É necessário hospedar qualquer serviço em regiões do Azure para utilizar com medidas de utilizadores reais?
Não, não precisa alojar qualquer componente do lado do servidor no Azure para medidas de utilizadores reais trabalhar. A imagem de pixel único transferida pela medição de JavaScript e o serviço de executá-lo em diferentes regiões do Azure é alojado e gerido pelo Azure. 

### <a name="will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements"></a>Meu uso de largura de banda do Azure irá aumentar quando posso utilizar medidas de utilizadores reais?
Conforme mencionado na resposta anterior, os componentes do lado do servidor de medidas de utilizadores reais são propriedade e são geridos pelo Azure. Isso significa que não irá aumentar a utilização de largura de banda do Azure porque utiliza medidas de utilizadores reais. Tenha em atenção que, isso não inclui qualquer utilização de largura de banda fora em que custos do Azure. Vamos minimizar a largura de banda utilizada ao transferir apenas uma imagem de pixel único para a medida a latência para uma região do Azure. 

## <a name="traffic-view"></a>Vista de Tráfego

### <a name="what-does-traffic-view-do"></a>O que faz a vista de tráfego?
Vista de tráfego é uma funcionalidade do Gestor de tráfego que o ajuda a compreender melhor os seus utilizadores e como é a experiência deles. Ele usa as consultas recebidas pelo Gestor de tráfego e as tabelas de inteligência de latência de rede que o serviço mantém para lhe fornecer o seguinte:
- As regiões a partir de onde estão a ligar os seus utilizadores para os pontos finais no Azure.
- O volume de utilizadores a ligar a partir dessas regiões.
- Regiões do Azure para o qual obter roteados aos.
- Sua experiência de latência para estas regiões do Azure.

Essas informações estão disponíveis para poder consumir por meio de sobreposição de mapa geográfico e modos de exibição em tabela no portal do além de estarem disponíveis como dados não processados para transferência.

### <a name="how-can-i-benefit-from-using-traffic-view"></a>Como pode beneficiar da utilização de vista de tráfego?

Vista de tráfego dá-lhe a visão geral do tráfego a receber de seus perfis do Gestor de tráfego. Em particular, ele pode ser usado para compreender onde a sua base de usuários liga-se do e igualmente importante é que o que é a experiência deles latência média. Em seguida, pode utilizar estas informações para localizar as áreas em que precisa se concentrar, por exemplo, ao expandir os seus requisitos de espaço do Azure para uma região que possa servir os utilizadores com uma latência mais baixa. Outra informação que pode derivar de utilizar a vista de tráfego é ver os padrões de tráfego para regiões diferentes, que por sua vez pode ajudá-lo a tomar decisões sobre aumenta ou reduz inventar nessas regiões.

### <a name="how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor"></a>Como a vista de tráfego é diferente das métricas de Gestor de tráfego disponíveis através do Azure monitor?

O Azure Monitor pode ser utilizado para compreender ao nível da agregação o tráfego recebido pelo seu perfil e seus pontos de extremidade. Ele também permite-lhe controlar o estado de funcionamento dos pontos finais, expondo os resultados da verificação de estado de funcionamento. Quando tiver de ir para além destas e compreender a experiência do utilizador final a ligar ao Azure a um nível regional, a vista de tráfego pode ser utilizada para esse efeito.

### <a name="does-traffic-view-use-edns-client-subnet-information"></a>O Vista de tráfego usa as informações de sub-rede de cliente EDNS?

As consultas DNS servidas pelo Gestor de tráfego do Azure, considere informações ECS para aumentar a precisão do encaminhamento. Mas, ao criar o conjunto de dados que mostra onde os usuários estão se conectando, a vista de tráfego está a utilizar apenas o endereço IP de resolução de DNS.

### <a name="how-many-days-of-data-does-traffic-view-use"></a>O número de dias de dados utilizar a vista de tráfego?

Vista de tráfego cria sua saída ao processar os dados dos sete dias antes do dia anterior quando for visualizado por si. Esta é uma janela móvel e os dados mais recentes serão utilizados sempre que visitá.

### <a name="how-does-traffic-view-handle-external-endpoints"></a>Como a vista de tráfego com pontos finais externos?

Quando utilizar pontos finais externos hospedados fora do regiões do Azure num perfil do Gestor de tráfego pode optar por ela está mapeada para uma região do Azure que é um proxy para suas características de latência (isso é na verdade necessária se utilizar o método de encaminhamento de desempenho). Se tiver este mapeamento de região do Azure, as métricas de latência nessa região do Azure serão utilizadas ao criar a vista de tráfego de saída. Não se for especificada nenhuma região do Azure, as informações de latência estará vazias nos dados para os pontos finais externos.

### <a name="do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription"></a>É necessário ativar a vista de tráfego para cada perfil em minha assinatura?

Durante o período de pré-visualização, a vista de tráfego foi ativada ao nível da subscrição. Como parte das melhorias feitas antes da disponibilidade geral, pode agora ativar a vista de tráfego num nível de perfil, permitindo que tenha mais granular, permitindo desta funcionalidade. Por predefinição, a vista de tráfego será desativada por um perfil.

>[!NOTE]
>Se ativou a vista de tráfego ao nível da subscrição durante o período de pré-visualização, terá agora a reativá-la para cada perfil nessa subscrição.
 
### <a name="how-can-i-turn-off-traffic-view"></a>Como posso desativar a vista de tráfego? 
Pode desativar a vista de tráfego para qualquer perfil com o Portal ou a REST API. 

### <a name="how-does-traffic-view-billing-work"></a>Como funciona a faturação da vista de tráfego?

Preços de vista de tráfego é baseado no número de pontos de dados utilizado para criar a saída. Atualmente, o tipo de dados apenas suportado é as consultas que recebe o seu perfil. Além disso, apenas são faturadas para o processamento que foi feito quando tiver ativado a vista de tráfego. Isso significa que, se ativar vista de tráfego para um determinado período de tempo num mês e desativá-lo durante a outras vezes, apenas os pontos de dados processados enquanto tiver a funcionalidade de número de ativações em direção à sua fatura.

## <a name="traffic-manager-endpoints"></a>Pontos finais do Gestor de tráfego

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Pode utilizar o Gestor de tráfego com pontos de extremidade de várias subscrições?

Utilizar pontos de extremidade de várias subscrições não é possível com aplicações Web do Azure. Aplicações Web do Azure requer que qualquer nome de domínio personalizado utilizado com as aplicações Web só é utilizada numa única subscrição. Não é possível utilizar as aplicações Web de várias subscrições com o mesmo nome de domínio.

Para outros tipos de ponto final, é possível utilizar o Gestor de tráfego com pontos de extremidade de mais de uma assinatura. No Gestor de recursos, pontos finais de qualquer subscrição podem de ser adicionados ao Gestor de tráfego, desde que a pessoa que configurar o perfil do Gestor de tráfego tenha acesso de leitura para o ponto final. Estas permissões podem ser concedidas usando [do Azure Resource Manager controlo de acesso baseado em funções (RBAC)](../role-based-access-control/role-assignments-portal.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Pode utilizar o Gestor de tráfego com blocos de "Transição" do serviço em nuvem?

Sim. Serviço em nuvem blocos de "teste" pode ser configurado no Traffic Manager como pontos finais externos. Verificações de estado de funcionamento são continua a ser cobrado à tarifa de pontos finais do Azure. Porque o tipo de ponto final externo está em utilização, as alterações ao serviço subjacente não são aplicadas automaticamente. Com pontos finais externos, o Gestor de tráfego não consegue detetar quando o serviço em nuvem é parado ou eliminado. Por conseguinte, o Gestor de tráfego continua a faturação de verificações de estado de funcionamento até que o ponto final está desativado ou eliminado.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>O Gestor de tráfego suporta pontos finais IPv6?

Atualmente, o Gestor de tráfego não fornece IPv6 addressible servidores de nomes. No entanto, o Gestor de tráfego ainda podem ser utilizado pelos clientes de IPv6 ligar pontos finais IPv6. Um cliente não faz a pedidos DNS diretamente para o Gestor de tráfego. Em vez disso, o cliente utiliza um serviço DNS recursivo. Um cliente apenas de IPv6 envia pedidos ao serviço DNS recursivo através de IPv6. Em seguida, o serviço de recursiva deve ser capaz de contactar os servidores de nomes do Gestor de tráfego utilizando IPv4.

O Gestor de tráfego responde com o nome DNS do ponto de extremidade. Para oferecer suporte a um ponto de extremidade do IPv6, tem de existir um registo de DNS AAAA apontar o nome DNS do ponto final para o endereço IPv6. Verificações de estado de funcionamento do Gestor de tráfego suportam apenas endereços IPv4. O serviço precisa expor um ponto de extremidade de IPv4 no mesmo nome DNS.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Pode utilizar o Gestor de tráfego com mais do que uma aplicação Web na mesma região?

Normalmente, o Gestor de tráfego é utilizado para direcionar o tráfego para aplicações implementadas em regiões diferentes. No entanto, também pode ser utilizado em que um aplicativo tem mais de uma implementação na mesma região. Pontos finais do Azure do Gestor de tráfego que não permitem mais do que um ponto final de aplicação Web da mesma região do Azure para ser adicionado para o mesmo perfil de Gestor de tráfego.

### <a name="how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group"></a>Como posso mover pontos finais do meu perfil Gestor de tráfego do Azure a um grupo de recursos diferente?

Pontos finais do Azure que estão associados um perfil do Gestor de tráfego são controlados usando seus IDs de recurso. Quando um recurso do Azure que está a ser utilizado como um ponto final (por exemplo, IP público, clássico de Cloud de serviço, aplicação Web ou outro perfil do Traffic Manager utilizado de forma aninhada) é movido para um grupo de recursos diferente, as alterações do mesmo ID de recurso. Neste cenário, atualmente, tem de atualizar o perfil do Gestor de tráfego por eliminar primeiro e, em seguida, adição de volta os pontos finais para o perfil. 

##  <a name="traffic-manager-endpoint-monitoring"></a>Monitorização de pontos finais do Gestor de tráfego

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>O Gestor de tráfego é resiliente a falhas de região do Azure?

Gestor de tráfego é um componente fundamental de entrega de aplicações altamente disponíveis no Azure.
Para assegurar elevada disponibilidade, o Gestor de tráfego tem de ter um excepcionalmente alto nível de disponibilidade e ser resiliente a falhas regionais.

Por predefinição, os componentes do Gestor de tráfego são resilientes a uma falha total de qualquer região do Azure. Este resiliência se aplica a todos os componentes do Gestor de tráfego: os nomes do DNS servidores, a API, a camada de armazenamento e o ponto final de serviço de monitorização.

Na improvável eventualidade de uma indisponibilidade de toda uma região do Azure, o Gestor de tráfego é esperado que continua a funcionar normalmente. Aplicações implementadas em várias regiões do Azure podem se basear no Gestor de tráfego para direcionar o tráfego para uma instância disponível de seu aplicativo.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Como a opção de localização do grupo de recursos afeta o Gestor de tráfego?

Gestor de tráfego é um único serviço global. Não é regional. A escolha da localização do grupo de recursos é indiferente para perfis de Gestor de tráfego implementados nesse grupo de recursos.

O Azure Resource Manager requer que todos os grupos de recursos especificar uma localização, que determina a localização predefinida para recursos implementados nesse grupo de recursos. Quando cria um perfil do Gestor de tráfego, é criado num grupo de recursos. Utilizam todos os perfis do Gestor de tráfego **global** como a localização, substituir a predefinição do grupo de recursos.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Como posso determinar o estado de funcionamento atual de cada ponto final?

O estado de monitorização atual de cada ponto de extremidade, além do perfil de geral, é apresentado no portal do Azure. Essas informações também estão disponíveis através do Monitor de tráfego [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [cmdlets do PowerShell](https://msdn.microsoft.com/library/mt125941.aspx), e [CLI do Azure de várias plataformas](../cli-install-nodejs.md).

Também pode utilizar o Azure Monitor para controlar o estado de funcionamento dos seus pontos finais e ver uma representação visual dos mesmos. Para obter mais informações sobre como utilizar o Azure Monitor, consulte a [documentação de monitorização do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics).

### <a name="can-i-monitor-https-endpoints"></a>Pode monitorizar pontos finais HTTPS?

Sim. Gestor de tráfego suporta pesquisa através de HTTPS. Configurar **HTTPS** como o protocolo na configuração da monitorização.

O Gestor de tráfego não pode fornecer qualquer validação de certificado, incluindo:

* Certificados de servidor não são validados.
* Certificados de servidor SNI não são suportados
* Certificados de cliente não são suportados

### <a name="i-stopped-an-azure-cloud-service--web-application-endpoint-in-my-traffic-manager-profile-but-i-am-not-receiving-any-traffic-even-after-i-restarted-it-how-can-i-fix-this"></a>Parado a um serviço cloud do Azure / ponto final da aplicação no meu perfil do Gestor de tráfego da web, mas não estou a receber qualquer tráfego, mesmo depois de eu reiniciado-lo. Como posso corrigir esta situação?

Quando um Azure serviço em nuvem / ponto final da aplicação da web está parada paradas de Gestor de tráfego a verificar o respetivo estado de funcionamento e reinicia as verificações de estado de funcionamento apenas depois de detetar que o ponto final tem de ser reiniciados. Para evitar este atraso, desativar e reativar, em seguida, o ponto de extremidade no perfil do Gestor de tráfego depois de reiniciar o ponto final.   

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Posso utilizar o Gestor de tráfego, mesmo que meu aplicativo não tem suporte para HTTP ou HTTPS?

Sim. Pode especificar TCP como protocolo de monitorização e o Gestor de tráfego pode iniciar uma ligação de TCP e aguardar por uma resposta do ponto final. Se o ponto final de responder ao pedido de ligação com uma resposta para estabelecer a ligação, dentro do período de tempo limite, esse ponto final está marcado como em bom estado.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>As respostas que específicos são necessárias a partir do ponto final ao utilizar a monitorização de TCP?

Quando é utilizado o monitoramento de TCP, o Gestor de tráfego é iniciado um handshake TCP de três vias ao enviar um pedido SYN ao ponto final na porta especificada. Em seguida, aguarda um período de tempo (conforme especificado nas definições de tempo limite) por uma resposta do ponto final. Se o ponto final de responder ao pedido de SYN com uma resposta de SYN-ACK dentro do período de tempo limite especificado nas definições de monitorização, em seguida, esse ponto final é considerado em bom estado. Se a resposta de SYN-ACK é recebida, o Gestor de tráfego repõe a ligação ao responder com um RST.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>A rapidez é que o Gestor de tráfego mover os meus utilizadores para fora de um ponto de extremidade de mau estado de funcionamento?

Gestor de tráfego oferece várias definições que podem ajudar a controlar o comportamento de ativação pós-falha do seu perfil do Gestor de tráfego da seguinte forma:
- Pode especificar que o Gestor de tráfego sondas os pontos finais com mais frequência ao definir o intervalo de pesquisa em 10 segundos. Isto garante que qualquer ponto final vai mau estado de funcionamento pode ser detetada logo que possível. 
- Pode especificar o período de tempo de espera antes de um Estado de funcionamento conferir tempos de pedido (o valor mínimo de tempo de limite é 5 s).
- Pode especificar o número de falhas pode ocorrer antes do ponto final está marcado como mau estado de funcionamento. Este valor pode ser um valor baixo como 0, nesse caso o ponto final está marcado mau estado de funcionamento quando ocorre uma falha para a primeira verificação de estado de funcionamento. No entanto, usando o valor mínimo de 0 para o número de falhas tolerado pode levar a que está a ser retirados da rotação devido a problemas transitórios que podem ocorrer no momento da pesquisa de pontos de extremidade.
- Pode especificar o time-to-live (TTL) para a resposta DNS ser tão baixo como 0. Isso significa que resoluções DNS não é possível colocar em cache a resposta e a cada nova consulta obtém uma resposta que incorpora as informações mais atualizadas do Estado de funcionamento com o Gestor de tráfego.

Ao utilizar estas definições, o Gestor de tráfego pode fornecer as ativações pós-falha de menos de 10 segundos depois de um ponto de extremidade fica em mau estado de funcionamento e é feita uma consulta DNS contra o perfil correspondente.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Como posso especificar diferentes definições de monitorização para diferentes pontos de extremidade num perfil?

Gestor de tráfego, as definições de monitorização estão num por nível de perfil. Se precisar de utilizar uma configuração de monitorização diferente para apenas um ponto final, pode ser feito ao ter o ponto de extremidade como uma [aninhados perfil](traffic-manager-nested-profiles.md) cujas definições de monitorização são diferentes do perfil de principal.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>O estado de funcionamento de ponto final de fazer de cabeçalho do anfitrião verifica utilização?

O Gestor de tráfego utiliza cabeçalhos de host HTTP e HTTPS verificações de estado de funcionamento. O cabeçalho de anfitrião utilizado pelo Gestor de tráfego é o nome do destino do ponto final configurado no perfil. O valor utilizado no cabeçalho de anfitrião não pode ser especificado em separado da propriedade de destino.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Quais são os endereços IP a partir do qual o estado de funcionamento verifica se originam?

Clique em [aqui](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json) para ver o ficheiro JSON que lista os endereços IP a partir do qual o Gestor de tráfego verificações de estado de funcionamento podem ser originados. Reveja os IPs listados no ficheiro JSON para se certificar de que são permitidas ligações de entrada estes endereços IP, os pontos de extremidade para verificar o estado de funcionamento.

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Quantas verificações de estado de funcionamento ao meu ponto final posso esperar do Gestor de tráfego?

O número de estado de funcionamento do Gestor de tráfego verifica a atingir o ponto final depende o seguinte:
- o valor que definiu para o intervalo de monitorização (intervalo menor significa mais pedidos de destino no seu ponto final em qualquer determinado período de tempo).
- o número de localizações a partir de onde as verificações de estado de funcionamento se originam (os endereços IP de onde pode esperar estas verificações está listado nas perguntas frequentes anteriores).

### <a name="how-can-i-get-notified-if-one-of-my-endpoints-goes-down"></a>Como posso ser notificado da se um dos meus pontos finais de ficar inativo? 
Uma das métricas fornecidas pelo Gestor de tráfego é o estado de funcionamento dos pontos finais num perfil. Pode ver isso como um agregado de todos os pontos finais dentro de um perfil (por exemplo, 75% dos seus pontos finais estão em bom estado), ou, num por nível de ponto final. Métricas de Gestor de tráfego são expostas através do Azure Monitor e pode utilizar o seu [capacidades de alerta](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) para receber notificações quando houver uma alteração no estado do ponto final do Estado de funcionamento. Para obter mais detalhes, consulte [Gestor de tráfego métricas e alertas](traffic-manager-metrics-alerts.md).  

## <a name="traffic-manager-nested-profiles"></a>O Gestor de tráfego aninhados perfis

### <a name="how-do-i-configure-nested-profiles"></a>Como posso configurar perfis aninhados?

Perfis aninhados do Gestor de tráfego podem ser configurados com o Azure Resource Manager e o clássico APIs de REST de Azure, cmdlets do PowerShell do Azure e os comandos da CLI do Azure de várias plataformas. Também são suportadas através do novo portal do Azure.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Como muitas camadas de faz o ninho tráfego Manager suportam?

Pode aninhar perfis até 10 níveis de profundidade. "Faz um loop" não é permitido.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Pode combinar outros tipos de ponto final com perfis de subordinados aninhados, no mesmo perfil de Gestor de tráfego?

Sim. Não há restrições sobre como combinar pontos finais de tipos diferentes de um perfil.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Como o modelo de faturação se aplica para aninhados perfis?

Não é o impacto da utilização de perfis aninhados de preço não negativo.

A faturação do Gestor de tráfego tem dois componentes: as verificações de estado de funcionamento do ponto final e milhões de consultas DNS

* As verificações de estado de funcionamento do ponto final: não implica custos para um perfil de subordinado quando configurado como um ponto final num perfil principal. Monitorização de pontos de extremidade no perfil subordinado é faturada como habitualmente.
* Consultas DNS: cada consulta só é contabilizada uma vez. Uma consulta em relação a um principal que retorna um ponto final de um perfil de subordinado é contabilizada face apenas para perfis do pai.

Para mais informações, consulte a [Gestor de tráfego, página de preços](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Existe um impacto de desempenho para aninhados perfis?

Não. Não há nenhum impacto no desempenho sujeito ao utilizar perfis aninhados.

Os servidores de nomes do Gestor de tráfego atravessam a hierarquia de perfil internamente durante o processamento de cada consulta DNS. Uma consulta DNS para um perfil de principal pode receber uma resposta DNS com um ponto final de um perfil de subordinados. Um único registo CNAME é utilizado se estiver a utilizar um único perfil ou perfis aninhados. Não é necessário para criar um registo CNAME para cada perfil na hierarquia.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Como é que o Gestor de tráfego computação o estado de funcionamento de um ponto final aninhado num perfil principal?

O perfil de principal não executa verificações de estado de funcionamento no filho diretamente. Em vez disso, o estado de funcionamento dos pontos finais do perfil de subordinados são utilizados para calcular o estado de funcionamento geral do perfil subordinado. Esta informação será propagada a hierarquia de perfil aninhada para determinar o estado de funcionamento do ponto final aninhado. O perfil de principal utiliza este estado de funcionamento agregado para determinar se o tráfego pode ser direcionado para o filho.

A tabela seguinte descreve o comportamento do Gestor de tráfego verificações de estado de funcionamento para um ponto final aninhado.

| Estado do Monitor de perfil do secundário | Estado do Monitor do ponto final principal | Notas |
| --- | --- | --- |
| Desativado. O perfil de subordinado foi desativado. |Parada |O estado de ponto final principal está parado, não desativado. O estado desativado está reservado para indicar que tiver desativado o ponto final no perfil de principal. |
| Degradado. Ponto final do perfil, pelo menos, um subordinado está num Estado Degraded. |Online: o número de pontos finais Online no perfil subordinado, pelo menos, é o valor de MinChildEndpoints.<BR>CheckingEndpoint: o número de pontos finais Online plus CheckingEndpoint no perfil subordinado, pelo menos, é o valor de MinChildEndpoints.<BR>Degradado: caso contrário. |O tráfego é encaminhado para um ponto final do Estado CheckingEndpoint. Se MinChildEndpoints for definida muito alta, o ponto final sempre está degradado. |
| Online. Ponto final do perfil, pelo menos, um subordinado é o Estado Online. Nenhum ponto final está no Estado Degraded. |Consulte acima. | |
| CheckingEndpoints. Ponto final do perfil, pelo menos, um subordinado é 'CheckingEndpoint'. Não existem pontos finais são "Online" ou "Degradado" |Mesmo que acima. | |
| Inativos. Todos os pontos de extremidade de perfil de subordinados são desativado ou parado ou este perfil não tem pontos finais. |Parada | |

## <a name="next-steps"></a>Passos seguintes:
- Saiba mais sobre o Gestor de tráfego [ativação pós-falha automática e monitorização do ponto de extremidade](../traffic-manager/traffic-manager-monitoring.md).
- Saiba mais sobre o Gestor de tráfego [métodos de encaminhamento de tráfego](../traffic-manager/traffic-manager-routing-methods.md).
