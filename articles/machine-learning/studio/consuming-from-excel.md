---
title: Consumir o serviço web no Excel - Azure Machine Learning Studio | Documentos da Microsoft
description: O Azure Machine Learning Studio facilita chamar serviços web diretamente a partir do Excel sem a necessidade de escrever qualquer código.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/1/2018
ms.openlocfilehash: 1425cb0f0b327e0f39733b99a4c10c03a0ec3089
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53092354"
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Consumir um serviço Web do Azure Machine Learning a partir do Excel
 O Azure Machine Learning Studio facilita chamar serviços web diretamente a partir do Excel sem a necessidade de escrever qualquer código.

Se estiver a utilizar o Excel 2013 (ou posterior) ou o Excel Online, recomendamos que utilize o Excel [suplemento do Excel](excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Passos
Publica um serviço web. [Esta página](walkthrough-5-publish-web-service.md) explica como fazer isso. Atualmente, a funcionalidade de livro do Excel só é suportada para os serviços de solicitação/resposta com uma única saída (ou seja, uma etiqueta classificação única). 

Depois de ter um serviço da web, clique nas **serviços da WEB** secção à esquerda do studio e, em seguida, selecione o serviço web para consumir a partir do Excel.

**Serviço Web clássico**

1. Sobre o **DASHBOARD** separador para o serviço web é uma linha para o **SOLICITAÇÃO/resposta** serviço. Se este serviço teve uma única saída, deverá ver o **transferir o livro do Excel** ligação nessa linha.
   
    ![][1]
2. Clique em **transferir o livro do Excel**.

**Novo serviço Web**

1. No portal do serviço Web do Azure Machine Learning, selecione **Consume**.
2. Na página Consume, no **opções de consumo do serviço Web** secção, clique no ícone do Excel.

**Utilizar o livro**

1. Abra o livro.
2. É apresentado um aviso de segurança; Clique nas **Enable Editing** botão.
   
    ![][2]
3. É apresentado um aviso de segurança. Clique nas **ativar conteúdo** botão para executar macros na folha de cálculo.
   
    ![][3]
4. Assim que as macros estejam habilitadas, é gerada uma tabela. Colunas azul são necessários como entrada para o serviço web RRS, ou **parâmetros**. Tenha em atenção a saída do serviço RRS, **valores previstos** em verde. Quando todas as colunas para uma determinada linha estiverem preenchidas, a pasta de trabalho automaticamente chama a API de classificação e exibe os resultados com a pontuação.
   
    ![][4]
5. Para mais de uma linha de pontuação, a segunda linha com dados e os valores previstos de preenchimento são produzidos. Pode até mesmo colar várias linhas ao mesmo tempo.

Pode utilizar qualquer uma das funcionalidades do Excel (gráficos, mapa de energia, formatação condicional, etc.) com os valores previstos para o ajudar a visualizar os dados.    

## <a name="sharing-your-workbook"></a>O livro de partilha
As macros trabalhar, a chave de API deve ser parte da folha de cálculo. Isso significa que deve compartilhá-la apenas com entidades/indivíduos que confia.

## <a name="automatic-updates"></a>Atualizações automáticas
É feita uma chamada RRS nessas duas situações:

1. Na primeira vez numa linha tem conteúdo em todos os seus **parâmetros**
2. Sempre que qualquer um da **parâmetros** alterações numa linha que tinha todos seus **parâmetros** introduzido.

[1]: ./media/consuming-from-excel/excellink.png
[2]: ./media/consuming-from-excel/enableeditting.png
[3]: ./media/consuming-from-excel/enablecontent.png
[4]: ./media/consuming-from-excel/sampletable.png
