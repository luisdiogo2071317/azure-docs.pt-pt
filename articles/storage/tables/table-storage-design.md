---
title: Design dimensionável e tabelas performant table storage do Azure. | Microsoft Docs
description: Design dimensionável e tabelas performant table storage do Azure.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: ba48283371ac099b162aeb3cbffd6127ccce1676
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661045"
---
# <a name="design-scalable-and-performant-tables"></a>Criar tabelas escaláveis e de desempenho

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

A estrutura dimensionável e performant tabelas, tem de considerar os fatores, tais como o desempenho, escalabilidade e o custo. Se anteriormente tiver concebido esquemas para bases de dados relacionais, estas considerações estão familiarizadas, mas enquanto existirem alguns semelhanças entre o modelo de armazenamento do serviço de Azure Table e modelos relacionais, também existem diferenças importantes. Estas diferenças normalmente levar a diferentes estruturas que podem parecer counter-intuitive ou errado para alguém familiarizado com bases de dados relacionais, ainda fazem sentido se está a conceber um arquivo de chave/valor NoSQL, tais como o serviço de Azure Table. Muitos dos seus diferenças de estrutura refletem o facto de que o serviço tabela foi concebido para suportar aplicações de escala da nuvem que podem conter billions das entidades (ou linhas na terminologia de base de dados relacional) de dados ou para conjuntos de dados tem de suportar a transação elevada volumes. Por conseguinte, tem de pensar em forma diferente como armazena os dados e a compreender como funciona o serviço de tabela. Um arquivo de dados NoSQL devidamente concebido pode ativar a sua solução de dimensionar muito mais e a um custo mais baixo de uma solução que utiliza uma base de dados relacional. Este guia ajuda-o com estes tópicos.  

## <a name="about-the-azure-table-service"></a>Sobre o serviço de tabelas do Azure
Esta secção realça algumas das principais funcionalidades do serviço tabela que são especialmente relevantes para estruturar para desempenho e escalabilidade. Se estiver familiarizado com o Storage do Azure e o serviço de tabela, leia primeiro [introdução ao Storage do Microsoft Azure](../../storage/common/storage-introduction.md) e [introdução ao armazenamento de tabelas do Azure através do .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) antes de ler o resto deste artigo . Embora o foco deste guia é o serviço tabela, inclui debate dos serviços do Azure fila e Blob, e como pode utilizá-los com o serviço de tabela.  

O que é o serviço tabela? Como seria de esperar do nome, o serviço tabela utiliza um formato de tabela para armazenar dados. Na terminologia padrão, cada linha da tabela representa uma entidade e as colunas armazenam várias propriedades dessa entidade. Cada entidade tem um par de chaves para identificar de forma exclusiva e uma coluna de carimbo que o serviço tabela utiliza para controlar quando a entidade foi atualizada pela última vez. O carimbo é aplicado automaticamente e manualmente não é possível substituir o timestamp com um valor arbitrário. O serviço de tabela utiliza este timestamp last-modified (LMT) para gerir a simultaneidade otimista.  

> [!NOTE]
> As operações de REST API do serviço tabela também devolvem uma **ETag** valor que deriva do LMT. Este documento utiliza os termos de licenciamento ETag e LMT-no alternadamente porque consultarem os mesmos dados subjacentes.  
> 
> 

