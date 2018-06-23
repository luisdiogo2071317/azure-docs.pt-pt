---
title: Criar um ponto final de reconhecimento de voz personalizadas com o serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como criar um ponto final de reconhecimento de voz para texto personalizado com o serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: 99bc275db1f0c1b45b3db440d2e03d0db9ab5cf6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351632"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Criar um ponto final de reconhecimento de voz para texto personalizado
Depois de criar modelos acústica personalizados ou modelos de idioma, pode implementá-las num ponto final reconhecimento de voz para texto personalizado. 

## <a name="create-an-endpoint"></a>Criar um ponto final
Para criar um novo ponto de final personalizado, selecione **implementações** no **reconhecimento de voz personalizadas** menu na parte superior da página. Esta ação demora ao **implementações** página, que contém uma tabela de pontos finais personalizados atuais. Se ainda não tiver criado quaisquer pontos finais, a tabela estiver vazia. A região atual é refletida no título de tabela. 

Para criar uma implementação de um idioma diferente, selecione **alteração região**. Para obter mais informações sobre os idiomas suportados, consulte [suportado regiões no serviço de reconhecimento de voz personalizadas](cognitive-services-custom-speech-change-locale.md).

Para criar um novo ponto final, selecione **criar novo**. No **criar implementação** painel, introduza informações no **nome** e **Descrição** caixas da sua implementação personalizada.

No **subscrição** combinação caixa, selecione a subscrição que pretende utilizar. Se se tratar de uma subscrição de S2, pode selecionar unidades de escala e o registo de conteúdo. Para obter mais informações sobre unidades de escala e o registo, consulte [medidores do serviço de reconhecimento de voz personalizadas e quotas](../cognitive-services-custom-speech-meters.md).

A tabela seguinte mostra como unidades de escala mapeiam pedidos simultâneos disponíveis:

| Unidade de escala | Número de pedidos simultâneos |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| N | 5 * n |

Também pode selecionar se o registo de conteúdo é mudado ou desativar. Ou seja, está a selecionar se o tráfego de ponto final está armazenado para utilização interna da Microsoft. Se não estiver selecionada, armazenar o tráfego irão ser suprimido. A supressão dos resultados de registo de conteúdo em custos adicionais. Consulte o [página de informações de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) para obter mais detalhes.

> [!NOTE]
> Registo de conteúdo é chamado "Não Trace" na página de preços.
>


Além disso, a Microsoft disponibiliza uma estimativa aproximada dos custos de modo a que tem conhecimento do impacto nos custos de unidades de escala e o registo de conteúdo. Esta estimativa é uma estimativa aproximada e poderá ser diferente dos custos reais.

> [!NOTE]
> Estas definições não estão disponíveis com as subscrições do F0 (escalão gratuito).
>

No **modelo acústica** lista, selecione o modelo acústica que pretende, e, no **idioma modelo** lista, selecione o modelo de idioma que pretende. As opções para modelos de idioma e acústica incluem sempre os modelos base do Microsoft. A seleção do modelo de base limita as combinações. Não é possível misturar conversational modelos base com a pesquisa e modelos de base da sua organização ditarem.

![A página de criar a implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Lembre-se de que aceite os termos de utilização e obter informações sobre preços, selecionando a caixa de verificação.
>

Depois de selecionar os modelos acústica e de idioma, selecione **criar**. Esta ação devolve ao **implementações** página. A tabela inclui agora uma entrada que corresponde ao seu novo ponto final. Estado do ponto final reflete o estado atual enquanto está a ser criada. Pode demorar até 30 minutos para instanciar um novo ponto final com os seus modelos personalizados. Quando o estado da implementação é *concluída*, o ponto final está pronto a utilizar.

![A página de implementações](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Quando a implementação estiver pronta, o nome da implementação torna-se uma ligação. Ao selecionar a hiperligação apresenta o **informações de implementação** página, que apresenta os URLs do seu ponto final personalizado para utilizar com o pedido de HTTP ou o Microsoft cognitivos serviços voz biblioteca de clientes, que utiliza os sockets web.

![A página de informações de implementação](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Passos Seguintes

Para tutoriais mais, consulte:
* [Utilizar um ponto final de reconhecimento de voz para texto personalizado](cognitive-services-custom-speech-use-endpoint.md)
* [Criar um modelo personalizado acústica](cognitive-services-custom-speech-create-acoustic-model.md)
* [Criar um modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md)
