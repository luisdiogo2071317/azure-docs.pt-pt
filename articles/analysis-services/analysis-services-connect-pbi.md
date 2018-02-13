---
title: "Se ligam aos serviços de análise do Azure com o Power BI | Microsoft Docs"
description: Saiba como ligar a um servidor de Analysis Services do Azure utilizando o Power BI.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/10/2018
ms.author: owend
ms.openlocfilehash: 568c18fb0591284995f5986f98b3762a9d19b056
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="connect-with-power-bi"></a>Estabelecer ligação com o Power BI

Assim que tiver criado um servidor no Azure e implementar um modelo de tabela, os utilizadores na sua organização, estará pronto ligar e começar a explorar os dados. 

> [!TIP]
> Lembre-se de que utiliza a versão mais recente do [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Ligar no Power BI Desktop

1. No ambiente de trabalho do Power BI, clique em **obter dados** > **Azure** > **base de dados do Azure Analysis Services**.

2. No **servidor**, introduza o nome do servidor. Não se esqueça de incluir o URL completo; Por exemplo, asazure://westcentralus.asazure.windows.net/advworks.

3. No **base de dados**, se souber o nome da base de dados em tabela do modelo ou perspetiva que pretende ligar, cole-a aqui. Caso contrário, pode deixar este campo em branco e selecione uma base de dados ou de uma perspetiva mais tarde.

4. Selecione uma opção de ligação e, em seguida, prima **Connect**. 

    Ambos **ligar em direto** e **importação** opções são suportadas. No entanto, é recomendado que utilizar ligações em direto porque o modo de importação tem algumas limitações; mais especificamente, o desempenho do servidor poderão ser afetado durante a importação. Também, se o modelo está a ser atualizado no serviço Power BI, o **permitir o acesso a partir do Power BI** definição aplica-se apenas ao escolher **ligar em direto**.

5. Se lhe for solicitado, introduza as credenciais de início de sessão. 

6. No **navegador**, expanda o servidor, em seguida, selecione o modelo ou perspetiva que pretende ligar a e, em seguida, clique em **Connect**. Clique num modelo ou perspetiva para mostrar todos os objetos para nessa vista.

    O modelo é aberto no Power BI Desktop com um relatório na vista de relatório em branco. A lista de campos apresenta todos os objetos de modelo não oculto. Estado de ligação é apresentado no canto inferior direito.

## <a name="connect-in-power-bi-service"></a>Ligar no Power BI (serviço)

1. Crie um ficheiro do Power BI Desktop que tenha uma ligação em direto para o seu modelo no seu servidor.
2. No [Power BI](https://powerbi.microsoft.com), clique em **obter dados** > **ficheiros**, em seguida, localize e selecione o ficheiro. pbix.



## <a name="see-also"></a>Consulte também
[Ligar ao Azure Analysis Services](analysis-services-connect.md)   
[Bibliotecas de cliente](analysis-services-data-providers.md)

