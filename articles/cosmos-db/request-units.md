---
title: Unidades de pedido e a estimar o débito - base de dados do Azure Cosmos | Microsoft Docs
description: Saiba mais sobre como compreender, especifique e estimar os requisitos da unidade de pedido na base de dados do Azure Cosmos.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 16ccda120aef0aa892bf365403f3f0bdc1209ca3
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823728"
---
# <a name="request-units-in-azure-cosmos-db"></a>Unidades do BD Azure Cosmos de pedido

[BD do Azure do Cosmos](https://azure.microsoft.com/services/cosmos-db/) é o Microsoft globalmente multimodel base de dados distribuída. Com base de dados do Azure Cosmos, não terá alugar máquinas virtuais, implementar software ou monitorizar bases de dados. BD do Azure do Cosmos é operada e monitorizado continuamente por engenheiros superiores da Microsoft para fornecer proteção de dados, desempenho e disponibilidade trabalho. Pode aceder os dados utilizando APIs à sua escolha, tal como o [SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md), e [tabela](table-introduction.md) APIs e gráfico através de [Gremlin API](graph-introduction.md). Todas as APIs são suportadas todos os nativamente. 

A moeda de base de dados do Azure Cosmos é o *unidade de pedido (RU)*. Com unidades de pedido, não precisa de reservar as capacidades de leitura/escrita ou o aprovisionar da CPU, memória e IOPS. BD do Azure do Cosmos suporta várias APIs que tenham operações diferentes, vão de simples lê e escreve consultas complexas de gráfico. Porque nem todos os pedidos são iguais, pedidos recebem uma quantidade normalizada de unidades de pedido com base na quantidade de computação necessária para servir o pedido. O número de unidades de pedido de uma operação é determinística. Pode controlar o número de unidades de pedido que são consumidos por qualquer operação na base de dados do Azure Cosmos através de um cabeçalho de resposta. 

Para fornecer um desempenho previsível, reserve débito em unidades de 100 RU por segundo. Pode [estimar o débito tem](request-units.md#estimating-throughput-needs) utilizando a BD do Cosmos Azure [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner).

![Calculadora de débito][5]

Depois de ler este artigo, poderá responder às seguintes questões:

* Quais são unidades de pedido e custos de pedido na base de dados do Azure Cosmos?
* Como posso especificar a capacidade da unidade de pedido de um contentor ou um conjunto de contentores do BD Azure Cosmos?
* Como estimar que tem de unidade de pedido da minha aplicação?
* O que acontece se posso exceder a capacidade de unidade de pedido para um contentor ou um conjunto de contentores do BD Azure Cosmos?

Porque a BD do Cosmos do Azure é uma base de dados multimodel, é importante ter em atenção que este artigo é aplicável a todos os modelos de dados e APIs do BD Azure Cosmos. Este artigo utiliza termos genéricos como *contentor* referir-se genericamente a uma coleção ou um gráfico e *item* genericamente consultar uma tabela, o documento, o nó ou a entidade.

## <a name="request-units-and-request-charges"></a>Unidades de pedido e custos de pedido

BD do Azure do Cosmos rápido, oferece um desempenho previsível ao reservar a recursos para satisfazer as necessidades de débito da sua aplicação. Padrões de carga e acesso de aplicação alteram ao longo do tempo. BD do Cosmos do Azure pode ajudar a facilmente aumentar ou diminuir a quantidade de débito reservado disponível para a aplicação.

Com base de dados do Azure Cosmos, débito reservado é especificado em termos de unidade de pedido de processamento por segundo. Pode considerar unidades de pedido como moeda de débito. Reserve um número de unidades de pedido garantida esteja disponível para a aplicação numa base por segundo. Cada operação na BD do Cosmos do Azure, incluindo a escrever um documento, executar uma consulta e atualizar um documento, consome CPU, memória e IOPS. Ou seja, cada operação implica um custo de pedido, que é expresso em unidades de pedido. Quando compreender os fatores que afetam os encargos de unidade de pedido e requisitos de débito da sua aplicação, pode executar a aplicação como custo eficientemente possível. 

Para ajudar a começar, Azure Gestor de programa do Cosmos DB Andrew Liu aborda unidades de pedido no vídeo seguinte: <br /><br />

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Isolamento de débito nas bases de dados globalmente distribuídas

Se replicar a base de dados a mais do que uma região, base de dados do Azure Cosmos fornece isolamento de débito para se certificar de que a utilização da unidade de pedido numa região não afeta a utilização da unidade de pedido noutra região. Por exemplo, se escrever dados para uma região e ler dados a partir de outra região, as unidades de pedido que são utilizadas para efetuar a operação de escrita na região A não ter unidades não são de divisão acro na direção oposta às unidades de pedido que são utilizadas para a operação de leitura na região pedido B. ss regiões nos quais implementou a base de dados. Cada região na qual a base de dados é replicado tem o número total de unidades de pedido aprovisionado. Para obter mais informações sobre a replicação global, consulte [como distribuir dados globalmente com o Azure Cosmos DB](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>Considerações de unidade de pedido
Quando a estimar o número de unidades de pedido para aprovisionar, é importante considerar as variáveis seguintes:

* **Tamanho do item**. À medida que aumenta de tamanho, o número de unidades de pedido utilizada para ler ou escrever os dados também aumenta.
* **Contagem de propriedade do item**. Partindo do princípio de indexação predefinido de todas as propriedades, as unidades consumidas para escrever um aumento do documento, o nó ou entidade à medida que aumenta de contagem de propriedade.
* **Consistência dos dados**. Quando utilizar modelos de consistência de dados como forte ou tem um vínculo vinculada, unidades de pedido adicionais são consumidas ler itens.
* **Indexada propriedades**. Uma política de índice em cada contentor determina as propriedades que são indexadas por predefinição. Pode reduzir o consumo de unidade de pedido para operações de escrita ao limitar o número de propriedades indexadas ou ao ativar a indexação lento.
* **A indexação de documento**. Por predefinição, cada item é automaticamente indexado. Consumir unidades de pedido de menos se optar por não indexar alguns dos seus itens.
* **Padrões de consulta**. A complexidade de uma consulta afeta o número de unidades de pedido são consumidas para uma operação. O número de consulta resulta, o número de predicados, a natureza os predicados, o número de funções definidas pelo utilizador, o tamanho dos dados de origem, e todas as projecções afetam o custo das operações de consulta.
* **Utilização do script**. Tal como acontece com consultas, acionadores e procedimentos armazenados consumam unidades de pedido com base na complexidade das operações que está a ser executadas. Como desenvolver a sua aplicação, Inspecione o cabeçalho de encargos de pedido para compreender melhor a forma como cada operação consome a capacidade de unidade de pedido.

## <a name="estimating-throughput-needs"></a>A estimar necessidades de débito
Uma unidade de pedido é uma medida de custo de processamento de pedidos normalizada. Uma unidade de pedido único representa a capacidade de processamento necessária para ler (através da ligação automática ou ID) de um único item de 1 KB, que consiste de 10 valores de propriedade exclusivo (excluindo as propriedades do sistema). Um pedido de criação (insert), substituir ou eliminar as mesmas item consome mais processamento do serviço e, deste modo, necessita de mais unidades de pedido. 

> [!NOTE]
> A linha de base da unidade de pedido de 1 para um item de 1 KB corresponde a uma ação obter simple através da ligação automática ou ID do item.
> 
> 

Por exemplo, aqui está uma tabela que mostra como muitas unidades de pedido para aprovisionar para itens com três tamanhos diferentes (de 1 KB, 4 KB e 64 KB) e dois níveis de desempenho diferentes (500 lidas por segundo + 100 escritas por segundo e 500 lidas por segundo + 500 escritas por segundo). Neste exemplo, a consistência de dados está definida como **sessão**, e a política de indexação está definida como **nenhum**.

| Tamanho do item | Leituras/segundo | Escritas de paginações/segundo | Unidades de pedidos
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3,000 RU/s
| 4 KB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1,350 RU/s
| 4 KB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4,150 RU/s
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9,800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29,000 RU/s


### <a name="use-the-request-unit-calculator"></a>Utilizar a Calculadora de unidade de pedido
Para ajudar a otimizar as estimativas de débito, pode utilizar uma conta baseada na web [Calculadora de unidade de pedido](https://www.documentdb.com/capacityplanner). A Calculadora pode ajudar a estimativa os requisitos da unidade de pedido para operações comuns, incluindo:

* Item cria (escritas)
* Leituras de item
* Elimina o item
* Atualizações do item

A ferramenta também inclui suporte para estimar necessidades de armazenamento de dados baseadas em itens de exemplo que fornecer.

Para utilizar a ferramenta:

1. Carregar um ou mais itens representativos (por exemplo, um documento JSON de exemplo).
   
    ![Carregar itens para a Calculadora de unidade de pedido][2]
2. Para estimar os requisitos de armazenamento de dados, introduza o número total de itens (por exemplo, documentos, linhas ou vértices) que pretende armazenar.
3. Introduza o número de criação, leitura, atualização e operações de eliminação que necessita de (numa base por segundo). Para estimar os encargos de unidade de pedido de operações de atualização do item, carregue uma cópia do item de exemplo do passo 1, que inclui atualizações de campo típica. Por exemplo, se o item atualizações normalmente modificar duas propriedades com o nome *lastLogin* e *userVisits*, copiar um item de exemplo, atualize os valores para as duas propriedades e, em seguida, carregue o item copiado.
   
    ![Introduza os requisitos de débito na Calculadora de unidade de pedido][3]
4. Selecione **Calculate**e, em seguida, examine os resultados.
   
    ![Resultados de calculadora de unidade de pedido][4]

> [!NOTE]
> Se tiver de tipos de itens de que diferem significativamente em termos de tamanho e o número de propriedades indexadas, carregue uma amostra de cada *tipo* típica do item para a ferramenta e, em seguida, calcular os resultados.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Utilize o cabeçalho de resposta de encargos de pedido de base de dados do Azure Cosmos
Cada resposta do serviço de base de dados do Azure Cosmos inclui um cabeçalho personalizado (`x-ms-request-charge`) que contenha as unidades de pedido consumidas para um determinado pedido. Também pode aceder a este cabeçalho através os SDKs do Azure Cosmos DB. SDK .NET, **RequestCharge** é uma propriedade do **ResourceResponse** objeto. Para consultas, o Explorador de dados de base de dados do Azure Cosmos no portal do Azure fornece informações de encargos de pedido para consultas executadas.

É um método para estimar a quantidade de débito reservado exigido pela sua aplicação registar os encargos de unidade de pedido associados a execução de operações típicas num item representativo que é utilizado pela sua aplicação. Em seguida, calcule o número de operações que antecipa para efetuar a cada segundo. Lembre-se de que também medir e incluir consultas típicas e a utilização de script de BD do Cosmos do Azure.

> [!NOTE]
> Se tiver de tipos de itens de que diferem significativamente em termos de tamanho e o número de propriedades indexadas, registe a taxa de unidade de pedido de operação aplicável associada a cada *tipo* do item típica.
> 
> 

Por exemplo, estes são os passos que pode efetuar:

1. Registe a taxa de unidade de pedido de criação (inserir) um item típico. 
2. Registe a taxa de unidade de pedido de leitura de um item típico.
3. Registe a taxa de unidade de pedido de atualizar um item típico.
4. Registe a taxa de unidade de pedido de consultas de item típica, comuns.
5. Registe a taxa de unidade de pedido de quaisquer scripts personalizados (procedimentos armazenados, acionadores, funções definidas pelo utilizador) que utiliza a aplicação.
6. Calcule as unidades de pedido necessários indicadas o número estimado de operações que antecipa para ser executada a cada segundo.

## <a name="a-request-unit-estimate-example"></a>Um exemplo de estimativa de unidade de pedido
Considere o seguinte documento, o que é de aproximadamente 1 KB em tamanho:

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
> Documentos são minified na base de dados do Azure Cosmos, pelo que o tamanho calculado pelo sistema do documento acima é ligeiramente inferior a 1 KB.
> 
> 

A tabela seguinte mostra os custos de unidade de pedido aproximado de típicos operações neste item. (A taxa de unidade de pedido aproximado parte do princípio de que o nível de consistência de conta é definido como **sessão** e de que todos os itens são indexados automaticamente.)

| Operação | Encargos de unidade de pedido |
| --- | --- |
| Criar item |~ 15 RU |
| Item de leitura |~ 1 RU |
| Item de consulta por ID |~2.5 RU |

A tabela seguinte mostra a pedido aproximado encargos de unidade para consultas típicos utilizados na aplicação:

| Consulta | Encargos de unidade de pedido | n. º de itens devolvidos |
| --- | --- | --- |
| Selecione prato por ID |~2.5 RU |1 |
| Selecione foods por fabricante |~ 7 RU |7 |
| Selecione pelo grupo prato e ordem por ponderação |~70 RU |100 |
| Selecione 10 foods superiores de um grupo de prato |~10 RU |10 |

> [!NOTE]
> Os encargos de unidade de pedido variam com base no número de itens devolvidos.
> 
> 

Com esta informação, pode estimar os requisitos da unidade de pedido para esta aplicação dado o número de operações e consultas que pode espera por segundo:

| Consulta/operação | Número estimado por segundo | Unidades de pedido necessários |
| --- | --- | --- |
| Criar item |10 |150 |
| Item de leitura |100 |100 |
| Selecione foods por fabricante |25 |175 |
| Selecione pelo grupo prato |10 |700 |
| Selecione 10 principais |15 |Total de 150 |

Neste caso, espera que um requisito de débito médio de 1,275 RU por segundo. Arredondamento até 100 mais próximo, teria de aprovisionar 1,300 RU por segundo para esta aplicação contentor (ou um conjunto de contentores).

## <a id="RequestRateTooLarge"></a> Exceder os limites de débito reservado do BD Azure Cosmos
Consumo de unidade de pedido é avaliado uma taxa por segundo. Para aplicações que excedem a taxa de pedidos de aprovisionamento de unidade, pedidos são taxa limitado até que a taxa de ignora abaixo do nível de débito aprovisionado. Quando um pedido é limitado de taxa, o servidor preventivamente termina o pedido com `RequestRateTooLargeException` (código de estado HTTP 429) e devolve o `x-ms-retry-after-ms` cabeçalho. O cabeçalho indica a quantidade de tempo, em milissegundos, que o utilizador terá de aguardar antes de repetir o pedido.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Se utilizar os SDK de cliente .NET e LINQ consultas, na maioria das vezes, nunca tem de lidar com esta exceção. A versão atual do SDK de cliente .NET implicitamente intercete esta resposta, respeita o servidor especificado depois de repetir cabeçalho e repete automaticamente o pedido. A menos que a sua conta está a ser acedida em simultâneo por vários clientes, a tentativa seguinte ocorrerá será bem sucedida.

Se tiver mais do que um cliente cumulativamente e a funcionar acima a taxa de pedidos, o comportamento de repetição predefinidos poderão ser suficiente e o cliente emite um `DocumentClientException` com o estado de código 429 à aplicação. Em casos como esta, poderá considerar a processar o comportamento de repetição e a lógica na rotinas de tratamento de erros da aplicação ou aumentar o débito aprovisionado para o contentor (ou conjunto de contentores).

## <a name="next-steps"></a>Passos Seguintes
 
- Saiba como [definir e obter débito do BD Azure Cosmos](set-throughput.md) utilizando o portal do Azure e SDKs.
- Saiba mais sobre [desempenho e dimensionamento de teste com base de dados do Azure Cosmos](performance-testing.md).
- Para saber mais sobre débito reservado com bases de dados do Azure Cosmos DB, consulte o artigo [preços de base de dados do Azure Cosmos](https://azure.microsoft.com/pricing/details/cosmos-db/) e [a criação de partições de dados na base de dados do Azure Cosmos](partition-data.md).
- Para saber mais sobre a BD do Cosmos do Azure, consulte o [documentação de base de dados do Azure Cosmos](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

