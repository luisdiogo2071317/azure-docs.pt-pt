---
title: Experimente o serviço de Voz gratuitamente
description: Descubra como pode experimentar o serviço de voz, sem qualquer custo.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: a9972d19d33c69ea2eb1b0e00512f37a315b92d8
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285021"
---
# <a name="try-the-speech-service-for-free"></a>Experimente o serviço de Voz gratuitamente

Começar a utilizar com o serviço de voz é fácil e económica. Uma avaliação gratuita de 30 dias permite-lhe detetar o que o serviço pode fazer e decidir se é adequada para as necessidades da sua aplicação.

Se precisar de mais tempo, inscreva-se para uma conta do Microsoft Azure — ele vem com 200 $ em crédito de serviço que pode aplicar no sentido de uma subscrição do serviço de voz paga durante 30 dias.

Por fim, o serviço de voz oferece um escalão gratuito de baixo volume adequado para o desenvolvimento de aplicativos. Pode manter esta subscrição gratuita mesmo depois de expira o seu crédito de serviço.

## <a name="free-trial"></a>Avaliação gratuita

A avaliação gratuita de 30 dias dá-lhe acesso a Standard S0 escalão de preço por um período limitado. Para se inscrever numa avaliação gratuita de 30 dias, siga estes passos.

1. Vá para o [experimentar os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página.

1. Mude para o separador de voz e clique nas **chave de API obter** junto a "Serviços de voz".

   ![Separador de serviços de voz](media/index/try-speech-api-free-trial1.png)<br>
   ![Chave de API](media/index/try-speech-api-free-trial2.png)

3. Aceitar os termos e selecione sua localidade no menu pendente.

   ![Aceitar termos](media/index/try-speech-api-free-trial3.png)

4. Inicie sessão com a sua conta Microsoft, Facebook, LinkedIn ou do GitHub. Em alternativa, pode inscrever-se numa conta gratuita do Microsoft:

    * Vá para o [portal de contas Microsoft](https://account.microsoft.com/account).
    * Clique em **iniciar sessão com a Microsoft**.

    ![Iniciar sessão](media/index/try-speech-api-free-trial4.png)

    * Quando lhe for pedido para iniciar sessão, clique em "Criar um".

    ![Criar conta nova](media/index/try-speech-api-free-trial5.png)

    * Nas etapas a seguir, introduza o seu endereço de e-mail ou número de telefone, atribua uma palavra-passe e siga as instruções para verificar a sua nova conta Microsoft.

Depois de iniciar sessão, começa a sua avaliação gratuita. A página Web apresentada lista todos os serviços cognitivos para os quais tem atualmente subscrições de avaliação. Duas chaves de subscrição são apresentadas ao lado de "Serviços de voz". Pode usar qualquer um dos chave em seus aplicativos.

> [!NOTE]
> Todas as subscrições de avaliação gratuitas são na região E.U.A. oeste. Certifique-se de que utilizar o ponto final correspondente a sua região, quando são efetuados pedidos.

## <a name="new-azure-account"></a>Nova conta do Azure

Novas contas do Azure recebem um crédito de serviço de US $200 que tem uma duração de 30 dias. Este crédito pode ser utilizado para ainda mais a explorar o serviço de voz ou começar o desenvolvimento de aplicativos.

Para se inscrever para uma nova conta do Azure, siga estes passos.

1. Vá para o [do Azure, inscreva-se página](https://azure.microsoft.com/free/ai/). 

1. Clique em **comece gratuitamente**.

    ![Iniciar gratuitamente](media/index/try-speech-api-new-azure1.png)

3. Inicie sessão com a sua conta Microsoft. Se não tiver um:

    * Vá para o [portal de contas Microsoft](https://account.microsoft.com/account).
    * Clique em **iniciar sessão com a Microsoft**.
    * Quando lhe for pedido para iniciar sessão, clique em "Criar um".
    * Nas etapas a seguir, introduza o seu endereço de e-mail ou número de telefone, atribua uma palavra-passe e siga as instruções para verificar a sua nova conta Microsoft.

1. Introduza o resto as informações pedidas para se inscrever para uma conta. Especifique o seu país e o seu nome e fornecer um endereço de e-mail e número de telefone.

    ![Introduza as informações](media/index/try-speech-api-new-azure2.png)

    Verificar a sua identidade por telefone e ao fornecer um número de cartão de crédito, em seguida, aceite o contrato de utilizador do Azure. (Seu cartão de crédito não será cobrado.)

    ![Aceitar o contrato](media/index/try-speech-api-new-azure3.png)

É criada a sua conta gratuita do Azure. Siga os passos na secção seguinte para iniciar uma subscrição para o serviço de voz.

## <a name="create-a-speech-resource-in-azure"></a>Criar um recurso de fala no Azure

Para adicionar um recurso de serviço de voz à sua conta do Azure, siga estes passos.

1. Inicie sessão para o [portal do Azure](https://ms.portal.azure.com/) com a sua conta Microsoft.

1. Clique em **criar um recurso** (o verde **+** ícone) na parte superior esquerda do portal.

    ![Criar recurso](media/index/try-speech-api-create-speech1.png)

1. Na nova janela, procure voz.

    ![Clique em voz](media/index/try-speech-api-create-speech2.png)

1. Nos resultados da pesquisa, clique em "Fala (pré-visualização)."

1. Clique nas **criar** na parte inferior do painel de serviço de voz.

    ![Clique em criar](media/index/try-speech-api-create-speech3.png)

1. No painel criar, introduza:

    * Um nome para o novo recurso. O nome auxilia a distinguir entre várias subscrições para o mesmo serviço.
    * Escolha a subscrição do Azure que o novo recurso está associado para determinar como são cobradas as taxas.
    * Selecione a região onde o recurso será utilizado. Atualmente, o serviço de voz está disponível nas regiões E.U.A. oeste, Europa do Norte e Ásia Oriental.
    * Escolha o escalão de preço, qualquer um dos F0 (limitado a subscrição gratuita) ou S0 (subscrição standard). Clique em **vista de detalhes de preço completos** para obter informações completas sobre preços e a utilização de quotas para cada camada.
    * Criar um novo grupo de recursos para esta subscrição de voz ou atribuí-lo a um existente. Os grupos de recursos ajudam a que manter suas várias subscrições do Azure, organizadas.
    * Para obter acesso conveniente a sua subscrição no futuro, marcar a **afixar ao dashboard** caixa de verificação.
    * Clique em **criar.**

    ![Clique em criar no painel](media/index/try-speech-api-create-speech4.png)

    Pode demorar alguns momentos para criar e implementar o novo recurso de voz. É apresentado o painel de início rápido com informações sobre o seu novo recurso.

    ![Painel de início rápido](media/index/try-speech-api-create-speech5.png)

1. Clique nas **chaves** ligação no passo 1, no painel de início rápido para exibir as chaves de subscrição. Cada subscrição tem duas chaves; Pode usar em seu aplicativo. Clique no botão junto a cada chave para copiá-lo para a área de transferência para colar no seu código.

> [!NOTE]
> Pode criar qualquer número de subscrições de escalão standard numa ou em várias regiões. No entanto, pode criar apenas uma subscrição do escalão gratuito.

## <a name="next-steps"></a>Passos Seguintes

Baixe um SDK e um código de exemplo para experimentar o serviço de voz.

> [!div class="nextstepaction"]
> [SDKs de voz](speech-sdk.md)

> [!div class="nextstepaction"]
> [Código de exemplo](samples.md)
