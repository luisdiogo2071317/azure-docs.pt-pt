---
title: "Começar a utilizar ferramentas de base de dados elásticas - Azure | Microsoft Docs"
description: "EXPLICAÇÃO básica da funcionalidade ferramentas de base de dados elástica do SQL Database do Azure, incluindo uma aplicação de exemplo simples para execução."
services: sql-database
documentationcenter: 
manager: jstrauss
author: anumjs
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: anjangsh
ms.openlocfilehash: 615e5b0bf299a844ea2d37476fc704c48e17b363
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Começar a utilizar as ferramentas de base de dados elástica
Este documento apresenta-lhe a experiência de programação para o [biblioteca de clientes de base de dados elástica](sql-database-elastic-database-client-library.md) por ajudar a executar uma aplicação de exemplo. A aplicação de exemplo cria uma aplicação em partição horizontal simples e explicar capacidades principais da funcionalidade ferramentas de base de dados elástica do SQL Database do Azure. Concentra-se em casos de utilização [gestão de mapa de partições horizontais](sql-database-elastic-scale-shard-map-management.md), [encaminhamento de dados dependentes](sql-database-elastic-scale-data-dependent-routing.md), e [consultar várias partições horizontais](sql-database-elastic-scale-multishard-querying.md). A biblioteca de clientes está disponível para .NET, bem como de Java. 

