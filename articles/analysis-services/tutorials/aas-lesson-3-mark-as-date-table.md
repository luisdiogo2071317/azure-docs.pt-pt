---
title: 'O Azure Analysis Services lição 3 do tutorial: Marcar como tabela de datas | Documentos da Microsoft'
description: Descreve como marcar uma tabela de datas no projeto de tutorial do Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c383fe30b8a6be3a5915f3cc1c0f5e5712ab328
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189002"
---
# <a name="mark-as-date-table"></a>Marcar como Tabela de Data

Lição 2: Obter dados, importou uma tabela de dimensões com o nome DimDate. Enquanto no seu modelo esta tabela é denominada DimDate, também pode ser conhecida como uma *tabela de datas* que contém dados de data e hora.  
  
Sempre que utilizar funções de inteligência de tempo DAX, como quando cria medidas mais tarde, deve especificar as propriedades que incluem uma *tabela de datas* e um identificador exclusivo *Coluna de datas* nessa tabela.
  
Nesta lição, marque a tabela DimDate como a *Tabela de datas* e a coluna de datas (na tabela de datas) como a *Coluna de datas* (identificador exclusivo).  

Antes de marcar a tabela de datas e a coluna de datas, este é um bom momento para fazer a manutenção do sistema para que seja mais fácil compreender o modelo. Observe, na tabela DimDate, uma coluna denominada **FullDateAlternateKey**. Esta coluna contém uma linha para cada dia em cada ano civil incluído na tabela. Utiliza muito esta coluna em fórmulas de medições e relatórios. Porém, FullDateAlternateKey não é um identificador válido para esta coluna. Deve mudar o respetivo nome para **Data**, de modo a facilitar a identificação e a inclusão em fórmulas. Sempre que possível, é recomendável mudar o nome aos objetos, como tabelas e colunas, para facilitar a sua identificação no SSDT e nas aplicações de relatório do cliente, como Power BI e Excel. 
  
Tempo estimado para concluir esta lição: **Três minutos**  
  
## <a name="prerequisites"></a>Pré-requisitos  
Este tópico faz parte de um tutorial de modelação em tabela que deve ser concluído por ordem. Antes de executar as tarefas nesta lição, deverá ter concluído a lição anterior: [Lição 2: Obter dados](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Para mudar o nome da coluna FullDateAlternateKey

1.  No designer de modelo, clique na tabela **DimDate**.

2.  Clique duas vezes no cabeçalho da coluna **FullDateAlternateKey** e, em seguida, mude o nome para **Data**.

  
### <a name="to-set-mark-as-date-table"></a>Para definir Marcar como tabela de datas  
  
1.  Selecione a coluna **Data** e, em seguida, na janela **Propriedades**, em **Tipo de dados**, confirme que **Data** está selecionado.  
  
2.  Clique no menu **Tabela**, clique em **Data**e, em seguida, clique em **Marcar como Tabela de Datas**.  
  
3.  Na caixa de diálogo **Marcar como Tabela de Datas**, na caixa de listagem **Datas**, selecione a coluna **Data** como o identificador exclusivo. Está geralmente selecionado predefinição. Clique em **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Passos seguintes?
[Lição 4: Criar relações](../tutorials/aas-lesson-4-create-relationships.md).
  
