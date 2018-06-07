---
title: Modelar relações na estrutura da tabela de armazenamento do Azure | Microsoft Docs
description: Compreenda o processo de modelação quando conceber a sua solução de armazenamento de tabela.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 561281375273135009a956fd27dfe9f193ab92ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661069"
---
# <a name="modeling-relationships"></a>Relações de modelação
Este artigo explica o processo de modelação para o ajudar a conceber as suas soluções de armazenamento de tabelas do Azure.

Criação de modelos de domínio é um passo chave na estrutura dos sistemas complexos. Normalmente, o processo de modelação é utilizado para identificar as entidades e as relações entre os mesmos como uma forma de compreender o domínio de empresa e informar sobre a estrutura do seu sistema. Esta secção centra-se na forma como pode traduzir alguns dos tipos de relação comuns encontrados nos modelos de domínio para as estruturas do serviço tabela. O processo de mapeamento de um modelo de dados lógico ao físico NoSQL baseada em modelo de dados-é diferente da utilizada ao conceber a base de dados relacional. Estrutura de bases de dados relacionais, normalmente, assume um processo de normalização de dados otimizado para minimizando a redundância – e uma capacidade de consulta declarativa que deduz como a implementação de como funciona a base de dados.  

## <a name="one-to-many-relationships"></a>Relações um-para-muitos
Relações um-para-muitos entre objetos de domínio do negócio ocorrerem com frequência: por exemplo, um departamento tem muitos colaboradores. Existem várias formas de implementar relações um-para-muitos no serviço tabela cada com os profissionais de TI e contras que podem ser relevantes para o cenário em particular.  

Considere o exemplo de uma grande empresa múltiplos national com dezenas de milhares de departamentos e entidades de empregado onde cada departamento tem muitos colaboradores e cada empregado como associados com um departamento específico. É uma abordagem armazenar departamento separado e as entidades de empregado como estas:  


![Armazenar departamento separado e as entidades de empregado](media/storage-table-design-guide/storage-table-design-IMAGE01.png)

Este exemplo mostra uma relação um-para-muitos implícita entre tipos com base no **PartitionKey** valor. Cada departamento pode ter muitos colaboradores.  

Este exemplo mostra também uma entidade de departamento e respetivos entidades de empregado relacionadas com a mesma partição. Pode optar por utilizar partições diferentes, tabelas ou contas de armazenamento, mesmo que para os tipos de entidade diferentes.  

Uma abordagem alternativa é denormalize os seus dados e armazenar as entidades de empregado apenas com os dados de departamento denormalized conforme mostrado no exemplo seguinte. Neste cenário específico, esta abordagem denormalized não pode ser a melhor se tiver um requisito para poder alterar os detalhes de um Gestor de departamento porque para efetuar este procedimento, tem de atualizar todos os funcionários do departamento.  

![entidade de empregado](media/storage-table-design-guide/storage-table-design-IMAGE02.png)

