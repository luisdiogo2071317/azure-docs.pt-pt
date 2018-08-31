---
title: Unidades de pedido e estimar o débito - Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre como compreender, especifique e estimar os requisitos da unidade de pedido no Azure Cosmos DB.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: rimman
ms.openlocfilehash: 66beeb2cc724f75d17a4c155f1cdb888153e8fbf
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286770"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades de pedido no Azure Cosmos DB

[O Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) é a base de dados Microsoft globalmente distribuída vários modelos. Com o Azure Cosmos DB, não tem de alugar máquinas virtuais, implementar software ou monitorizar bases de dados. O Azure Cosmos DB é operado e monitorizado continuamente por engenheiros de topo da Microsoft para garantir a proteção de disponibilidade, desempenho e dados de classe mundial. Pode aceder aos dados através de APIs à sua escolha, como o [SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md), e [tabela](table-introduction.md) APIs e gráficos por meio do [Gremlin API](graph-introduction.md). Todas as APIs são suportadas todos nativamente. 

A moeda do Azure Cosmos DB é o *unidade de pedido (RU)*. Com unidades de pedido, não terá de reservar as capacidades de leitura/gravação ou aprovisionar da CPU, memória e IOPS. Azure Cosmos DB suporta várias APIs que possuem operações diferentes, desde simples lê e escreve consultas de gráficos complexos. Uma vez que nem todos os pedidos são iguais, pedidos recebem uma quantidade normalizada de unidades de pedido com base na quantidade de computação necessária para atender à solicitação. O número de unidades de pedido para uma operação é determinístico. Pode controlar o número de unidades de pedido são consumidas por qualquer operação no Azure Cosmos DB através de um cabeçalho de resposta. 

