---
title: 'O Azure Analysis Services Lição 4 do tutorial: Criar relações | Documentos da Microsoft'
description: Descreve como criar relações no projeto de tutorial do Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d37b30708e3817460e5aa01f045c1e9f9032532e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117746"
---
# <a name="create-relationships"></a>Criar relações

Nesta lição, vai verificar as relações que foram criadas automaticamente quando importou dados e vai adicionar relações novas entre tabelas diferentes. Uma relação é uma ligação entre duas tabelas que estabelece a forma como os dados nas mesmas devem ser correlacionados. Por exemplo, as tabelas DimProduct DimProductSubcategory têm uma relação com base no facto de cada produto pertencer a uma subcategoria. Para saber mais, veja [Relationships](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular) (Relações).
  
Tempo estimado para concluir esta lição: **10 minutos**  
  
## <a name="prerequisites"></a>Pré-requisitos  
Este tópico faz parte de um tutorial de modelação em tabela que deve ser concluído por ordem. Antes de executar as tarefas nesta lição, deverá ter concluído a lição anterior: [Lição 3: Marcar como tabela de datas](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Rever relações existentes e adicionar novas  
Quando utilizou Get Data para importar dados, obteve sete tabelas da base de dados AdventureWorksDW2014. Em geral, quando importa dados de uma origem relacional, as relações existentes são importadas automaticamente em conjunto com os dados. Para obter dados criar automaticamente relações no modelo de dados efetuada, tem de existir relações entre tabelas na origem de dados.

Antes de continuar a criar o seu modelo, deve confirmar se as relações entre as tabelas foram criadas adequadamente. Neste tutorial, também vai adicionar três relações novas.  

  
#### <a name="to-review-existing-relationships"></a>Para rever relações existentes  
  
1.  Clique no menu **Modelo** > **Vista de Modelo** > **Vista de Diagrama**.  

    O estruturador de modelos aparece agora na Vista de Diagrama, um formato gráfico que apresenta todas as tabelas que importou com linhas entre si. Estas linhas indicam as relações que foram criadas automaticamente quando importou os dados.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    > [!NOTE]
    > Se não vir quaisquer relações entre tabelas, provavelmente significa que não existem relações entre essas tabelas na origem de dados.

    Utilize os controlos de minimapa no canto inferior direito do estruturador de modelos para incluir o máximo de tabelas possível. Também pode clicar e arrastar as tabelas para diferentes localizações, aproximando-as ou dispondo-as numa determinada ordem. Mover as tabelas não afeta as relações que existem entre as mesmas. Para ver todas as colunas de uma tabela específica, clique e arraste uma extremidade dessa tabela para expandi-la ou diminuí-la.  
  
2.  Clique na linha sólida entre as tabelas **DimCustomer** e **DimGeography**. A linha sólida entre estas duas tabelas mostra que esta relação está ativa, ou seja, é utilizada por predefinição ao calcular fórmulas DAX.  
  
    Repare que a coluna **GeographyKey** da tabela **DimCustomer** e a coluna **GeographyKey** da tabela **DimGeography** aparecem agora ambas dentro de uma caixa. Estas colunas são utilizadas na relação. As propriedades da relação também aparecem agora na janela **Propriedades**.  
  
    > [!TIP]  
    > Para além de utilizar o estruturador de modelos na vista de diagrama, também pode utilizar a caixa de diálogo Gerir Relações para mostrar as relações entre todas as tabelas num formato de tabela. No Explorador de Modelos em Tabela, clique com o botão direito do rato em **Relações** > **Gerir Relações**.
  
3.  Verifique se foram criadas as relações seguintes quando cada uma das tabelas foi importada da base de dados AdventureWorksDW:  
  
    |Ativa|Tabela|Tabela de Pesquisa Relacionada|  
    |----------|---------|------------------------|  
    |Sim|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Sim|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Sim|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Sim|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Sim|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Se qualquer uma das relações estiver em falta, verifique se o que seu modelo inclui as tabelas seguintes: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory e FactInternetSales. Se forem importadas em momentos diferentes tabelas da mesma ligação de origem de dados, as relações entre essas tabelas têm de ser criadas manualmente. Se não forem apresentadas relações, significa que estas não existem na origem de dados. Pode criá-las manualmente no modelo de dados.

### <a name="take-a-closer-look"></a>Uma visão mais detalhada
Na Vista de Diagrama, repare numa seta, num asterisco e num número nas linhas que mostram as relações entre as tabelas.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

A seta mostra a direção do filtro. O asterisco mostra que esta tabela é o lado muitos na cardinalidade da relação e o “um” mostra que esta tabela é o lado um da relação. Se tiver de editar uma relação, por exemplo, alterar a direção do filtro ou a cardinalidade da relação, faça duplo clique na linha da relação para abrir a caixa de diálogo Editar Relação.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Estas funcionalidades destinam-se a modelação de dados avançada e não estão no âmbito deste tutorial. Para saber mais, veja [Bi-directional cross filters for tabular models in Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services) (Filtros cruzados bidirecionais para modelos em tabela no Analysis Services).

Em alguns casos, poderá ter de criar relações adicionais entre tabelas do seu modelo, de modo a suportar determinadas lógicas de negócio. Neste tutorial, tem de criar três relações adicionais entre as tabelas FactInternetSales e DimDate.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Para adicionar relações novas entre tabelas  
  
1.  No estruturador de modelos, na tabela **FactInternetSales**, clique sem soltar na coluna **OrderDate**, arraste o cursor para a coluna **Date** da tabela **DimDate** e largue.  

    Aparece uma linha sólida, que mostra que criou uma relação ativa entre a coluna **OrderDate** da tabela **Internet Sales** e a coluna **Date** da tabela **Date**. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > A cardinalidade e a direção do filtro entre a tabela principal e a tabela de pesquisa relacionada é selecionada automaticamente quando as relações são criadas.  
  
2.  Na tabela **FactInternetSales**, clique sem soltar na coluna **DueDate**, arraste o cursor para a coluna **Date** da tabela **DimDate** e largue.  
  
    Aparece uma linha pontilhada que mostra que criou uma relação inativa entre a coluna **DueDate** da tabela **FactInternetSales** e a coluna **Date** da tabela **DimDate**. Pode ter várias relações entre tabelas, mas só pode estar ativa uma relação de cada vez. As relações inativas podem ser ativadas, para realizar agregações especiais em expressões DAX personalizadas.  
  
3.  Por fim, crie mais uma relação. Na tabela **FactInternetSales**, clique sem soltar na coluna **ShipDate**, arraste o cursor para a coluna **Date** da tabela **DimDate** e largue.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Passos seguintes?
[Lição 5: Criar colunas calculadas](../tutorials/aas-lesson-5-create-calculated-columns.md).
  
  
  
