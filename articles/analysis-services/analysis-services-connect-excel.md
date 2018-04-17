---
title: Se ligam aos serviços de análise do Azure com o Excel | Microsoft Docs
description: Saiba como ligar a um servidor de Analysis Services do Azure utilizando o Excel.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8414597c2c394e0b642ff47cba79c87488f56b24
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="connect-with-excel"></a>Ligar com o Excel

Assim que tiver criado um servidor no Azure e implementar um modelo de tabela, está pronto para ligar e começar a explorar os dados.


## <a name="connect-in-excel"></a>Ligar no Excel

Ligar a um servidor no Excel é suportada utilizando obter dados no Excel 2016. A ligação utilizando o Assistente de importação de tabelas no Power Pivot não é suportada. 

**Para se ligar no Excel 2016**

1. No Excel 2016, no **dados** do Friso, clique em **obter dados externos** > **de outras origens** > **do Analysis Services** .

2. No Assistente de ligação de dados, no **nome do servidor**, introduza o nome do servidor, incluindo o protocolo e o URI. Em seguida, no **credenciais de início de sessão**, selecione **utilize o seguinte nome de utilizador e palavra-passe**e, em seguida, escreva o nome de utilizador organizacionais, por exemplo nancy@adventureworks.come a palavra-passe.

    > [!NOTE]
    > Se iniciar sessão com um Account Microsoft Live ID, Yahoo, Gmail, etc., ou é necessários para iniciar sessão com a autenticação multifator, deixe o campo de palavra-passe em branco. É-lhe pedida uma palavra-passe depois de clicar em seguinte.

    ![Ligar a partir do início de sessão de Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. No **selecionar base de dados e tabela**, selecione a base de dados e o modelo ou perspetiva e, em seguida, clique em **concluir**.
   
    ![Ligar a partir de modelo selecione do Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ver também
[Bibliotecas de cliente](analysis-services-data-providers.md)   
[Gerir o seu servidor](analysis-services-manage.md)     