Para fornecer um desempenho previsível, reserve débito em unidades de 100 RU/segundo. Pode [calcular as necessidades de débito da sua](request-units.md#estimating-throughput-needs) ao utilizar o Azure Cosmos DB [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner).

![Calculadora de débito][5]

Depois de ler este artigo, será capaz de responder às seguintes perguntas:

* O que são unidades de pedido e os encargos de pedido no Azure Cosmos DB?
* Como posso especificar capacidade de unidade de pedido para um contentor ou o conjunto de contentores no Azure Cosmos DB?
* Como estimar a que unidade de pedido do meu aplicativo precisa?
* O que acontece se exceder a capacidade de unidade de pedido para um contentor ou o conjunto de contentores no Azure Cosmos DB?

Como o Azure Cosmos DB é uma base de dados de vários modelos, é importante observar que este artigo é aplicável a todos os modelos de dados e APIs no Azure Cosmos DB. Este artigo usa termos genéricos, como *contentor* para se referir genericamente para um gráfico ou uma coleção e *item* para se referir genericamente para uma tabela, o documento, o nó ou a entidade.

## <a name="request-units-and-request-charges"></a>Unidades de pedido e os encargos de pedido

O Azure Cosmos DB rápidas e proporciona um desempenho previsível ao reservar recursos para satisfazer as necessidades de débito da sua aplicação. Padrões de carga e de acesso à aplicação alteram ao longo do tempo. O Azure Cosmos DB lhe permite facilmente aumentar ou diminuir a quantidade de débito reservado disponível para a aplicação.

Com o Azure Cosmos DB, o débito reservado é especificado em termos de unidades de pedido de processamento por segundo. Pode pensar unidades de pedido como moeda de débito. Reservar um número de unidades de pedido garantida estejam disponíveis para a sua aplicação numa base por segundo. Cada operação no Azure Cosmos DB, incluindo a criação de um documento, executar uma consulta e atualização de um documento, consome CPU, memória e IOPS. Ou seja, cada operação incorre um encargo de pedido, o que é expressa em unidades de pedido. Quando compreender os fatores que afetam os custos da unidade de pedido e requisitos de débito da sua aplicação, pode executar a aplicação como o custo com eficiência possível. 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Isolamento de débito em bases de dados distribuídos globalmente

Se replicar a base de dados para várias regiões, o Azure Cosmos DB fornece isolamento de débito para se certificar de que a utilização da unidade de pedido numa única região não afeta a utilização de unidades de pedido noutra região. Por exemplo, se escrever dados para uma região e ler dados a partir de outra região, as unidades de pedido que são utilizadas para efetuar a operação de escrita na região A não se afastar as unidades de pedido que são utilizadas para a operação de leitura na região B. pedido unidades não são divididas acro ss as regiões em que implementou a sua base de dados. Cada região na qual a base de dados é replicado tem o número total de unidades de pedido aprovisionadas. Para obter mais informações sobre a replicação global, consulte [como distribuir dados globalmente com o Azure Cosmos DB](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Considerações de unidade de pedido
Ao calcular o número de unidades de pedido para aprovisionar, é importante considerar as seguintes variáveis:

* **Tamanho do item**. À medida que aumenta o tamanho, também aumenta o número de unidades de pedido consumidas para ler ou escrever os dados.
* **Contagem de propriedade do item**. Partindo do princípio de padrão de indexação de todas as propriedades, as unidades consumidas para escrever um aumento de documento, o nó ou entidade à medida que aumenta a propriedade count.
* **Consistência dos dados**. Ao utilizar modelos de consistência de dados, como forte ou estagnação limitada, unidades de pedido adicionais são consumidas ler itens.
* **Propriedades indexadas**. Uma política de índice em cada contentor determina quais propriedades são indexadas por predefinição. Pode reduzir o consumo de unidade de pedido para operações de escrita ao limitar o número de propriedades indexadas ou ao ativar a indexação lento.
* **Documento de indexação**. Por predefinição, cada item é indexado automaticamente. Consumir menos unidades de pedido se optar por não indexar alguns dos seus itens.
* **Padrões de consulta**. A complexidade de uma consulta afeta quantas unidades de pedido são consumidas para uma operação. O número de consulta resulta, o número de predicados, a natureza dos predicados, o número de funções definidas pelo utilizador, o tamanho dos dados de origem, e todas as projeções de afetam o custo das operações de consulta.
* **Utilização do script**. Tal como acontece com consultas, procedimentos armazenados e acionadores consumam unidades de pedido com base na complexidade das operações que está a ser executadas. Ao desenvolver seu aplicativo, Inspecione o cabeçalho de cobrança do pedido para compreender melhor como cada operação consome a capacidade de unidade de pedido.

## <a name="estimating-throughput-needs"></a>Estimando necessidades de débito
Uma unidade de pedido é uma medida normalizada de custo de processamento de solicitação. Uma unidade de pedido único representa a capacidade de processamento necessária para ler (via ligação automática ou ID), um único item de 1 KB, que consiste em 10 valores de propriedade única (excluindo as propriedades do sistema). Um pedido para criar (inserir), substituir ou eliminar o mesmo item consome mais processamento do serviço e, portanto, requer mais unidades de pedido. 

> [!NOTE]
> Linha de base da unidade de 1 pedido para um item de 1 KB corresponde a um GET simple pela ligação automática ou ID do item.
> 
> 

Por exemplo, aqui está uma tabela que mostra o número de unidades de pedido para aprovisionar para itens com três tamanhos diferentes (de 1 KB, de 4 KB e de 64 KB) e em dois níveis de desempenho diferente (500 leituras/segundo + escritas por segundo de 100 e 500 leituras/segundo + 500 escritas por segundo). Neste exemplo, a consistência de dados está definida como **sessão**, e a política de indexação é definida como **nenhum**.

| Tamanho do item | Leituras/segundo | Escritas/segundo | Unidades de pedidos
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3 000 RU/s
| 4 KB | 500 | 100 | (500 * 1.3) + (100 * 7) = 1,350 RU/s
| 4 KB | 500 | 500 | (500 * 1.3) + (500 * 7) = 4,150 RU/s
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9,800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29,000 RU/s


### <a name="use-the-request-unit-calculator"></a>Utilizar a Calculadora de unidade de pedido
Para ajudar a otimizar suas estimativas de débito, pode utilizar uma conta baseada na web [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner). A Calculadora pode ajudar a sua estimativa os requisitos de unidade de pedido para operações comuns, incluindo:

* Item cria (gravações)
* Leituras de item
* Elimina o item
* Atualizações de item

A ferramenta também inclui suporte para estimar as necessidades de armazenamento de dados com base nos itens de exemplo que fornecer.

Para utilizar a ferramenta:

1. Carregar um ou mais itens representativos (por exemplo, um documento JSON de exemplo).
   
    ![Carregar itens para a Calculadora de unidade de pedido][2]
2. Para estimar os requisitos de armazenamento de dados, introduza o número total de itens (por exemplo, documentos, linhas ou vértices) que pretende armazenar.
3. Introduza o número de criação, leitura, atualização e operações de eliminação de que necessita (numa base por segundo). Para estimar os custos de unidade de pedido de operações de atualização de item, carregar uma cópia do item de exemplo do passo 1, que inclui atualizações de campo típico. Por exemplo, se o item atualizações normalmente modificam duas propriedades chamadas *lastLogin* e *userVisits*, copiar um item de exemplo, atualize os valores para essas duas propriedades e, em seguida, carregue o item copiado.
   
    ![Introduzir os requisitos de débito na Calculadora de unidade de pedido][3]
4. Selecione **Calculate**e, em seguida, examinar os resultados.
   
    ![Resultados de calculadora de unidade de pedido][4]

> [!NOTE]
> Se tiver de tipos de itens que são muito diferem em termos de tamanho e o número de propriedades indexadas, carregue um exemplo de cada *tipo* típico de item para a ferramenta e, em seguida, calcular os resultados.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Utilizar o cabeçalho de resposta de cobrança de pedido do Azure Cosmos DB
Cada resposta do serviço do Azure Cosmos DB inclui um cabeçalho personalizado (`x-ms-request-charge`) que contém as unidades de pedido consumidas para uma determinada solicitação. Também pode acessar esse cabeçalho através dos SDKs do Azure Cosmos DB. No SDK do .NET, **RequestCharge** é uma propriedade da **ResourceResponse** objeto. Para consultas, o Azure Cosmos DB Data Explorer no portal do Azure fornece informações de custo de pedido para consultas executadas. Para saber sobre como obter e definir débito através de APIs com vários modelos diferentes, veja [definido e obtenção de débito no Azure Cosmos DB](set-throughput.md) artigo.

Um método para estimar a quantidade de débito reservado exigida pela sua aplicação é registrar o custo da unidade de pedido associado à execução de operações típicas em relação a um item representativo, que é utilizado pela sua aplicação. Em seguida, calcule o número de operações que prevê para executar a cada segundo. Certifique-se de que também medir e incluem consultas típicas e a utilização de script do Azure Cosmos DB.

> [!NOTE]
> Se tiver de tipos de itens que são muito diferem em termos de tamanho e o número de propriedades indexadas, registe as cobranças de unidades de pedido de operação aplicável associada a cada *tipo* de item de típico.
> 
> 

Por exemplo, estes são os passos que pode efetuar:

1. Registe o custo da unidade de pedido de criação (inserir) um item típico. 
2. Registe o custo da unidade de pedido de leitura de um item típico.
3. Registe o custo da unidade de pedido de atualização de um item típico.
4. Registe o custo da unidade de pedido de consultas de itens típicos, comuns.
5. Registe o custo da unidade de pedido de quaisquer scripts personalizados (procedimentos armazenados, disparadores, funções definidas pelo utilizador) que o aplicativo utiliza.
6. Calcule as unidades de pedido necessário tendo em conta o número estimado de operações que prevê para executar a cada segundo.

## <a name="a-request-unit-estimate-example"></a>Um exemplo de estimativa de unidade de pedido
Considere o seguinte documento, o que é aproximadamente 1 KB de tamanho:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Documentos são reduzidos no Azure Cosmos DB, para que o tamanho de calculado o sistema de mensagens em fila do documento acima é um pouco menos de 1 KB.
> 
> 

A tabela seguinte mostra os custos da unidade de pedido aproximada para operações típicas neste item. (O custo da unidade de pedido aproximado parte do princípio de que o nível de consistência da conta está definido como **sessão** e que todos os itens são indexados automaticamente.)

| Operação | Cobranças de unidades de pedido |
| --- | --- |
| Criar item |~ 15 RU |
| Item de leitura |~ 1 RU |
| Item de consulta por ID |~2.5 RU |

A tabela seguinte mostra o pedido aproximado custos da unidade para consultas comuns usadas no aplicativo:

| Consulta | Cobranças de unidades de pedido | n. º de itens devolvidos |
| --- | --- | --- |
| Selecione a comida por ID |~2.5 RU |1 |
| Selecione foods por fabricante |~ 7 RU |7 |
| Selecione por grupo de comida e ordem por peso |~70 RU |100 |
| Selecione 10 foods principais num grupo de comida |~10 RU |10 |

> [!NOTE]
> Custos da unidade de pedido variam consoante o número de itens devolvidos.
> 
> 

Com essas informações, pode determinar os requisitos de unidade de pedido para esta aplicação dado o número de operações e consultas que espera por segundo:

| Consulta/operação | Número estimado por segundo | Unidades de pedido necessária |
| --- | --- | --- |
| Criar item |10 |150 |
| Item de leitura |100 |100 |
| Selecione foods por fabricante |25 |175 |
| Selecione por grupo de comida |10 |700 |
| Selecione os 10 principais |15 |Total de 150 |

Neste caso, espera que um requisito de débito médio de 1,275 RU/segundo. Arredondamento até 100 o mais próximo, semelhante 1.300 RU/segundo para esta aplicação contentor (ou conjunto de contentores).

## <a id="RequestRateTooLarge"></a> Exceder os limites de débito reservado no Azure Cosmos DB
Consumo de unidades de pedido é avaliado a uma taxa por segundo. Para aplicações que excedem a taxa de unidade de pedido aprovisionadas, as solicitações são taxa limitado até que a velocidade cai abaixo do nível de débito aprovisionado. Quando um pedido está limitado a taxa, o servidor preventivamente termina o pedido com `RequestRateTooLargeException` (código de estado HTTP 429) e retorna o `x-ms-retry-after-ms` cabeçalho. O cabeçalho indica a quantidade de tempo, em milissegundos, que o utilizador tem de aguardar antes de repetir o pedido.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Se utilizar as consultas do SDK do cliente do .NET e LINQ, a maioria das vezes, nunca precisa lidar com essa exceção. A versão atual do SDK de cliente .NET implicitamente captura essa resposta, respeita o servidor especificado cabeçalho retry-after e repete automaticamente o pedido. A menos que a sua conta está sendo acessada em simultâneo por vários clientes, a próxima repetição será concluída com êxito.

Se tiver mais do que um cliente cumulativamente e operacional acima a taxa de pedidos, o comportamento de repetição predefinida poderá ser insuficiente e o cliente gera um `DocumentClientException` com o estado de código 429 à aplicação. Em casos como esse, pode querer considerar a lidar com o comportamento de repetição e a lógica em rotinas de tratamento de erros do seu aplicativo ou aumentar o débito aprovisionado para o contentor (ou conjunto de contentores).

## <a name="next-steps"></a>Passos Seguintes
 
- Saiba como [definido e obtenção de débito no Azure Cosmos DB](set-throughput.md) utilizando o portal do Azure e SDKs.
- Saiba mais sobre [desempenho e dimensionamento testes com o Azure Cosmos DB](performance-testing.md).
- Para saber mais sobre o débito reservado com bancos de dados do Azure Cosmos DB, veja [preços do Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) e [criação de partições de dados no Azure Cosmos DB](partition-data.md).
- Para saber mais sobre o Azure Cosmos DB, veja a [documentação do Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

