---
title: 'Padrão de conceção do Cosmos BD do Azure: aplicações de redes sociais | Microsoft Docs'
description: Saiba mais sobre um padrão de conceção para as redes sociais, tirando partido a flexibilidade de armazenamento de base de dados do Azure Cosmos e outros serviços do Azure.
keywords: aplicações de redes sociais
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: maquaran
ms.openlocfilehash: f81a087a2595db41dbe84a54ad1fd01adf043515
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060408"
---
# <a name="going-social-with-azure-cosmos-db"></a>Vai sociais com base de dados do Azure Cosmos
Living num society em massa interligado significa que, num ponto da vida, passam a fazer parte de um **rede social**. Utilizar as redes sociais manter entre em contacto com amigos, colegas, família, ou, por vezes, partilhar o seu passion com pessoas com interesses comuns.

Como engenheiros ou os programadores, poderá ter wondered como estas redes armazenar e interligar-se os dados, ou poderá ter mesmo foi a tarefa para criar ou architect uma nova rede social para um mercado niche específico yourselves. Que é quando a pergunta significativa seja: como a todos os dados são armazenados?

Vamos imaginar que está a criar uma novo e shiny rede social, onde os utilizadores podem publicar artigos com suporte de dados relacionados, como, fotografias, vídeos ou até mesmo música. Os utilizadores podem comentar publicações e forneça pontos para as classificações. Existirá um feed de mensagens que os utilizadores ser capazes de interagir com a página de destino principal Web site. Isto não som complexo (inicialmente), mas com vista à, simplicidade, vamos parar existe (pode delve para feeds personalizadas do utilizador afetados por relações, mas excede o objetivo deste artigo).

Por isso, como a armazenar esta e onde?

