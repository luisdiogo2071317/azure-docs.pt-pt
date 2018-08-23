---
title: Construção de cadeias de caracteres de filtro para o designer de tabela | Documentos da Microsoft
description: Construção de cadeias de caracteres de filtro para o designer de tabela
services: visual-studio-online
author: ghogen
manager: douge
assetId: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 3ed3e0829932a6db37b4bd48627b68480f5d7343
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055959"
---
# <a name="constructing-filter-strings-for-the-table-designer"></a>Construção de cadeias de caracteres de filtro para o Designer de tabela
## <a name="overview"></a>Descrição geral
Para filtrar dados numa tabela do Azure que é apresentado no Visual Studio **Designer de tabela**, construir uma cadeia de filtro e introduza-o no campo de filtro. A sintaxe da cadeia de caracteres de filtro é definida pelo WCF Data Services e é semelhante a uma cláusula WHERE de SQL, mas é enviada para o serviço de tabela através de um pedido HTTP. O **Designer de tabela** lida com a codificação apropriada para, portanto, para filtrar num valor de propriedade pretendida, só tem de introduzir o nome da propriedade, operador de comparação, valor dos critérios e, opcionalmente, booleano operador no campo de filtro. Não é necessário incluir a opção de consulta $filter, tal como faria se foram construir um URL para consultar a tabela através da [referência de API de REST de serviços de armazenamento](http://go.microsoft.com/fwlink/p/?LinkId=400447).

O WCF Data Services baseiam-se a [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Para obter detalhes sobre a opção de consulta de filtro do sistema (**$filter**), consulte a [especificação de convenções de URI do OData](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Operadores de Comparação
Os seguintes operadores lógicos são suportados para todos os tipos de propriedade:

| Operador lógico | Descrição | Cadeia de caracteres de filtro de exemplo |
| --- | --- | --- |
| EQ |Igual a |City eq "Redmond" |
| gt |Maior que |Preço gt 20 |
| ge |Maior que ou igual a |Preço ge 10 |
| lt |Menos do que |Preço lt 20 |
| Le |Menor ou igual |Le de preço 100 |
| ne |Não é igual a |Cidade ne 'London' |
| e |E |Le de preço 200 e preço gt 3.5 |
| ou |Ou |Le de preço 3.5 ou preço gt 200 |
| não |não |não isAvailable |

Ao construir uma cadeia de filtro, as seguintes regras são importantes:

* Utilize os operadores lógicos para comparar uma propriedade para um valor. Tenha em atenção que não é possível comparar uma propriedade para um valor dinâmico; um lado da expressão tem de ser uma constante.
* Todas as partes da cadeia de filtro são sensíveis a maiúsculas e minúsculas.
* O valor constante tem de ser do mesmo tipo de dados da propriedade para que o filtro devolva resultados válidos. Para obter mais informações sobre os tipos de propriedade suportados, veja [Noções Básicas sobre o Modelo de Dados do Serviço Tabela](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtragem nas propriedades de cadeia de caracteres
Quando filtrar nas propriedades de cadeia de caracteres, coloque a constante de cadeia de caracteres entre plicas.

O exemplo seguinte filtra no **PartitionKey** e **RowKey** propriedades; sem chave adicional propriedades também poderiam ser adicionadas para a cadeia de filtro:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Pode incluir cada expressão de filtro entre parênteses, embora não seja necessário:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Tenha em atenção que o serviço de tabela não suporta consultas de caráter universal e não são suportados no Table Designer seja. No entanto, pode executar o prefixo que corresponda ao utilizar os operadores de comparação no prefixo pretendido. O exemplo seguinte devolve entidades com um início de propriedade LastName com a letra "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrar propriedades numéricas
Para filtrar num número inteiro ou o número de vírgula flutuante, especifique o número sem aspas.

Este exemplo retorna todas as entidades com uma propriedade de idade cujo valor é superior a 30:

    Age gt 30

Este exemplo retorna todas as entidades com uma propriedade de AmountDue cujo valor é menor ou igual a 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrar Propriedades booleanas
Para filtrar num valor booleano, especifique **true** ou **falso** sem aspas.

O exemplo seguinte devolve todas as entidades em que a propriedade IsActive está definida como **true**:

    IsActive eq true

Também pode escrever esta expressão de filtro sem o operador lógico. No exemplo a seguir, o serviço de tabela também devolve todas as entidades em que é IsActive **true**:

    IsActive

Para retornar todas as entidades onde IsActive for false, pode usar o não operador:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtragem nas propriedades de DateTime
Para filtrar num valor de DateTime, especifique a **datetime** palavra-chave, seguido da constante de data/hora plicas. A constante de data/hora tem de estar no formato UTC combinado, conforme descrito em [Formatar valores de propriedades de DateTime](http://go.microsoft.com/fwlink/p/?LinkId=400449).

O exemplo seguinte devolve entidades em que a propriedade CustomerSince é igual a 10 de Julho de 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
