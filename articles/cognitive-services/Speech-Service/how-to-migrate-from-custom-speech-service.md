---
title: Migrar do serviço de voz personalizada para o serviço de voz
titlesuffix: Azure Cognitive Services
description: Saiba como migrar do serviço de voz personalizada para o serviço de voz.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: 7cb8c992b4c131b7f28eca6c2f35ee9facdf8d4e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416108"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrar do serviço de voz personalizada para o serviço de voz

Utilize este artigo para migrar seus aplicativos de serviço de voz personalizada para o serviço de voz.

O serviço de voz personalizada agora é parte integrante do serviço de voz. Mude para o serviço de voz para tirar partido dos mais recentes atualizações de qualidade e funcionalidade.
 
## <a name="migration-for-new-customers"></a>Migração para os novos clientes

O modelo de preços é mais simples, com um modelo de preços baseado em hora para o serviço de voz.   

1. Crie um recurso do Azure em cada região em que a sua aplicação está disponível. O nome de recurso do Azure é **voz**. Pode usar um único recurso do Azure para os seguintes serviços na mesma região, em vez de criar recursos separados:

    * Conversão de voz em texto
    * Personalizado de voz em texto
    * Conversão de texto em voz
    * Tradução de voz

2. Transfira o [SDK de voz](speech-sdk.md). 

3. Siga os guias de início rápido e amostras de SDK para utilizar as APIs corretas. Se utilizar as APIs REST, terá também de utilizar os pontos de extremidade corretos e as chaves do recurso. 

4. Atualize a aplicação de cliente para utilizar o serviço de voz e APIs. 

> [!NOTE]
> * Se ativou a voz na compreensão de idiomas (LUIS), um único recurso de LUIS na mesma região irá funcionar para LUIS, bem como todos os serviços de voz. Para obter mais informações, consulte [reconhecer intenções de voz](how-to-recognize-intents-from-speech-csharp.md).
> * Tradução de texto em texto não é parte integrante do serviço de voz. Esta funcionalidade requer a sua própria subscrição de recursos do Azure.
  


## <a name="migration-for-existing-customers"></a>Migração para os clientes existentes

Migre as chaves de recurso existente para o serviço de voz no portal do serviço de voz. Utilize os passos seguintes: 

> [!NOTE] 
> As chaves do recurso só podem ser migradas dentro da mesma região. 

1. Inicie sessão para o [cris.ai](http://www.cris.ai) portal e selecione a subscrição no menu superior direito. 

2. Selecione **migrar a subscrição selecionada**.

3. Introduza a chave de subscrição na caixa de texto e selecione **migrar**.

## <a name="next-steps"></a>Passos Seguintes

* [Experimente gratuitamente o serviço de voz](get-started.md).
* Saiba mais [conversão de voz em texto](./speech-to-text.md) conceitos.

## <a name="see-also"></a>Consulte também

* [O que é o serviço de voz](overview.md)
* [Documentação do serviço de voz e o SDK](speech-sdk.md#get-the-sdk)