## <a name="elastic-database-tools-for-java"></a>Ferramentas de base de dados elástica para Java
### <a name="prerequisites"></a>Pré-requisitos
* Um Java Developer Kit (JDK), versão 1.8 ou posterior
* [Maven](http://maven.apache.org/download.cgi)
* Um servidor lógico no Azure ou uma instância local do SQL Server

### <a name="download-and-run-the-sample-app"></a>Transferir e executar a aplicação de exemplo
Para criar os ficheiros JAR e começar a utilizar o projeto de exemplo, efetue o seguinte: 
1. Clone o [repositório do GitHub](https://github.com/Microsoft/elastic-db-tools-for-java) que contém a biblioteca de cliente, juntamente com a aplicação de exemplo. 

2. Editar o _./sample/src/main/resources/resource.properties_ ficheiro para definir o seguinte:
    * TEST_CONN_USER
    * TEST_CONN_PASSWORD
    * TEST_CONN_SERVER_NAME

3. Para compilar o projeto de exemplo, além de _. / exemplo_ diretório, execute o seguinte comando:

    ```
    mvn install
    ```
    
4. Para iniciar o projeto de exemplo no _. / exemplo_ diretório, execute o seguinte comando: 
    
    ```
    mvn -q exec:java "-Dexec.mainClass=com.microsoft.azure.elasticdb.samples.elasticscalestarterkit.Program"
    ```
    
5. Para saber mais sobre as funcionalidades da biblioteca de cliente, experimente as várias opções. Pode explorar o código para saber mais sobre a implementação de aplicação de exemplo.

    ![Progresso-java][5]
    
Parabéns! Criada com êxito e executar primeiro a aplicação ao utilizar ferramentas de base de dados elásticas em SQL Database do Azure. Utilize o Visual Studio ou o SQL Server Management Studio para ligar à base de dados SQL e observe rápida shards criado o exemplo. Vai notar novas bases de dados de partição horizontal de exemplo e uma base de dados manager partições horizontais do mapa que tiver criado o exemplo. 

Para adicionar a biblioteca de clientes para o seus próprios projeto Maven, adicione a seguinte dependência no seu ficheiro POM:

```xml
<dependency> 
    <groupId>com.microsoft.azure</groupId> 
    <artifactId>elastic-db-tools</artifactId> 
    <version>1.0.0</version> 
</dependency> 
```

## <a name="elastic-database-tools-for-net"></a>Ferramentas de base de dados elástica para .NET 
### <a name="prerequisites"></a>Pré-requisitos
* Visual Studio 2012 ou posterior com c#. Transferir uma versão gratuita em [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 ou posterior. Para obter a versão mais recente, consulte [instalar NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

### <a name="download-and-run-the-sample-app"></a>Transferir e executar a aplicação de exemplo
Para instalar a biblioteca, visite [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). A biblioteca é instalada com a aplicação de exemplo descrita na secção seguinte.

Para transferir e executar o exemplo, siga estes passos: 

1. Transferir o [elástico ferramentas DB para SQL do Azure - introdução exemplo](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) da MSDN. Deszipe o exemplo para uma localização que escolher.

2. Para criar um projeto, abra o *ElasticScaleStarterKit.sln* soluções do *c#* diretório.

3. A solução para o projeto de exemplo, abra o *App. config* ficheiro. Em seguida, siga as instruções no ficheiro para adicionar o nome do servidor SQL Database do Azure e as informações início de sessão (nome de utilizador e palavra-passe).

4. Crie e execute a aplicação. Quando lhe for pedido, ative o Visual Studio para restaurar os pacotes de NuGet da solução. Esta ação transfere a versão mais recente da biblioteca de clientes de base de dados elástica NuGet.

5. Para saber mais sobre as funcionalidades da biblioteca de cliente, experimente as várias opções. Tenha em atenção os passos que leva a aplicação na consola de saída e à vontade explorar o código em segundo plano.
   
    ![Progresso][4]

Parabéns! Criada com êxito e executar primeiro a aplicação ao utilizar ferramentas de base de dados elástica na base de dados do SQL Server. Utilize o Visual Studio ou o SQL Server Management Studio para ligar à base de dados SQL e observe rápida shards criado o exemplo. Vai notar novas bases de dados de partição horizontal de exemplo e uma base de dados manager partições horizontais do mapa que tiver criado o exemplo.

> [!IMPORTANT]
> Recomendamos que utilize sempre a versão mais recente do Management Studio, para que o se a manter sincronizadas com as atualizações do Azure e a SQL Database. [Atualize o SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="key-pieces-of-the-code-sample"></a>Peças chave de exemplo de código
* **Gerir shards e partições horizontais mapeia**: O código ilustra como trabalhar com shards, intervalos e mapeamentos no *ShardManagementUtils.cs* ficheiro. Para obter mais informações, consulte [ampliação das bases de dados com o Gestor de mapa de partições horizontais](http://go.microsoft.com/?linkid=9862595).  

* **Encaminhamento de dados dependentes**: encaminhamento de transações para o ID de partição horizontal direita é apresentado no *DataDependentRoutingSample.cs* ficheiro. Para obter mais informações, consulte [encaminhamento de dados dependentes](http://go.microsoft.com/?linkid=9862596). 

* **Consultar o ao longo de vários shards**: consultar em shards situação é ilustrada no *MultiShardQuerySample.cs* ficheiro. Para obter mais informações, consulte [consultar várias partições horizontais](http://go.microsoft.com/?linkid=9862597).

* **Adicionar shards vazios**: iterativo a adição de partições horizontais vazias novo é realizada o código de *CreateShardSample.cs* ficheiro. Para obter mais informações, consulte [ampliação das bases de dados com o Gestor de mapa de partições horizontais](http://go.microsoft.com/?linkid=9862595).

## <a name="other-elastic-scale-operations"></a>Outras operações de dimensionamento elástico
* **Divisão de um ID de partição horizontal existente**: A capacidade de dividir shards é fornecida pela ferramenta de intercalação de divisão. Para obter mais informações, consulte [mover dados entre bases de dados de nuvem de escalamento horizontal](sql-database-elastic-scale-overview-split-and-merge.md).

* **A intercalação de partições horizontais existentes**: partições horizontais intercala também é executada utilizando a ferramenta de intercalação de divisão. Para obter mais informações, consulte [mover dados entre bases de dados de nuvem de escalamento horizontal](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Custo
A biblioteca de ferramentas de base de dados elásticas é gratuita. Quando utilizar ferramentas de base de dados elásticas, pode implicar sem custos adicionais além do custo de utilização do Azure da sua. 

Por exemplo, a aplicação de exemplo cria novas bases de dados. O custo desta capacidade depende da edição de base de dados do SQL Server que escolher e a utilização do Azure da sua aplicação.

Para obter informações sobre preços, consulte [base de dados SQL detalhes de preços](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Passos seguintes
Para obter mais informações sobre as ferramentas de base de dados elásticas, consulte os artigos seguintes:

* Exemplos de código: 
  * Ferramentas de base de dados elástica ([.NET](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE), [Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-elasticdb-tools%22))
  * [Ferramentas de base de dados elástica para o SQL do Azure - Entity Framework integração](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Elasticidade de partições horizontais no Centro de scripts](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blogue: [anúncio do dimensionamento elástico](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [implementar fragmentação através de escalamento horizontal com o vídeo de biblioteca de cliente de base de dados elástica](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Canal 9: [horizontal elástico vídeo de descrição geral](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Fórum de discussão: [fórum da SQL Database do Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Para medir o desempenho: [contadores de desempenho para o Gestor de mapa de partições horizontais](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
[5]: ./media/sql-database-elastic-scale-get-started/java-client-library.PNG

