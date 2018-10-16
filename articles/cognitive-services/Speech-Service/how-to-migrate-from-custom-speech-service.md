---
title: Migrar do serviço de voz personalizada para o serviço de voz
titlesuffix: Azure Cognitive Services
description: Saiba como a migração do serviço de voz personalizada para o serviço de voz.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 10/01/2018
ms.author: panosper
ms.openlocfilehash: db09a85daff553dc911d039d37c826343e93d240
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338527"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Migrar do serviço de voz personalizada para o serviço de voz

Utilize este artigo para migrar as suas aplicações do serviço de voz personalizada para o serviço de voz.

O serviço de voz personalizada agora é parte integrante do serviço de voz. Mude para o serviço de voz para tirar partido dos mais recentes atualizações de qualidade e funcionalidade.
 
## <a name="migration-for-new-customers"></a>Migração para os novos clientes

O modelo de preços é mais simples, mudar para um modelo de preços baseado em hora para o serviço de voz.   

1. Crie um recurso do Azure em cada região em que a sua aplicação está disponível. O nome de recurso do Azure é **voz**. Pode usar um único recurso do Azure para os seguintes serviços na mesma região, em vez de criar recursos separados:

    * Conversão de voz em texto
    * Personalizado de voz em texto
    * Voz
    * Tradução de voz

2. Transfira o [SDK de voz](speech-sdk.md). 

3. Siga os guias de início rápido e amostras de SDK para utilizar as APIs corretas. Se utilizar as APIs REST, terá também de utilizar os pontos de extremidade corretos e as chaves do recurso. 

4. Atualize a aplicação de cliente para utilizar o serviço de voz e APIs. 

> [!NOTE]
> * LUIS – se ativou a voz na compreensão de idiomas (LUIS), um único recurso de LUIS na mesma região irá funcionar para LUIS, bem como todos os serviços de voz. Ver [reconhecer intenções de voz](how-to-recognize-intents-from-speech-csharp.md) documentação.
> * Tradução de texto em texto não é parte integrante do serviço de voz. Ele tem sua própria subscrição de recursos do Azure.
  


## <a name="migration-for-existing-customers"></a>Migração para os clientes existentes

Os clientes existentes são necessários para migrar as suas chaves de recurso existente para o serviço de voz no portal do serviço de voz. Utilize os passos seguintes: 

> [!NOTE] 
> As chaves do recurso só podem ser migradas dentro da mesma região. 

1. Inicie sessão para o [cris.ai](http://www.cris.ai) portal e selecione a subscrição no menu superior direito. 

2. Selecione **migrar a subscrição selecionada**.

3. Introduza a chave de subscrição na caixa de texto e selecione **migrar**.

## <a name="next-steps"></a>Passos Seguintes

* [Experimente gratuitamente o serviço de voz](get-started.md)
* Saiba mais [conversão de voz em texto](./speech-to-text.md) conceitos

## <a name="see-also"></a>Consulte também

* [O que é o serviço de voz](overview.md)
* [Documentação do serviço de voz e o SDK](speech-sdk.md#get-the-sdk)