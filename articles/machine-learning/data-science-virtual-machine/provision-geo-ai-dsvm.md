---
title: Aprovisionar uma máquina de Virtual Artificial Intelligence Georreplicação no Azure - Azure | Microsoft Docs
description: Como aprovisionar uma máquina de Virtual AI Georreplicação no Azure.
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
ms.openlocfilehash: 93dfe6594aeaf45a6905fe8cb55c98dd37cc9599
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408613"
---
# <a name="provision-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Aprovisionar uma máquina de Virtual Artificial Intelligence Georreplicação no Azure 

Georreplicação AI dados ciência de Máquina Virtual (Georreplicação DSVM) é uma extensão do populares [máquina de Virtual de ciência de dados do Azure](http://aka.ms/dsvm) que especial está configurada para combinar a análise de AI e geoespacial. A análise de geoespacial na VM é utiliza a tecnologia de [ArcGIS Pro](https://www.arcgis.com/features/index.html). A VM de ciência de dados permite a rápida formação de machine learning modelos e até mesmo de avançada learning modelos, nos dados que é enriquecidos com informações geográficas. É suportada apenas no Windows DSVM de 2016. 

A Georreplicação-DSVM contém várias ferramentas para AI, incluindo:

- Edições de GPU de arquiteturas populares learning profunda como Microsoft cognitivos Toolkit TensorFlow, Keras, Caffe2, Chainer; 
- ferramentas para adquirir e imagem previamente processo, textual dados, 
- ferramentas de atividades de desenvolvimento, tais como Microsoft R Server Developer Edition, Anaconda Python, blocos de notas do Jupyter para Python e R, IDEs para Python e R, o SQL Server de bases de dados
- Software de hora ambiente de trabalho do ArcGIS Pro do ESRI com interfaces de Python e R pode trabalhar com os dados de geoespacial das suas aplicações de AI. 


## <a name="create-your-geo-ai-data-science-vm"></a>Criar a ciência de dados de AI Georreplicação VM

Eis o procedimento para criar uma instância da VM de ciência de dados de AI Georreplicação: 


1. Navegue para a máquina virtual listagem no [portal do Azure](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
2. Selecione o **criar** na parte inferior para ser levados num assistente.
![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)
3. O assistente utilizado para criar o DSVM Georreplicação requer **entradas** para cada um do **quatro passos** enumerado à direita desta figura. Seguem-se as entradas necessárias para configurar cada um destes passos:



   - **Noções básicas**

      1. **Nome**: nome do servidor de ciência de dados que está a criar.

      2. **Nome de utilizador**: id de início de sessão de conta de administrador.

      3. **Palavra-passe**: palavra-passe da conta de administrador.

      4. **Subscrição**: Se tiver mais do que uma subscrição, selecione aquele no qual a máquina está a ser criado e cobrados.

      5. **Grupo de recursos**: pode criar uma nova ou utilize um **vazio** grupo de recursos do Azure existente na sua subscrição.

      6. **Localização**: selecione o Centro de dados que é mais adequado. Normalmente, é o Centro de dados que contém a maioria dos seus dados ou se mais próxima da sua localização física para acesso de rede mais rápido. Se precisar de fazer profunda aprendizagem GPU, tem de escolher uma das localizações no Azure com as instâncias de série de NC GPU VM. Atualmente as localizações que tenham GPU VMs são: **EUA leste, Norte Central dos EUA, Sul Central dos EUA, EUA oeste, 2, Europa do Norte, Europa Ocidental**. Para obter a lista mais recente, verifique o [produtos do Azure por região página](https://azure.microsoft.com/regions/services/) e procure **NC série** em **computação**. 


   - **Definições**: selecione um tamanho da máquina virtual de série de NC GPU se planear executar learning profunda em GPU no seu DSVM Georreplicação. Caso contrário, pode escolher um da CPU com base instância.  Crie uma conta de armazenamento para a VM. 
   
   - **Resumo**: Certifique-se de que todas as informações que introduziu estão corretas.

   - **Comprar**: clique em **comprar** para iniciar o aprovisionamento. É fornecida uma ligação para os termos do serviço. A VM não tem quaisquer taxas adicionais para além de computação para o tamanho de servidor que selecionou no **tamanho** passo. 

>[!NOTE]
> O aprovisionamento deve demorar cerca de 20-30 minutos. O estado de aprovisionamento da é apresentado no portal do Azure.


## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Como aceder à máquina de Virtual ciência de dados de Georreplicação AI

Assim que a VM é criada, está pronto para começar a utilizar as ferramentas que estão instaladas e pré-configurados no mesmo. Existem mosaicos de menu de início e de ambiente de trabalho ícones de muitas das ferramentas. Poderá utilizar o ambiente de trabalho remoto para a mesma utilizando as credenciais de conta de administrador que configurou no precedente **Noções básicas** secção. 


## <a name="using-arcgis-pro-installed-in-the-vm"></a>Utilizar o ArcGIS Pro instalado na VM

A Georreplicação-DSVM já tem o ambiente de trabalho de ArcGIS Pro previamente instalado e o ambiente de pré-configurado para trabalhar com as ferramentas de DSVM. Quando iniciar ArcGIS-pede-lhe um início de sessão à sua conta ArcGIS. Se já tiver uma conta de ArcGIS e número de licenças de software, pode utilizar as suas credenciais existentes.  

![Arc-GIS-Logon](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Caso contrário, pode inscrever-se para a nova conta ArcGIS e a licença ou obter um [avaliação gratuita](https://www.arcgis.com/features/free-trial.html). 

![ArcGIS-Free-Trial](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Depois da inscrição para uma um paga ou uma conta gratuita ArcGIS, podem autorizar ArcGIS Pro para a sua conta ao seguir as instruções no [como começar a documentação de profissionais ArcGIS](http://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf). 

Depois de iniciar sessão com a sua conta ArcGIS ambiente de trabalho ArcGIS Pro, está pronto para começar a utilizar as ferramentas de ciência de dados que estão instaladas e configuradas na VM para a sua análise Geoespacial e projetos de aprendizagem.

## <a name="next-steps"></a>Passos Seguintes

Começar a utilizar a VM de ciência de dados de AI Georreplicação com orientação dos seguintes tópicos:

* [Utilize a ciência de dados de AI Georreplicação VM](use-geo-ai-dsvm.md)