O exemplo seguinte mostra um design de tabela simples para armazenar as entidades dos empregados e departamento. Muitos dos exemplos mostrados posteriormente neste guia são baseados nesta estrutura simple.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Carimbo de data/hora</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Apelido</th>
<th>Idade</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Apelido</th>
<th>Idade</th>
<th>Email</th>
</tr>
<tr>
<td>Jun.</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Departamento</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Nome do Departamento</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Vendas</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Apelido</th>
<th>Idade</th>
<th>Email</th>
</tr>
<tr>
<td>Manuel</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Até ao momento, estes dados parecem ser semelhantes a uma tabela da base de dados relacional com as principais diferenças que está a ser colunas obrigatórias e a capacidade de armazenar vários tipos de entidade na mesma tabela. Além disso, cada uma das propriedades definidas pelo utilizador, tais como **FirstName** ou **idade** tem um tipo de dados, tais como número inteiro ou uma cadeia, apenas como uma coluna na base de dados relacional. Embora ao contrário de uma base de dados relacional, a natureza sem esquema do serviço tabela significa que uma propriedade não precisa de ter os mesmo tipo em cada entidade de dados. Para armazenar os tipos de dados complexas numa única propriedade, tem de utilizar um formato serializado como JSON ou XML. Para obter mais informações sobre os tipos de dados de serviço suportado, tais como tabela, intervalos de data suportados, regras de nomenclatura e restrições de tamanho, consulte [compreender o modelo de dados do serviço tabela](http://msdn.microsoft.com/library/azure/dd179338.aspx).

À sua escolha de **PartitionKey** e **RowKey** são fundamentais para estruturar tabela bom. Cada entidade armazenada numa tabela tem de ter uma combinação única de **PartitionKey** e **RowKey**. Tal como acontece com as chaves de uma tabela de base de dados relacional, o **PartitionKey** e **RowKey** valores são indexados para criar um índice em cluster para ativar look-ups rápidas. No entanto, o serviço de tabela não cria índices secundários, por isso, **PartitionKey** e **RowKey** são as propriedades indexadas apenas. Alguns dos padrões descritos [padrões de conceção de tabela](table-storage-design-patterns.md) ilustrar a forma como pode contornar esta limitação aparente.  

Uma tabela é composta por uma ou mais partições e muitas das decisões de conceção que efetuar serão à volta de escolher um adequado **PartitionKey** e **RowKey** para otimizar a sua solução. Uma solução pode ser composto por uma única tabela que contém todas as entidades organizadas em partições, mas normalmente uma solução tem várias tabelas. As tabelas ajudá-lo a logicamente organizar as entidades, ajudar a gerir o acesso aos dados por listas de controlo de acesso e pode colocar uma tabela inteira utilizando uma operação de armazenamento única.  

## <a name="table-partitions"></a>Partições da tabela
O nome da conta, o nome de tabela, e **PartitionKey** em conjunto identificar a partição no âmbito do serviço de armazenamento onde o serviço tabela armazena a entidade. Bem como a ser parte do esquema de endereçamento para entidades, partições definem um âmbito para transações (consulte [entidade grupo transações](#entity-group-transactions) abaixo), e a base da forma como dimensiona serviço tabela. Para obter mais informações sobre as partições, consulte [metas de desempenho e escalabilidade do Storage do Azure](../../storage/common/storage-scalability-targets.md).  

No serviço tabela, um nó individual dos serviços de um ou mais conclua partições e as escalas de serviço, dinamicamente o balanceamento de carga partições entre nós. Se um nó estiver sob carga, o serviço de tabela pode *dividir* o intervalo de partições servidos esse nó em nós diferentes; quando subsides de tráfego, o serviço pode *intercalação* fazer uma cópia de intervalos de partição de nós silenciosos para um único nó.  

Para obter mais informações sobre os detalhes internos do serviço tabela e, em especial, como o serviço gere partições, consulte o documento [armazenamento do Microsoft Azure: A nuvem armazenamento serviço de elevada disponibilidade com consistência forte](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

## <a name="entity-group-transactions"></a>Transações de grupo de entidade
No serviço tabela, transações de grupo de entidade (EGTs) são o mecanismo apenas incorporado para executar atualizações atómicas entre várias entidades. EGTs são, por vezes, também denominados *lote de transações*. EGTs só pode operar em entidades armazenadas na mesma partição (ou seja, partilham a mesma chave de partição numa tabela especificada). Por isso, sempre que necessitar que atómico transacional entre várias entidades, tem de garantir que as entidades estão a ser a mesma partição. Isto é frequentemente um motivo para manter os vários tipos de entidade na mesma tabela (e partição) e não a utilizar várias tabelas para tipos de entidade diferente. Um único EGT pode operar no máximo de 100 entidades.  Se submeter vários EGTs em simultâneo para processamento, é importante certificar-se de que esses EGTs não operam entidades que são comuns em EGTs; caso contrário, processamento pode sofrer um atraso.

EGTs também introduzir um compromisso potencial para avaliar na sua estrutura. Ou seja, utilizar mais partições aumenta a escalabilidade da sua aplicação, porque o Azure tem mais de oportunidades de balanceamento de carga pedidos entre nós. Mas utilizar mais partições poderá limitar a capacidade da sua aplicação para efetuar transações atómicas e manter a consistência forte para os seus dados. Além disso, existem metas de escalabilidade específico no nível de uma partição que pode limitar o débito de transações que pode esperar para um único nó. Para obter mais informações sobre os destinos de escalabilidade para contas do storage do Azure e o serviço tabela, consulte [metas de desempenho e escalabilidade do Storage do Azure](../../storage/common/storage-scalability-targets.md).   

## <a name="capacity-considerations"></a>Considerações sobre a capacidade
A tabela seguinte descreve alguns dos valores da chave a ter em consideração quando está a conceber uma solução de serviço tabela:  

| Capacidade total de uma conta de armazenamento do Azure | 500 TB |
| --- | --- |
| Número de tabelas de uma conta de armazenamento do Azure |Limitado apenas pela capacidade da conta de armazenamento |
| Número de partições numa tabela |Limitado apenas pela capacidade da conta de armazenamento |
| Número de entidades numa partição |Limitado apenas pela capacidade da conta de armazenamento |
| Tamanho de uma entidade individuais |Com um máximo de 255 propriedades até 1 MB (incluindo o **PartitionKey**, **RowKey**, e **Timestamp**) |
| Tamanho do **PartitionKey** |A cadeia até 1 KB de tamanho |
| Tamanho do **RowKey** |A cadeia até 1 KB de tamanho |
| Tamanho de uma transação de grupo de entidade |Uma transação pode incluir um máximo de 100 entidades e o payload tem de ser inferior a 4 MB de tamanho. Uma vez, uma EGT só pode atualizar uma entidade. |

Para obter mais informações, consulte [compreender o modelo de dados do serviço tabela](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

## <a name="cost-considerations"></a>Considerações sobre o custo
O Table storage é relativamente barato, mas deve incluir estimativas de custo de utilização da capacidade e a quantidade de transações como parte da sua avaliação de qualquer solução de serviço tabela. No entanto, muitos cenários, armazenar dados denormalized ou duplicados para melhorar o desempenho ou escalabilidade da sua solução é uma abordagem válida. Para obter mais informações sobre preços, consulte [preços do Storage do Azure](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="next-steps"></a>Passos Seguintes

- [Padrões de conceção da tabela](table-storage-design-patterns.md)
- [Relações de modelação](table-storage-design-modeling.md)
- [Conceção para consultas](table-storage-design-for-query.md)
- [Encriptar dados da tabela](table-storage-design-encrypt-data.md)
- [Estrutura de modificação de dados](table-storage-design-for-modification.md)