Para obter mais informações, consulte o [padrão Denormalization](table-storage-design-patterns.md#denormalization-pattern) mais à frente neste guia.  

A tabela seguinte resume as profissionais de TI e contras de cada uma das abordagens descritas acima para armazenar o empregado e entidades de departamento que tenham uma relação um-para-muitos. Deve também considerar a frequência esperada desempenhar várias operações: pode ser aceitável para ter uma estrutura que inclui uma operação dispendiosa, se essa operação só acontece com pouca frequência.  

<table>
<tr>
<th>Abordagem</th>
<th>Profissionais de TI</th>
<th>Contras</th>
</tr>
<tr>
<td>Separe os tipos de entidade, a mesma partição, a mesma tabela</td>
<td>
<ul>
<li>Pode atualizar uma entidade de departamento com uma única operação.</li>
<li>Pode utilizar um EGT para manter a consistência, se tiver um requisito para modificar uma entidade de departamento sempre que atualizar/insert/eliminar uma entidade do empregado. Por exemplo, se mantém uma contagem de empregado departamentais para cada departamento.</li>
</ul>
</td>
<td>
<ul>
<li>Terá de obter um empregado e uma entidade de departamento para algumas atividades de cliente.</li>
<li>Operações de armazenamento acontecer na mesma partição. Em volumes de transação elevada, isto pode resultar num hotspot.</li>
<li>Não é possível mover um empregado para departamento novo utilizando um EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Tipos de entidade separado, partições diferentes ou tabelas ou contas de armazenamento</td>
<td>
<ul>
<li>Pode atualizar uma entidade do departamento ou uma entidade de empregado com uma única operação.</li>
<li>Em volumes de transação elevada, isto pode ajudar a distribuídos a carga por mais partições.</li>
</ul>
</td>
<td>
<ul>
<li>Terá de obter um empregado e uma entidade de departamento para algumas atividades de cliente.</li>
<li>Não é possível utilizar EGTs para manter a consistência quando a atualização/insert/eliminar um empregado e a atualização de um departamento. Por exemplo, ao atualizar uma contagem do empregado na entidade de um departamento.</li>
<li>Não é possível mover um empregado para departamento novo utilizando um EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalize para o tipo de entidade única</td>
<td>
<ul>
<li>Pode obter todas as informações que necessárias com um único pedido.</li>
</ul>
</td>
<td>
<ul>
<li>Poderá ser Caro manter a consistência se precisar de atualizar as informações de departamento (Isto iria requerem que Atualize todos os funcionários do departamento de um).</li>
</ul>
</td>
</tr>
</table>

Como escolher entre estas opções e de que os profissionais de TI e contras são mais significativas, depende os cenários de aplicação específica. Por exemplo, com que frequência modificar entidades departamento; todas as consultas de empregado precisa as informações adicionais departamentais; como fechar está os limites de escalabilidade no seu partições ou a sua conta de armazenamento?  

## <a name="one-to-one-relationships"></a>Relações um para um
Modelos de domínio podem incluir um para um relações entre entidades. Se precisar de implementar uma relação unívoca no serviço tabela, também tem de escolher como ligar as duas entidades relacionadas quando precisar de recuperar os dois. Esta ligação pode ser implícita, com base numa convenção de valores da chave ou explícito ao armazenar uma ligação sob a forma de **PartitionKey** e **RowKey** valores em cada entidade para a entidade relacionada. Para ver um debate do se deve armazenar as entidades relacionadas na mesma partição, consulte a secção [um-para-muitos relações](#one-to-many-relationships).  

Também existem considerações de implementação que podem originar a implementação relações um para um no serviço tabela:  

* Processamento de entidades grandes (para obter mais informações, consulte [padrão de entidades grandes](table-storage-design-patterns.md#large-entities-pattern)).  
* Implementar controlos de acesso (para obter mais informações, consulte [controlar o acesso com assinaturas de acesso partilhado](#controlling-access-with-shared-access-signatures)).  

## <a name="join-in-the-client"></a>Associação no cliente
Embora existam formas para modelar relações no serviço tabela, deve não se esqueça que as duas razões prime para utilizar o serviço de tabela são escalabilidade e desempenho. Se encontrar que são modelação muitas relações comprometer o desempenho e escalabilidade da sua solução, deverá solicitar a si se é necessário compilar todas as relações de dados para a estrutura da tabela. Poderá simplificar a estrutura e melhorar a escalabilidade e o desempenho da sua solução se permitir que a aplicação de cliente, efetuar quaisquer associações necessárias.  

Por exemplo, se tiver pequenas tabelas que contêm dados que não se altera com frequência, em seguida, pode obter estes dados uma vez e cache-la no cliente. Isto pode evitar e voltas repetidas para obter os mesmos dados. Nos exemplos que consultou neste guia, o conjunto de departamentos numa organização pequeno é provável que sejam pequenas e alterar raramente tornando um bom candidato para dados que a aplicação cliente pode transferir uma vez e cache como aspeto dos dados.  

## <a name="inheritance-relationships"></a>Relações de herança
Se a aplicação de cliente utiliza um conjunto de classes que fazem parte de uma relação de herança para representar as entidades de negócio, facilmente, pode manter as entidades no serviço tabela. Por exemplo, pode ter o seguinte conjunto de classes definidas na sua aplicação de cliente onde **pessoa** é uma classe abstracta.

![Pessoa classe abstrata](media/storage-table-design-guide/storage-table-design-IMAGE03.png)

Pode manter instâncias das classes concretas duas no serviço tabela utilizando uma única tabela de pessoa utilizar entidades em que aspeto este:  

![Tabela de pessoa](media/storage-table-design-guide/storage-table-design-IMAGE04.png)

Para obter mais informações sobre como trabalhar com diversos tipos de entidade na mesma tabela no código de cliente, consulte a secção [trabalhar com tipos de entidade heterogéneos](#working-with-heterogeneous-entity-types) mais à frente neste guia. Isto fornece exemplos de como reconhecer o tipo de entidade no código de cliente.  


## <a name="next-steps"></a>Passos Seguintes

- [Padrões de conceção da tabela](table-storage-design-patterns.md)
- [Conceção para consultas](table-storage-design-for-query.md)
- [Encriptar dados da tabela](table-storage-design-encrypt-data.md)
- [Estrutura de modificação de dados](table-storage-design-for-modification.md)