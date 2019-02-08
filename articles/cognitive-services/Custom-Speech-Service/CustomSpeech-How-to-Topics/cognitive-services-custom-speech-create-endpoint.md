---
title: Criar um ponto de final de voz personalizada com o serviço de voz personalizada no Azure | Documentos da Microsoft
description: Saiba como criar um ponto de final de voz em texto personalizado com o serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: nitinme
ms.openlocfilehash: efd88a3f222b229fcd10432fc011d5ffa68cd414
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883955"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Criar um ponto final de conversão de voz em texto personalizado

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Depois de criar modelos acústicos personalizados ou modelos de linguagem, pode implementá-las num ponto de final de voz em texto personalizado. 

## <a name="create-an-endpoint"></a>Criar um ponto final
Para criar um novo ponto final personalizado, selecione **implementações** sobre o **voz personalizada** menu na parte superior da página. Esta ação leva-o para o **implementações** página, que contém uma tabela de pontos finais personalizados atuais. Se ainda não criou quaisquer pontos de extremidade, a tabela estiver vazia. A região atual está refletida no título da tabela. 

Para criar uma implementação para um idioma diferente, selecione **localidade de alteração**. Para obter mais informações sobre os idiomas suportados, consulte [localidades com suporte no serviço de voz personalizada](cognitive-services-custom-speech-change-locale.md).

Para criar um novo ponto final, selecione **criar novo**. Na **criar implementação** painel, introduza as informações no **nome** e **Descrição** caixas de implantação personalizada.

Na **subscrição** combinação caixa, selecione a subscrição que pretende utilizar. Se for uma subscrição de S2, pode selecionar unidades de escala e de registo de conteúdo. Para obter mais informações sobre unidades de escala e de registo, consulte [medidores do serviço de voz personalizada e quotas](../cognitive-services-custom-speech-meters.md).

A tabela seguinte mostra como o mapeamento de unidades de escala, para pedidos em simultâneo disponíveis:

| Unidade de escala | Número de pedidos simultâneos |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| D | 5 * n |

Também pode selecionar se o registo de conteúdo é mudado ou desativar. Ou seja, que está selecionando se o tráfego de ponto final é armazenado para uso interno da Microsoft. Se não estiver selecionada, armazenar o tráfego será suprimido. A supressão dos resultados de registo de conteúdo em custos adicionais. Consulte a [página de informações de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) para obter detalhes.

> [!NOTE]
> Registo de conteúdo é chamado de "Sem rastreio" na página de preços.
>


Além disso, a Microsoft fornece uma estimativa dos custos, para que esteja ciente do impacto sobre os custos de unidades de escala e de registo de conteúdo. Esta estimativa é uma estimativa aproximada e pode diferir dos custos reais.

> [!NOTE]
> Estas definições não estão disponíveis com as assinaturas do F0 (escalão gratuito).
>

Na **modelo acústico** , selecione o modelo acústico que pretende, e, no **modelo de idioma** , selecione o modelo de idioma que pretende. As opções para modelos de acústica e idioma incluem sempre os modelos bases do Microsoft. A seleção do modelo de base limita as combinações. Não é possível misturar os modelos bases conversacionais com pesquisa e estabelecer modelos de bases.

![A página Criar implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Certifique-se de que aceite os termos de utilização e obter informações sobre preços, selecionando a caixa de verificação.
>

Depois de selecionar os modelos de acústica e idioma, selecione **criar**. Esta ação devolve-lhe a **implementações** página. A tabela inclui agora uma entrada que corresponde ao seu novo ponto de extremidade. Estado do ponto de extremidade reflete o estado atual enquanto ele está a ser criado. Pode demorar até 30 minutos para instanciar um novo ponto final com os seus modelos personalizados. Quando o estado da implementação estiver *Complete*, o ponto final está pronto para ser utilizado.

![A página implementações](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Quando a implementação estiver pronta, o nome da implementação torna-se uma ligação. Selecionar a ligação apresenta os **informações de implementação** página, que apresenta os URLs do seu ponto final personalizado para utilizar com a um pedido de HTTP ou a Microsoft cognitivos serviços de voz biblioteca de cliente, que utiliza web sockets.

![A página de informações de implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Passos Seguintes

Para mais tutoriais, consulte:
* [Utilizar um ponto de final de voz em texto personalizado](cognitive-services-custom-speech-use-endpoint.md)
* [Criar um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
* [Criar um modelo de linguagem personalizado](cognitive-services-custom-speech-create-language-model.md)
