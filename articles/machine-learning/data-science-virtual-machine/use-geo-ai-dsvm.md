---
title: Utilizar a Georreplicação Artificial Intelligence dados ciência máquina - Azure | Microsoft Docs
description: Como utilizar uma máquina de Virtual AI Georreplicação no Azure.
keywords: ligação avançada learning AI, ferramentas de ciência de dados, a máquina de virtual de ciência de dados, a análise Geoespacial
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: gokuma
ms.openlocfilehash: f346b086a0269f247d64edf9346b01849ba3d0ee
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408042"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Utilizar a máquina de Virtual ciência de dados do Georreplicação Artificial Intelligence

Utilize a VM de ciência de dados de AI Georreplicação para obter dados para análise, execute dados wrangling e criar modelos para aplicações de AI que consumam informações geoespacial. Depois de ter aprovisionado a VM de ciência de dados de AI Georreplicação e com sessão iniciada no ArcGIS Pro com a sua conta ArcGIS, pode começar a interagir com o ambiente de trabalho ArcGIS e ArcGis online. Também pode aceder ArcGIS de interfaces de Python e uma ponte de idioma de R pré-configurados na VM de ciência de dados de Georreplicação. Para criar aplicações de AI avançadas, combiná-la com a máquina de aprendizagem e a profundidade de aprendizagem estruturas e outro software de ciência de dados disponível na VM de ciência de dados.  


## <a name="configuration-details"></a>Detalhes de configuração

Biblioteca Python, [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), que é utilizado para a interface com ArcGIS está instalado no ambiente de conda raiz global da VM de ciência de dados que se encontra em ```c:\anaconda```. 

- Se estiver a executar o Python numa linha de comandos, execute ```activate``` ativar para o ambiente de Python conda raiz. 
- Se estiver a utilizar um bloco de notas do IDE ou Jupyter, pode selecionar o ambiente ou kernel para garantir que estão no ambiente conda correto. 

A bridge de R ao ArcGIS é instalado como uma biblioteca de R com o nome [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) no R Microsoft principal instância autónoma do servidor, localizada em ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio e Jupyter já são previamente configurada para utilizar este ambiente de R e terão acesso ao ```arcgisbinding``` biblioteca R. 


## <a name="geo-ai-data-science-vm-samples"></a>Exemplos de VM de ciência de dados de AI Georreplicação

Para além de ML e avançada learning com base em framework amostras da VM de ciência de dados base, um conjunto de exemplos geoespacial também é fornecido como parte da VM de ciência de dados de AI Georreplicação. Estes exemplos podem ajudar a iniciar o desenvolvimento de aplicações de AI utilizando Geoespacial dados e o software de ArcGIS. 


1. [Obter indicado Geoespacial Analytics com o Python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): um exemplo de lançamento que mostra como trabalhar com dados Geoespacial através da interface do Python para ArcGIS fornecida pelo [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) biblioteca. Também mostra como pode combinar tradicional de machine learning com dados geoespacial e visualizar os resultados num mapa no ArcGIS. 

2. [Obter indicado Geoespacial Analytics com R](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): um exemplo de lançamento que mostra como trabalhar com dados Geoespacial através da interface de R para ArcGIS fornecida pelo [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) biblioteca. 

3. [Telefone de nível de pixel utilizar a classificação](https://github.com/Azure/pixel_level_land_classification): um tutorial que ilustra como criar um modelo de rede neuronal profundo que aceita uma imagem com vista aérea como entrada e devolve uma etiqueta de telefone-abrange. Exemplos de etiquetas de telefone-abrange são "forested" ou "nível máximo." O modelo devolve essa uma etiqueta para cada pixel na imagem. O modelo é criado através open source da Microsoft [cognitivos Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) learning profunda framework. O exemplo mostra também como aumentar horizontalmente a formação em [do Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) e consumir serão as predições modelo no software ArcGIS Pro. 


## <a name="next-steps"></a>Passos Seguintes

Estão disponíveis exemplos adicionais que utilizam a VM de ciência de dados:

* [Amostras](dsvm-samples-and-walkthroughs.md)

