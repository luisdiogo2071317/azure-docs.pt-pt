---
title: Importar um ficheiro do Power BI Desktop para Azure Analysis Services | Microsoft Docs
description: Descreve como importar um ficheiro do Power BI Desktop (pbix) utilizando o portal do Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aea6f3efcf3740527c43b75a30caadf6b2a8b623
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601076"
---
# <a name="import-a-power-bi-desktop-file"></a>Importar um ficheiro do Power BI Desktop

Pode importar um modelo de dados num ficheiro Power BI Desktop (pbix) para o Azure Analysis Services. Metadados do modelo, dados em cache e ligações de origem de dados são importadas. Relatórios e visualizações não foram importadas. Importar dados de modelos do Power BI Desktop estão no nível de compatibilidade do 1400.

**Restrições**   
- O modelo de pbix pode ligar ao **SQL Database do Azure** e **Azure SQL Data Warehouse** apenas origens de dados. 
- O modelo de pbix não pode ter direta ou ligações de DirectQuery. 
- Importação poderá falhar se o modelo de dados pbix contém metadados que não é suportado no Analysis Services.

## <a name="to-import-from-pbix"></a>Para importar a partir de pbix

1. No seu servidor **descrição geral** > **Web designer**, clique em **abra**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. No **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. No **novo modelo**, escreva um nome de modelo e, em seguida, selecione o ficheiro do Power BI Desktop.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. No **importação**, localize e selecione o ficheiro.

     ![Ligar a caixa de diálogo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Consulte também

[Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
[Ligar ao Azure Analysis Services](analysis-services-connect.md)  
