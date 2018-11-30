---
title: Resolver problemas de reparametrização um serviço web clássico de Machine Learning Studio - Azure | Documentos da Microsoft
description: Identificar e corrigir encontrou de problemas comuns quando são reparametrização do modelo para um web service do Azure Machine Learning Studio.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.openlocfilehash: 1105b81d0f8ba80bd76bcdf140fe79b9e8a7102d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307207"
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-studio-classic-web-service"></a>Resolução de problemas relativos a novas preparações de um serviço web clássico do Azure Machine Learning Studio
## <a name="retraining-overview"></a>Descrição geral de reparametrização
Ao implementar uma experimentação preditiva como um serviço web de pontuação é um modelo estático. À medida que novos dados torna-se disponível, ou quando o consumidor da API tem seus próprios dados, tem de reestruturar o modelo. 

Para obter instruções completas do processo reparametrização de um serviço web clássico, veja [Retrain Machine Learning modelos programaticamente](retrain-models-programmatically.md).

## <a name="retraining-process"></a>Reparametrização do processo
Quando precisar de voltar a preparar o serviço web, tem de adicionar algumas partes adicionais:

* Um serviço web implementado a partir da experimentação de preparação. A experimentação tem de ter uma **saída de serviço Web** módulo anexado à saída do **Train Model** módulo.  
  
    ![Anexe a saída de serviço da web para o modelo de formação.][image1]
* Um novo ponto de final adicionado ao seu serviço web de pontuação.  Pode adicionar o ponto de extremidade por meio de programação com o código de exemplo referenciado nos modelos do Machine Learning voltar a preparar programaticamente tópico ou através do portal do Azure Machine Learning Web Services.

Em seguida, pode utilizar o exemplo C# código de página de ajuda de API do serviço de Web de treinamento para voltar a preparar modelo. Depois de ter avaliado os resultados e estiver satisfeito com os mesmos, atualize o modelo treinado classificação web service usando o novo ponto final que adicionou.

Com todas as peças no lugar, os principais passos que tem de efetuar para voltar a preparar o modelo são os seguintes:

1. Chamar o Web Service de treinamento: A chamada é para o serviço BES (Batch Execution), não a resposta RRS (Request Service). Pode utilizar o exemplo C# código na página de ajuda da API para efetuar a chamada. 
2. Localizar os valores para o *BaseLocation*, *RelativeLocation*, e *SasBlobToken*: estes valores são devolvidos na saída da sua chamada para o serviço da Web de treinamento. 
   ![a mostrar a saída do exemplo reparametrização e os valores de BaseLocation RelativeLocation e SasBlobToken.][image6]
3. Atualizar o ponto de final de adicionado do serviço web classificação com o novo modelo treinado: usando o código de exemplo fornecido nos modelos do Machine Learning voltar a preparar programaticamente, atualize o novo ponto final adicionado para o modelo de classificação com o modelo treinado recentemente a partir do Serviço da Web de treinamento.

## <a name="common-obstacles"></a>Obstáculos comuns
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Verifique se tem o URL correto do PATCH
O URL de aplicar o PATCH de mensagens em fila que está a utilizar tem de ser um associados com o novo ponto de extremidade classificação adicionado para o serviço web de pontuação. Existem várias formas de obter o URL de PATCHES:

**Opção 1: programaticamente**

Para obter o URL correto de PATCHES:

1. Executar o [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) código de exemplo.
2. A partir da saída de AddEndpoint, localize a *HelpLocation* valor e copie o URL.
   
   ![HelpLocation na saída do exemplo addEndpoint.][image2]
3. Cole o URL num browser para navegar para uma página que fornece ligações de ajuda para o serviço web.
4. Clique nas **recursos de atualização** link para abrir a página de ajuda do patch.

**Opção 2: Utilizar o portal de serviços da Web do Azure Machine Learning**

1. Inicie sessão para o [serviços da Web do Azure Machine Learning](https://services.azureml.net/) portal.
2. Clique em **serviços Web** ou **serviços Web clássicos** na parte superior.
4. Clique em serviço web de pontuação está a trabalhar com (se não modificar o nome predefinido do serviço web, ele termina em "[Scoring Exp.]").
5. Clique em **+ novo**.
6. Depois do ponto final é adicionado, clique no nome do ponto final.
7. Sob o **Patch** URL, clique em **API ajuda** para abrir a página de ajuda de aplicação de patches.

> [!NOTE]
> Se tiver adicionado o ponto final para o serviço da Web de treinamento em vez do serviço Web preditivo, receberá o erro seguinte ao clicar o **recursos de atualização** ligação: "Pedimos, mas esta funcionalidade não é suportado ou disponíveis no Neste contexto. Este serviço Web tem não existem recursos atualizáveis. Podemos desculpa pelo inconveniente e estão a trabalhar na melhoria este fluxo de trabalho."
> 
> 

A página de ajuda de PATCH contém o URL de aplicar o PATCH de mensagens em fila tem de utilizar e fornece o código de exemplo, que pode usar para chamá-lo.

![URL de patch.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Verifique que está a atualizar o ponto de final de classificação correto
* Não corrigir o serviço da web de treinamento: A operação de correção deve ser executada no serviço web de pontuação.
* Não aplicar o patch o ponto final predefinido do serviço web: A operação de correção deve ser executada em nova classificação web ponto final do serviço que adicionou.

Pode verificar o serviço web, o ponto final está no, visite o portal de serviços da Web. 

> [!NOTE]
> Certifique-se de que está a adicionar o ponto de extremidade ao serviço Web preditivo, não o serviço da Web de treinamento. Se tiver implementado um treinamento e um serviço Web preditivo corretamente, deverá ver dois serviços da web separado listados. O serviço Web preditivo deve terminar com "[exp preditiva.]".
> 
> 

1. Inicie sessão para o [serviços da Web do Azure Machine Learning](https://services.azureml.net/) portal.
2. Clique em **serviços Web** ou **serviços Web clássicos**.
3. Selecione o seu serviço Web preditivo.
4. Certifique-se de que o seu novo ponto final foi adicionado ao serviço web.

### <a name="check-that-your-workspace-is-in-the-same-region-as-the-web-service"></a>Verifique se a sua área de trabalho está na mesma região que o serviço web
1. Inicie sessão no [do Machine Learning Studio](https://studio.azureml.net/).
2. Na parte superior, clique na lista pendente as áreas de trabalho.

   ![Região de aprendizagem da máquina da interface do Usuário.][image4]

3. Certifique-se a região na sua área de trabalho.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/check-workspace-region.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