Muitos dos poderão ter experiência em bases de dados do SQL Server ou ter, pelo menos, a noção de [modelação de dados relacional](https://en.wikipedia.org/wiki/Relational_model) e poderá ser tempted para iniciar o desenho algo semelhante ao seguinte:

![Diagrama que ilustra um modelo de relacional relativo](./media/social-media-apps/social-media-apps-sql.png) 

Uma estrutura de dados perfeitamente normalizado e pretty... que não a escala. 

Não obter-me o problema, posso já trabalhou com bases de dados SQL, de todos os meus vida, são excelente, mas como cada plataforma padrão, prática e software, não é perfeita para cada cenário.

Por que motivo não SQL Server neste cenário, a melhor opção? Vamos ver a estrutura de um pedido de post único, posso era mostrar que post num Web site ou aplicação, que seria necessário fazer uma consulta com... Associações de tabelas oito (!) apenas para mostrar um único post, agora, imagem de um fluxo de mensagens que dinamicamente carregar e são apresentados num ecrã e poderá ver onde vou.

Pode, obviamente, utilizar uma instância do SQL bastantes com suficiente adequada para resolver milhares de consultas com estas associações muitos para servir o conteúdo, mas verdadeiramente, motivo pelo qual seria, quando existe uma solução mais simples?

## <a name="the-nosql-road"></a>Viagem NoSQL
Este artigo irá guiá-lo para a modelação de dados da sua plataforma de redes sociais com base de dados do Azure NoSQL [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) de forma económica, tirando partido de outras BD do Cosmos Azure funcionalidades como o [Gremlin Graph API](../cosmos-db/graph-introduction.md). Utilizar um [NoSQL](https://en.wikipedia.org/wiki/NoSQL) abordagem, armazenamento de dados, no formato JSON e aplicar [denormalization](https://en.wikipedia.org/wiki/Denormalization), a publicação complicada anteriormente pode ser transformada numa única [documento](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

E pode ser obtido com uma única consulta e com nenhuma associações. Isto é muito mais simples e fácil e budget-wise, requer menos recursos para alcançar um resultado melhor.

BD do Azure do Cosmos certifica-se de que todas as propriedades são indexadas com o respetivo indexação automática, que pode ser mesmo [personalizado](indexing-policies.md). A abordagem sem esquema permite-na armazenar documentos com diferentes e dinâmicas estruturas, talvez amanhã que pretende cronologia ter uma lista de categorias nem hashtags associados aos mesmos, base de dados do Cosmos processará novos documentos com os atributos adicionados com nenhum trabalho adicional requerido por-nos.

Comentários sobre um pedido post podem ser tratados como outras mensagens com uma propriedade principal (Isto simplifica o mapeamento de objeto). 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

E todas as interações de redes sociais podem ser armazenadas num objeto separado como contadores:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

A criação de feeds é apenas um fim de criação de documentos que podem conter uma lista de ids de post com uma ordem de relevância especificado:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Pode ter uma transmissão em fluxo "mais recente" com mensagens ordenadas por data de criação, uma sequência "hottest" com essas publicações com mais likes nas últimas 24 horas, mesmo que foi implementar uma sequência personalizada para cada utilizador com base na lógica como followers e interesses e ainda seria uma lista de  publicações. É um independentemente de como criar estas listas, mas o desempenho de leitura permanece unhindered. Depois de adquirir uma destas listas, emitir uma única consulta Cosmos BD utilizando a [no operador](sql-api-sql-query.md#WhereClause) obter páginas de mensagens de cada vez.

Os fluxos de feed pode ser criados utilizando [dos serviços de aplicações do Azure](https://azure.microsoft.com/services/app-service/) em segundo plano processos: [Webjobs](../app-service/web-sites-create-web-jobs.md). Quando um pedido post for criado, o processamento em segundo plano pode ser acionado utilizando [Storage do Azure](https://azure.microsoft.com/services/storage/) [filas](../storage/queues/storage-dotnet-how-to-use-queues.md) e Webjobs acionadas utilizando o [SDK de Webjobs do Azure](https://github.com/Azure/azure-webjobs-sdk/wiki), implementar o após a propagação dentro de fluxos com base na sua própria lógica personalizada. 

Pontos e likes através de um pedido post podem ser processados de forma diferida com esta técnica mesma para criar um ambiente eventualmente consistente.

Followers são trickier. Cosmos DB tem um limite de tamanho máximo do documento e leitura/escrita documentos grandes podem afetar a escalabilidade da sua aplicação. Por isso, pode pensar sobre o armazenamento followers como um documento com esta estrutura:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Isto poderá funcionar para um utilizador com milhares de alguns followers, mas se alguns celebrity associa ranks, esta será abordagem levar para um tamanho de documento grande e, eventualmente, poderá atingiu o limite de tamanho do documento.

Para resolver isto, pode utilizar uma abordagem mista. Como parte do documento estatísticas de utilizadores pode armazenar o número de followers:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

E o gráfico de followers real pode ser armazenado utilizando a base de dados do Azure Cosmos [Gremlin Graph API](../cosmos-db/graph-introduction.md), para criar [vertexes](http://mathworld.wolfram.com/GraphVertex.html) para cada utilizador e [contornos](http://mathworld.wolfram.com/GraphEdge.html) que manter o "D-forma-B" relações. A Graph API vamos, não apenas obter followers de um determinado utilizador, mas criar consultas mais complexas, mesmo sugerir pessoas em comum. Se adicionar ao gráfico as categorias de conteúdo que as pessoas gosta ou desfrutar, pode iniciar weaving experiências que incluem a deteção de conteúdos inteligente, sugerindo conteúdo que os que se segue, como, ou localizar pessoas com quem pode ter praticamente em comum.

O documento de estatísticas de utilizadores ainda pode ser utilizado para criar cartões na IU ou pré-visualizações de perfil rápida.

## <a name="the-ladder-pattern-and-data-duplication"></a>A duplicação de dados e padrão "Ladder"
Como poderá ter reparado no documento JSON que faça referência a um pedido post, existem várias ocorrências de um utilizador. E seria ter adivinhado direita, que isto significa que as informações que representa fornecido por este denormalization, um utilizador poderão estar presentes em mais do que um local.

Para permitir a consultas mais rápidas, pode implicar duplicação de dados. O problema com este efeito é que, se por alguma ação, alterações de dados de um utilizador, tem de localizar todas as atividades ele nunca foi e atualizá-las todas as. Não som práticas, direito?

Pretender resolvê-lo, identificando os atributos de chave de um utilizador que mostram na sua aplicação para cada atividade. Se visualmente mostrar um pedido post na sua aplicação e mostrar apenas o criador nome e a imagem, motivo pelo qual armazenar todos os dados do utilizador no atributo "revisão criada por"? Se para cada comentário Mostrar apenas imagem do utilizador, realmente não têm o resto do seus informações. É onde algo que posso chamar o padrão"Ladder" entra em play.

Vamos informações de utilizador como um exemplo:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"\@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Ao observar estas informações, pode rapidamente detetar que é informações importantes e que, por conseguinte, não se encontra, criar um "Ladder":

![Diagrama de um padrão de ladder](./media/social-media-apps/social-media-apps-ladder.png)

O passo mais pequeno é chamado um UserChunk, a informação de informações que identifica um utilizador mínima e é utilizada para dados duplicados. Ao reduzir o tamanho dos dados duplicados para apenas as informações que "mostrará", pode reduzir a possibilidade de atualizações em grande escala.

O passo média denomina-se o utilizador, é de dados completa que serão utilizados na maior parte das consultas de desempenho dependentes na BD do Cosmos, mais acedidos e crítico. Inclui as informações representadas por um UserChunk.

A maior é o utilizador expandido. Inclui todas as informações de utilizador críticos e outros dados que realmente não necessitam de leitura rápida ou de utilização é eventual (por exemplo, o processo de início de sessão). Estes dados podem ser armazenados fora do Cosmos DB, na SQL Database do Azure ou tabelas de armazenamento do Azure.

Por que motivo seria que o utilizador de divisão e mesmo armazenar estas informações em locais diferentes? Porque de um ponto de vista do desempenho, a maiores documentos, o costlier consultas. Manter os documentos slim, com as informações corretas para executar todas as suas consultas dependentes de desempenho para a sua rede social e armazenar outras informações adicionais para cenários eventual como perfil completo edições, inícios de sessão, mesmo a extração de dados para análise de utilização e macrodados iniciativas. Realmente não está interessado se a recolha de dados de extração de dados são mais lentos porque está em execução na base de dados do Azure SQL, tem de ter concern apesar que os utilizadores tenham uma experiência slim e rápida. Um utilizador, armazenado na base de dados do Cosmos, deverá ter o seguinte aspeto:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"\@john"
    }

E um pedido Post deverá ter o seguinte aspeto:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

E quando uma edição acontecer onde um dos atributos do segmento é afetado, é fácil localizar os documentos afetados através de consultas que apontam para os atributos indexados (SELECIONAR * FROM mensagens p onde p.createdBy.id = = "edited_user_id") e, em seguida, atualizar os segmentos.

## <a name="the-search-box"></a>Caixa de pesquisa
Os utilizadores irão gerar, luckily, muito conteúdo. E deve ser capaz de fornecer a capacidade de pesquisar e localizar conteúdo que poderá não ser diretamente no respetivos conteúdos fluxos, talvez porque não segue os criadores ou, talvez estiver apenas a tentar encontrar que post antigo fez há de seis meses.

Thankfully, e porque estão a utilizar o Azure Cosmos DB, pode facilmente implementar um motor de pesquisa utilizando [da Azure Search](https://azure.microsoft.com/services/search/) em alguns minutos e sem escrever uma única linha de código (exceto obviamente, o processo de pesquisa e IU).

Por que motivo é por isso, fácil?

A Azure Search implementa que chamarem [indexadores](https://msdn.microsoft.com/library/azure/dn946891.aspx), em segundo plano processa esse hook nos seus repositórios de dados e automagically adicionar, atualizar ou remover os objetos nos índices. Suportam um [indexadores de base de dados do Azure SQL](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [indexadores de Blobs do Azure](../search/search-howto-indexing-azure-blob-storage.md) e thankfully, [indexadores de base de dados do Azure Cosmos](../search/search-howto-index-documentdb.md). A transição das informações da base de dados do Cosmos para a Azure Search é simples, como as informações de arquivo no formato JSON, apenas terá de [criar o seu índice](../search/search-create-index-portal.md) e mapear os atributos a partir dos seus documentos pretende indexada e que é num fim de minutos (depende do tamanho dos seus dados,) todo o seu conteúdo estará disponível para ser procurado após a, pela melhor solução de pesquisa-como-um-serviço na infraestrutura de nuvem. 

Para obter mais informações sobre a Azure Search, pode visitar o [Guia do Hitchhiker para pesquisa](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="the-underlying-knowledge"></a>O conhecimento subjacente
Depois de armazenar este conteúdo que aumenta e o crescimentos de todos os dias, poderá encontrar pensar: o que posso fazer com todos os este fluxo de informações dos meus utilizadores?

A resposta é simples: colocá-la para o trabalho e obter a partir do mesmo.

No entanto, o que aprender? Alguns exemplos de fácil incluem [análise de dados de sentimento](https://en.wikipedia.org/wiki/Sentiment_analysis), recomendações com base nas preferências de um utilizador ou mesmo uma automatizada conteúdo moderator que garante que todos os conteúdos publicados pelo seu rede social são seguros para a família de conteúdo.

Agora que tem posso que estabelecer ligação com ele, provavelmente irá considerar terá algumas PhD no ciência de bibliotecas para extrair estes padrões e as informações fora de bases de dados simples e ficheiros, mas seria errado.

[O Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), que fazem parte do [Cortana Intelligence Suite](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx), é um serviço em nuvem completamente gerido que lhe permite criar fluxos de trabalho utilizando algoritmos numa interface simples de arrastar e largar, os seus próprios algoritmos no decódigo[ R](https://en.wikipedia.org/wiki/R_\(programming_language\)) ou utilizar algumas das já criadas e pronta a utilizar APIs como: [análise de texto](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Moderator conteúdo, ou [recomendações](https://gallery.azure.ai/Solution/Recommendations-Solution).

Para alcançar qualquer um destes cenários de Machine Learning, pode utilizar [do Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) para incorporar as informações de diferentes origens e utilizar [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) para processar as informações e gerar uma saída que podem ser processados pelo Azure Machine Learning.

Outra opção disponível é utilizar [serviços cognitivos Microsoft](https://www.microsoft.com/cognitive-services) para analisar os seus utilizadores conteúdos; não só pode compreende-las melhor (através da análise escrever com [API de análise de texto](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), mas Também pode detetar conteúdo madura ou indesejável e agir em conformidade com [computador visão API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Serviços cognitivos inclui várias soluções de out of box que não necessitam de qualquer tipo de dados de conhecimento do Machine Learning para utilizar.

## <a name="a-planet-scale-social-experience"></a>Uma experiência de redes sociais planet escala
Existe um último, mas não pelo menos, artigo importante posso devem ser abordadas: **escalabilidade**. Ao conceber uma arquitetura é fundamental que cada componente pode dimensionar no seu próprio, ou porque tem de processar os dados mais ou porque que pretende ter uma maior cobertura geográfica (ou ambos!). Thankfully, alcançar uma tarefa complexa é um **experiência chave na mão** com base de dados do Cosmos.

Suporte cosmos DB [criação dinâmica de partições](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-a-box criando automaticamente partições com base num determinado **chave de partição** (definido como um dos atributos na sua documentos). Definir a chave de partição correto tem de ser efetuada no momento da conceção e mantendo em mente a [melhores práticas](../cosmos-db/partition-data.md#designing-for-partitioning) disponíveis; no caso de uma experiência de redes social, a estratégia de criação de partições têm de estar alinhada com a forma de consultar (leituras no mesmo partição são desejável) e de escrita (evitar "frequente oportunidades" pelo escritas em várias partições). Algumas opções são: partições com base numa chave temporal (dia/mês/semana), por categoria de conteúdo, por região geográfica, por utilizador; todas as realmente depende de como irá consultar os dados e mostrá-lo na sua experiência de redes social. 

Um ponto interessante mencionar é que a BD do Cosmos irá executar as suas consultas (incluindo [agregados](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) em todas as partições de forma transparente, não precisa de adicionar qualquer lógica à medida que aumenta os dados.

Com a hora, eventualmente irá aumentar no tráfego e o consumo de recursos (medido em [RUs](request-units.md), ou unidades de pedido) irá aumentar. Irá ler e escrever com mais frequência, à medida que aumenta a sua base de utilizadores e irão começar a criar e ler mais conteúdos; a capacidade de **aumentar o débito** é vital. Aumentar a sua RUs é fácil, pode fazê-lo com alguns cliques no portal do Azure ou pelo [emitir comandos através da API](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer).

![Como aumentar verticalmente e definir uma chave de partição](./media/social-media-apps/social-media-apps-scaling.png)

O que acontece se coisas mantém a melhorar e os utilizadores de outra região, país ou continente, tenha em atenção a plataforma e comece a utilizá-la, que um excelente surprise!

Mas, aguarde..., logo que tenha em consideração as suas experiências com a plataforma não é o ideal; são até ao momento na direção oposta ao sua região operacional que a latência é terríveis e, obviamente não quisê-los para sair. Se apenas Ocorreu uma forma fácil de **expandir o alcance global**... mas não existe!

Cosmos DB permite-lhe [replicar os dados globalmente](../cosmos-db/tutorial-global-distribution-sql-api.md) e transparente com alguns cliques e automaticamente, selecione entre as regiões disponíveis da sua [código de cliente](../cosmos-db/tutorial-global-distribution-sql-api.md). Isto também significa que pode fazer com que [várias regiões de ativação pós-falha](regional-failover.md). 

Quando replicar os dados globalmente, tem de certificar-se de que os clientes podem tirar partido dos mesmos. Se estiver a utilizar um front-end da web ou aceder a APIs de clientes móveis, pode implementar [Traffic Manager do Azure](https://azure.microsoft.com/services/traffic-manager/) e clonar o App Service do Azure em todas as regiões pretendidas, com uma configuração de desempenho para suportar o expandida global cobertura. Quando os clientes acedem o front-end ou APIs, serão encaminhados para o serviço de aplicações mais próximo, o que por sua vez, irá ligar-se para a réplica local do Cosmos DB.

![A adição de cobertura global a sua plataforma de redes social](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Conclusão
Este artigo tenta shed algumas claro para alternativas de criação de redes sociais completamente no Azure com os serviços de baixo custo e pelo fornecimento de resultados excelente por incentivando a utilização de uma distribuição de solução e os dados de múltiplos em camadas de armazenamento denominada "Ladder".

![Diagrama de interação entre os serviços do Azure para redes sociais](./media/social-media-apps/social-media-apps-azure-solution.png)

O truth é que não há nenhum marca prata para este tipo de cenários, é synergy criado pela combinação dos serviços excelente que permitem-nos criar experiências excelente: a velocidade e a liberdade de BD do Cosmos do Azure para fornecer uma aplicação excelente de redes social, o Intelligence atrás de uma solução de pesquisa de primeira classe, como a Azure Search, a flexibilidade de serviços de aplicações do Azure para alojar aplicações não mesmo idioma agnóstico mas processos em segundo plano poderosa e o expansível Storage do Azure e SQL Database do Azure para armazenar quantidades enormes de dados e a capacidade de análise do Azure Machine Learning para criar dados de conhecimento e intelligence, que pode fornecer comentários para os processos e ajude-na entregar o conteúdo adequado para os utilizadores à direita.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre casos de utilização para Cosmos DB, consulte [casos de utilização comuns Cosmos DB](use-cases.md).
