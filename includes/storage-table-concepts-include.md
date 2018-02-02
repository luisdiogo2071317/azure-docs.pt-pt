## <a name="what-is-table-storage"></a>O que é o armazenamento de Tabelas
O armazenamento de Tabelas do Azure armazena grandes quantidades de dados estruturados. O serviço é um arquivo de dados NoSQL que aceita chamadas autenticadas de dentro e de fora da nuvem do Azure. As tabelas do Azure são ideais para armazenar dados estruturados não relacionais. Utilizações comuns do Armazenamento de Tabelas:

* Armazenamento de TBs de dados estruturados com capacidade para servirem aplicações de dimensionamento da Web
* Armazenamento de conjuntos de dados que não precisam de associações complexas, chaves externas ou procedimentos armazenados e podem ser desnormalizadas para acesso rápido
* Consulta rápida de dados com um índice em cluster
* Acesso a dados com o protocolo OData e consultas LINQ com Bibliotecas de .NET do Serviço de Dados WCF

Pode utilizar o armazenamento de Tabelas para armazenar e consultar conjuntos enormes de dados estruturados não relacionais. As suas tabelas serão dimensionadas à medida que aumenta a procura.

## <a name="table-storage-concepts"></a>Conceitos de armazenamento de tabelas
O armazenamento de tabelas contém os seguintes componentes:

![diagrama de componente de armazenamento de tabelas][Table1]

* **Formato de URL:** contas de armazenamento de tabelas do Azure, utilize este formato:`http://<storage account>.table.core.windows.net/<table>`

  As contas de API de tabela do Cosmos BD do Azure, utilize este formato:`http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Pode também endereçar diretamente as tabelas do Azure utilizando este endereço com o protocolo OData. Para obter mais informações, veja [OData.org][OData.org].
* **Contas:** todos os acessos ao Storage do Azure é efetuado através de uma conta de armazenamento. Veja [Metas de Desempenho e Escalabilidade do Storage do Azure](../articles/storage/common/storage-scalability-targets.md) para obter detalhes acerca da capacidade das contas de armazenamento. 

    Todos os acessos à base de dados do Azure Cosmos é feito através de uma conta de API de tabela. Consulte [criar uma conta de API de tabela](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account) para criar uma conta de API de tabela de detalhes.
* **Tabela**: uma tabela é uma coleção de entidades. As tabelas não impõem um esquema a entidades, o que significa que uma única tabela pode conter entidades que tenham conjuntos diferentes de propriedades.  
* **Entidade**: uma entidade é um conjunto de propriedades, semelhantes a uma linha de base de dados. Uma entidade no armazenamento do Azure pode ter até 1MB de tamanho. Uma entidade na base de dados do Azure Cosmos pode ter até 2MB de tamanho.
* **Propriedades**: uma propriedade é um par nome/valor. Cada entidade pode incluir até 252 propriedades para armazenar dados. Cada entidade tem também três propriedades do sistema que especificam uma chave de partição, uma chave de linha e um carimbo de data/hora. As entidades com a mesma chave de partição podem ser consultadas mais rapidamente e inseridas/atualizadas em operações atómicas. A chave de linha de uma entidade é o seu identificador exclusivo dentro de uma partição.

Para obter detalhes sobre a nomenclatura das tabelas e das propriedades, veja [Noções Básicas sobre o Modelo de Dados do Serviço Tabela](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
