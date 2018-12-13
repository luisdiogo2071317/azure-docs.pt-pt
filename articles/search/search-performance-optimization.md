---
title: O Azure Search desempenho e otimização considerações - Azure Search
description: Aprenda técnicas e práticas recomendadas para ajustar o desempenho de pesquisa do Azure e configurar o dimensionamento ideal.
author: LiamCavanagh
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/01/2017
ms.author: liamca
ms.custom: seodec2018
ms.openlocfilehash: 0a98e7f05e766d47a5ea9293409a74a6fafbf837
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310260"
---
# <a name="azure-search-performance-and-optimization-considerations"></a>Considerações do Azure de desempenho e otimização de pesquisa
Uma experiência de pesquisa fantásticas é uma chave para aplicações web e de sucesso para dispositivos móveis muitos. De espaço real, a utilizado marketplaces de carro para catálogos online, o fast search e resultados relevantes irão afetar a experiência do cliente. Este documento destina-se para obter ajuda a detetar o suportam de práticas recomendadas para saber como tirar o máximo partido do Azure Search, especialmente para cenários avançados com requisitos sofisticados para escalabilidade, vários idiomas ou classificação personalizada.  Além disso, este documento descreve os elementos internos e abrange as abordagens que funcionam com eficiência em aplicativos de clientes reais.

## <a name="performance-and-scale-tuning-for-search-services"></a>Desempenho e otimização de dimensionamento para serviços de pesquisa
Nós estamos acostumados mecanismos como o Bing e a Google e o elevado desempenho que oferecem de pesquisa.  Como resultado, quando os clientes utilizam a sua capacidade de pesquisa web ou aplicações móveis, eles esperam características de desempenho semelhantes.  Ao otimizar o desempenho de pesquisa, uma das abordagens melhores é enfocar a latência, o que é o tempo que uma consulta demora a concluir e devolver resultados.  Quando a Otimização da latência de pesquisa, é importante:

1. Escolher uma latência de destino (ou a quantidade máxima de tempo) que uma pesquisa típica do pedido deve demorar para concluir.
2. Criar e testar uma carga de trabalho real em relação a seu serviço de pesquisa com um conjunto de dados realista para medir estas tarifas de latência.
3. Comece com um número reduzido de consultas por segundo (QPS) e continuar a aumentar o número executado no teste, até que a latência da consulta passa a ser inferior a latência de destino definidos.  Este é um parâmetro de comparação importante para ajudar a planear para dimensionamento à medida que aumenta a aplicação em utilização.
4. Sempre que possível, reutilize ligações HTTP.  Se estiver a utilizar o SDK .NET da Azure Search, isso significa que deve reutilizar uma instância ou [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) instância, e se estiver a utilizar a API REST, deve reutilizar um HttpClient único.

Ao criar essas cargas de trabalho de teste, existem algumas características do Azure Search para ter em mente:

1. É possível enviar por push para a pesquisa de muitas consultas em simultâneo, que os recursos disponíveis no seu serviço de pesquisa do Azure irão ficar sobrecarregados.  Quando isto acontecer, verá códigos de resposta de HTTP 503.  Por esse motivo, é melhor começar com vários intervalos de pedidos de pesquisa para ver as diferenças nas taxas de latência, à medida que adiciona mais pedidos de pesquisa.
2. Carregamento de conteúdo para o Azure Search irá afetar o desempenho geral e a latência do serviço Azure Search.  Se esperar enviar dados, enquanto os utilizadores estão a efetuar pesquisas, é importante levar em conta esta carga de trabalho em seus testes.
3. Nem todas as consultas de pesquisa executará os mesmos níveis de desempenho.  Por exemplo, uma sugestão de pesquisa ou de pesquisa de documento, normalmente, irá efetuar mais rapidamente do que uma consulta com um número significativo de facetas e filtros.  É melhor assumir várias consultas que esperam para ver em consideração ao criar seus testes.  
4. Variação de pedidos de pesquisa é importante porque se executar continuamente os mesmos pedidos de pesquisa, a cache de dados começará a fazer do desempenho parecer melhor que ele poderá com uma consulta mais diferentes definido.

> [!NOTE]
> [Teste de carga do Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) é uma excelente maneira de executar o benchmark testa, pois permite-lhe executar pedidos HTTP que seria necessário para executar consultas no Azure Search e permite a paralelização de pedidos.
> 
> 

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Dimensionamento do Azure Search para as taxas de consulta elevado e limitado de pedidos
Quando está recebendo demasiados pedidos limitados ou excede as taxas de latência de destino de uma carga maior de consulta, pode consultar para diminuir as taxas de latência de uma de duas formas:

1. **Aumente as réplicas:**  Uma réplica é como uma cópia dos seus dados, permitindo que o Azure Search carregar pedidos de balanceamento em relação as várias cópias.  Tudo o balanceamento de carga e a replicação de dados em réplicas é gerida pelo Azure Search e é possível alterar o número de réplicas alocado para o seu serviço em qualquer altura.  Pode alocar até 12 réplicas num serviço de pesquisa padrão e 3 réplicas num serviço de pesquisa básica. As réplicas podem ser ajustadas a partir da [portal do Azure](search-create-service-portal.md) ou [PowerShell](search-manage-powershell.md).
2. **Aumente o escalão de pesquisa:**  O Azure Search é uma [número de camadas](https://azure.microsoft.com/pricing/details/search/) e cada uma dessas camadas oferece diferentes níveis de desempenho.  Em alguns casos, pode ter muitas consultas que a camada a que se estiver a utilizar não é possível fornecer as taxas de tão baixa latência, mesmo quando as réplicas são alcance o limite máximo.  Neste caso, poderá querer considerar a tirar partido de um dos escalões de pesquisa superior, como a camada de Azure Search S3 que é ideal para cenários com grande número de documentos e cargas de trabalho de consulta extremamente alta.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Dimensionamento do Azure Search para consultas individuais lentas
Outro motivo por que as taxas de latência podem ser lentas é a partir de uma única consulta demorar demasiado tempo a concluir.  Neste caso, a adicionar réplicas não irá melhorar as taxas de latência.  Neste caso, há duas opções disponíveis:

1. **Aumentar as partições** uma partição é um mecanismo para dividir os dados entre recursos extras.  Por esse motivo, quando adiciona uma segunda partição, é dividir os dados em duas.  Uma terceira partição divide o índice em três, etc.  Isso também tem o efeito que em alguns casos, consultas lentas terá um desempenho mais rápido devido a paralelização de computação.  Existem alguns exemplos de onde vimos essa paralelização funcionam muito bem com consultas com consultas de baixa seletividade.  Isso consiste em consultas que correspondam a muitos documentos ou quando facetamento precisa fornecer contagens ao longo de um grande número de documentos.  Uma vez que há muito de computação necessário para classificar a relevância dos documentos ou para contar o número de documentos, adicionar partições Extras pode ajudar a fornecer computação adicional.  
   
   Pode haver um máximo de 12 partições no serviço de pesquisa padrão e 1 partição no serviço de pesquisa básica.  As partições podem ser ajustadas a partir da [portal do Azure](search-create-service-portal.md) ou [PowerShell](search-manage-powershell.md).
2. **Campos de cardinalidade elevada de limite:** Um campo de cardinalidade elevada consiste num facetável ou o campo filtrável tem um número significativo de valores exclusivos, e que tenha como resultado, muitos recursos para calcular os resultados ao longo.   Por exemplo, definir um campo de ID do produto ou a descrição facetável/filtrável como faria para cardinalidade elevada porque a maioria dos valores do documento para documento é exclusiva. Sempre que possível, limite o número de campos de cardinalidade elevada.
3. **Aumente o escalão de pesquisa:**  Mover até um escalão mais elevado do Azure Search pode ser outra maneira de melhorar o desempenho das consultas lentas.  Cada escalão superior também fornece a CPU mais rápida e mais memória que pode ter um impacto positivo no desempenho de consulta.

## <a name="scaling-for-availability"></a>Dimensionamento para disponibilidade
Réplicas não só ajudam a reduzir a latência da consulta, mas também podem permitir para elevada disponibilidade.  Com uma única réplica, deve esperar periódico período de indisponibilidade devido a reinícios de servidor após as atualizações de software ou para outros eventos de manutenção que irão ocorrer.  Como resultado, é importante considerar se o seu aplicativo requer alta disponibilidade das pesquisas (consultas), bem como de escritas (indexação eventos).  O Azure Search oferece opções de SLA em todas as ofertas de pesquisa paga com os seguintes atributos:

* 2 réplicas para elevada disponibilidade de só de leitura cargas de trabalho (consultas)
* 3 ou mais réplicas para elevada disponibilidade de cargas de trabalho de leitura / escrita (consultas e indexação)

Para obter mais detalhes sobre isso, visite o [contrato de nível de serviço do Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Uma vez que as réplicas são cópias dos seus dados, ter várias réplicas permite o Azure Search para machine reinícios e manutenção com base numa réplica de cada vez, permitindo que consultas para continuar a ser executado em relação as outras réplicas.  Por esse motivo, também terá de considerar como este tempo de inatividade pode afetar as consultas que agora tem de ser executado contra um menos cópia dos dados.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Dimensionar cargas de trabalho distribuída geograficamente e fornecer redundância geográfica
Para cargas de trabalho distribuída geograficamente, descobrirá que os utilizadores que estão localizados longe do Datacenter onde está alojado o serviço Azure Search terão as taxas de latência superior.  Por esse motivo, muitas vezes, é importante ter vários serviços de pesquisa em regiões que estão em proximidade com mais detalhes para estes utilizadores.  O Azure Search atualmente não fornecem um método automatizado de georreplicação índices da Azure Search em várias regiões, mas existem algumas técnicas que podem ser utilizadas que podem tornar esse processo simples de implementar e gerir. Elas são descritas nas próximas seções.

O objetivo de um conjunto distribuída geograficamente dos serviços de pesquisa é ter dois ou mais índices disponíveis em duas ou mais regiões em que um utilizador será encaminhado para o serviço de Azure Search que fornece a latência mais baixa, como mostrado neste exemplo:

   ![Cross-separador de serviços por região][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Mantendo os dados sincronizados em vários serviços do Azure Search
Existem duas opções para manter os seus serviços de pesquisa distribuída em sincronia que são compostas por meio da [indexador de pesquisa do Azure](search-indexer-overview.md) ou da API Push (também conhecido como o [API REST da Azure Search](https://docs.microsoft.com/rest/api/searchservice/)).  

### <a name="azure-search-indexers"></a>Indexadores do Azure Search
Se estiver a utilizar o indexador de pesquisa do Azure, já está a importar as alterações de dados de um arquivo de dados central, como o BD SQL do Azure ou do Azure Cosmos DB. Quando cria uma nova pesquisa de serviço, simplesmente também criar um indexador de pesquisa do Azure novo para esse serviço que aponta para esse mesmo arquivo de dados. Dessa forma, sempre que vêm de novas alterações no arquivo de dados, eles serão, em seguida, ser indexados por vários indexadores.  

Eis um exemplo de como ficaria nessa arquitetura.

   ![Origem de dados única com o indexador distribuído e combinações de serviço][2]

### <a name="push-api"></a>API de push
Se estiver a utilizar a API do Azure Search Push para [atualizar o conteúdo no seu índice da Azure Search](https://docs.microsoft.com/rest/api/searchservice/update-index), pode manter os vários serviços de pesquisa em sincronia ao enviar alterações para todos os serviços de pesquisa, sempre que é necessária uma atualização.  Ao fazer isso é importante certificar-se de que lidar com casos em que uma atualização do serviço de pesquisa de uma falha e uma ou mais atualizações com êxito.

## <a name="leveraging-azure-traffic-manager"></a>Tirar partido do Gestor de tráfego do Azure
[O Gestor de tráfego do Azure](../traffic-manager/traffic-manager-overview.md) permite-lhe para encaminhar os pedidos para vários sites localizados geograficamente que, em seguida, são apoiados por vários serviços de pesquisa do Azure.  Uma vantagem do Gestor de tráfego é que podem testar o Azure Search para garantir que está disponível e encaminhar os utilizadores para serviços de pesquisa alternativo em caso de período de indisponibilidade.  Além disso, se o encaminhamento de pedidos de pesquisa por meio de Web Sites do Azure, Gestor de tráfego do Azure permite que carregar casos de saldo em que o Web site está ativo, mas não a Azure Search.  Eis um exemplo da aparência da arquitetura que tira partido do Gestor de tráfego.

   ![Cross-separador de serviços por região, com o Gestor de tráfego central][3]

## <a name="monitoring-performance"></a>Monitorização do desempenho
O Azure Search oferece a capacidade de analisar e monitorizar o desempenho do seu serviço através de [análise de tráfego de pesquisa (STA)](search-traffic-analytics.md). Por meio de STA, opcionalmente, pode iniciar as operações de pesquisa individuais, bem como métricas agregadas para uma conta de armazenamento do Azure que, em seguida, pode ser processada para análise ou visualizada no Power BI.  Utilizar métricas STA, poderá rever as estatísticas de desempenho, tais como o número médio de consultas ou tempos de resposta da consulta.  Além disso, o registo de operação permite-lhe explorar detalhes de operações de pesquisa específico.

STA é uma ferramenta valiosa para compreender as taxas de latência desse ponto de vista do Azure Search.  Uma vez que as métricas de desempenho de consulta com sessão iniciadas baseiam-se o tempo que uma consulta leva para ser totalmente processados na Azure Search (desde o momento em que for solicitada quando ele é enviado), é possível usá-lo para determinar se são de problemas de latência do lado do serviço de Azure Search ou detalhes IDE do serviço, como from a latência de rede.  

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre os limites de serviços e escalões de preços para cada um, veja [limites de serviço do Azure Search](search-limits-quotas-capacity.md).

Visite [planeamento de capacidade](search-capacity-planning.md) para saber mais sobre as combinações de partição e réplica.

Para obter mais aprofundamento sobre o desempenho e para ver algumas demonstrações de como implementar as otimizações discutidas neste artigo, veja o vídeo seguinte:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png
