---
title: Executar várias regiões escritas e leituras escolhendo o direito a chave de criação de partições | Documentos da Microsoft
description: Saiba mais sobre como conceber arquiteturas de aplicativos com locais leituras e escritas em várias regiões geográficas com o Azure Cosmos DB ao escolher uma chave de partição.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/6/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d38b7cd7d1f28f706e94782602926abc8fc11e3
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42057603"
---
# <a name="perform-multi-region-writes-and-reads-by-choosing-the-right-partitioning-key"></a>Executar várias regiões escritas e leituras escolhendo o direito a chave de criação de partições

Azure Cosmos DB suporta de chave na mão [replicação global](distribute-data-globally.md), que permite que distribua os dados em várias regiões com acesso de baixa latência em qualquer lugar na carga de trabalho. Esse modelo é geralmente utilizado para cargas de trabalho do publicador/consumidor quando existir um gravador numa única região geográfica e distribuídos globalmente leitores noutras regiões (leitura). 

Também pode utilizar o suporte de replicação global do Azure Cosmos DB para criar aplicativos em que gravadores e leitores são distribuídos globalmente. Este documento descreve um padrão que permite a obtenção de escrita local e o local de acesso de leitura para os escritores distribuídos com o Azure Cosmos DB.

## <a id="ExampleScenario"></a>Publicação de conteúdo - um cenário de exemplo
Vamos examinar um cenário do mundo real para descrever como é possível usar padrões e distribuída globalmente várias-region/várias-master leitura escrita com o Azure Cosmos DB. Considere uma plataforma de publicação de conteúdo criada no Azure Cosmos DB. Seguem-se alguns requisitos que esta plataforma tem de cumprir para obter uma experiência de utilizador fantástica para os editores e consumidores.

* Autores e os assinantes estão distribuídos pelo mundo 
* Os autores tem de publicar artigos (escrita) para a respetiva região (mais próxima dos) local
* Os autores de tem os leitores/subscritores dos respetivos artigos que são distribuídos em todo o mundo. 
* Os assinantes devem receber uma notificação quando novas publicações são publicadas.
* Os assinantes tem de ser capazes de ler os artigos da respetiva região local. Também deve ser capazes de adicionar revisões os artigos seguintes. 
* Qualquer pessoa, incluindo o autor dos artigos deve ser capaz de ver todas as revisões anexados a artigos de uma região local. 

Partindo do princípio de milhões de consumidores e publicadores com milhares de milhões de artigos, em breve temos que confrontar problemas de dimensionamento, juntamente com assegurando uma localidade de acesso que são. Tal como acontece com a maioria dos problemas de escalabilidade, a solução é uma boa estratégia de particionamento. Em seguida, vamos ver como pode modelar a artigos, revisão e notificações como documentos, configurar contas do Azure Cosmos DB e implemente uma camada de acesso de dados. 

Se gostaria de saber mais sobre a criação de partições e chaves de partição, veja [criação de partições e dimensionamento no Azure Cosmos DB](partition-data.md).

## <a id="ModelingNotifications"></a>Notificações de modelagem
As notificações são feeds de dados específicos a um utilizador. Por conseguinte, os padrões de acesso para documentos de notificações estão sempre no contexto de utilizador único. Por exemplo, seria "post uma notificação a um utilizador" ou "obter todas as notificações para um determinado usuário". Por isso, a opção ideal de chave para este tipo de criação de partições seria `UserId`.

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // The user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // The partition Key for the resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of the notification. 
        public string ArticleId { get; set; } 
    }

## <a id="ModelingSubscriptions"></a>Subscrições de modelagem
As assinaturas podem ser criadas para diversos critérios, como uma categoria específica de artigos de interesse ou um fabricante específico. Por conseguinte, o `SubscriptionFilter` é uma boa opção para a chave de partição.

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <a id="ModelingArticles"></a>Artigos de modelagem
Depois de um artigo é identificado por meio de notificações, as consultas subseqüentes normalmente se baseiam o `Article.Id`. Escolher `Article.Id` como partição a chave, portanto, fornece a melhor distribuição para armazenar os artigos dentro de uma coleção do Azure Cosmos DB. 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of the article
        public string Author { get; set; }

        // Category/genre of the article
        public string Category { get; set; }

        // Tags associated with the article
        public string[] Tags { get; set; }

        // Title of the article
        public string Title { get; set; }
        
        //... 
    }

