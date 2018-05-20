---
title: Se ligam aos serviços de análise do Azure com o Excel | Microsoft Docs
description: Saiba como ligar a um servidor de Analysis Services do Azure utilizando o Excel.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b3ab749b4610f57e19ad1af0fc74d41340d7555b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="connect-with-excel"></a>Ligar com o Excel

Assim que tiver criado um servidor e implementar um modelo de tabela, os clientes podem ligar e começar a explorar os dados.


## <a name="connect-in-excel"></a>Ligar no Excel

Ligar a um servidor no Excel é suportada utilizando obter dados no Excel 2016. A ligação utilizando o Assistente de importação de tabelas no Power Pivot não é suportada. 

**Para se ligar no Excel 2016**

1. No Excel 2016, no **dados** do Friso, clique em **obter dados externos** > **de outras origens** > **do Analysis Services** .

2. No Assistente de ligação de dados, no **nome do servidor**, introduza o nome do servidor, incluindo o protocolo e o URI. Por exemplo, asazure://westcentralus.asazure.windows.net/advworks. Em seguida, no **credenciais de início de sessão**, selecione **utilize o seguinte nome de utilizador e palavra-passe**e, em seguida, escreva o nome de utilizador organizacionais, por exemplo nancy@adventureworks.come a palavra-passe.

    > [!IMPORTANT]
    > Se iniciar sessão com um Account Microsoft Live ID, Yahoo, Gmail, etc., ou é necessários para iniciar sessão com a autenticação multifator, deixe o campo de palavra-passe em branco. É-lhe pedida uma palavra-passe depois de clicar em seguinte.

    ![Ligar a partir do início de sessão de Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. No **selecionar base de dados e tabela**, selecione a base de dados e o modelo ou perspetiva e, em seguida, clique em **concluir**.
   
    ![Ligar a partir de modelo selecione do Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Consulte também
[Bibliotecas de cliente](analysis-services-data-providers.md)   
[Gerir o seu servidor](analysis-services-manage.md)     