## <a id="ModelingReviews"></a>Revisões de modelagem
Como artigos, revisões são principalmente escritas e ler no contexto do artigo. Escolher `ArticleId` como uma partição chave fornece melhor distribuição e acesso eficiente de revisões associado artigo. 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of the review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <a id="DataAccessMethods"></a>Métodos de camada de acesso de dados
Agora vamos examinar os dados principais métodos de acesso é necessário implementar. Eis a lista de métodos que o `ContentPublishDatabase` tem de:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Configuração de conta do Azure Cosmos DB
Para garantir local leituras e escritas, que tem de partição dados não apenas na partição chave, mas também com base no padrão de acesso geográfica em regiões. O modelo se baseia em ter uma conta de base de dados georreplicado do Azure Cosmos DB para cada região. Por exemplo, com duas regiões, aqui está um programa de configuração para escritas de várias regiões:

| Nome da Conta | Região de Escrita | Região de Leitura |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

O diagrama seguinte mostra como leituras e gravações são realizadas num aplicativo típico com esta configuração:

![Arquitetura de vários mestre do Azure Cosmos DB](./media/multi-master-workaround/multi-master.png)

Aqui está um trecho de código que mostra como inicializar os clientes numa DAL em execução no `West US` região.
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

Com a configuração anterior, a camada de acesso a dados pode reencaminhar todas as escritas para a conta local com base nas quais tenha sido implementado. Leituras são executadas pela leitura de ambas as contas para obter a vista global dos dados. Essa abordagem pode ser estendida para tantas regiões conforme necessário. Por exemplo, aqui está uma configuração com três regiões geográficas:

| Nome da Conta | Região de Escrita | Região de leitura 1 | Região de leitura 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Implementação de camada de acesso de dados
Agora vamos examinar a implementação de camada de acesso de dados (DAL) para uma aplicação com duas regiões graváveis. A DAL deve implementar os seguintes passos:

* Criar várias instâncias de `DocumentClient` para cada conta. Com duas regiões, cada instância camada DAL tem um `writeClient` e um `readClient`. 
* Com base na região da aplicação implementada, a configurar os pontos de extremidade `writeclient` e `readClient`. Por exemplo, a DAL implementada no `West US` usa `contentpubdatabase-usa.documents.azure.com` para efetuar operações de escrita. A DAL implementado num `NorthEurope` usa `contentpubdatabase-europ.documents.azure.com` para escritas.

Com a configuração anterior, os métodos de acesso de dados podem ser implementados. Escrever operações reencaminhar a gravação para os correspondentes `writeClient`.

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

Para ler notificações e avaliações, é necessário ler de regiões e União os resultados como mostrado no seguinte fragmento:

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

Portanto, ao escolher uma chave de particionamento bom e, em seguida, com base na conta de particionamento estático, pode conseguir escritas de locais de várias regiões e leituras com o Azure Cosmos DB.

## <a id="NextSteps"></a>Passos seguintes
Neste artigo, descrevemos como pode usar padrões de escrita de leitura de várias regiões distribuídas globalmente com o Azure Cosmos DB com a publicação de conteúdos como um cenário de exemplo.

* Saiba mais sobre como o Azure Cosmos DB suporta [distribuição global](distribute-data-globally.md)
* Saiba mais sobre [ativações pós-falha automáticas e manuais no Azure Cosmos DB](regional-failover.md)
* Saiba mais sobre [consistência global com o Azure Cosmos DB](consistency-levels.md)
* Desenvolver com várias regiões com o [Azure Cosmos DB - API de SQL](tutorial-global-distribution-sql-api.md)
* Desenvolver com várias regiões com o [Azure Cosmos DB - API de MongoDB](tutorial-global-distribution-MongoDB.md)
* Desenvolver com várias regiões com o [Azure Cosmos DB - API de tabela](tutorial-global-distribution-table.md)
