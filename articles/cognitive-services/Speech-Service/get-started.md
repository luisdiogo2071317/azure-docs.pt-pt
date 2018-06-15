---
title: Experimente gratuitamente o serviço de reconhecimento de voz | Microsoft Docs
description: Detete a forma como pode experimentar o serviço de reconhecimento de voz sem qualquer custo.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 3feef04694336d9173b419285a96fcaef543e18f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356240"
---
# <a name="try-the-speech-service-for-free"></a>Experimente gratuitamente o serviço de reconhecimento de voz

Introdução com o serviço de reconhecimento de voz é fácil e económica. Uma avaliação gratuita de 30 dias permite-lhe detetar que o serviço pode fazer e decidir se é adequada para as necessidades da sua aplicação.

Se precisar de mais tempo, inscreva-se uma conta do Microsoft Azure — vem com $200 em crédito de serviço que pode aplicar para uma subscrição paga do serviço de reconhecimento de voz para 30 dias.

Por fim, o serviço de reconhecimento de voz oferece um escalão gratuito, volume de baixa adequado para desenvolver aplicações. Pode manter esta subscrição gratuita, mesmo depois do crédito de serviço expira.

## <a name="free-trial"></a>Avaliação gratuita

A versão de avaliação gratuita de 30 dias dá-lhe acesso para o escalão de preço para um período de tempo limitado de padrão de S0. Para se inscrever numa avaliação gratuita de 30 dias, siga estes passos.

1. Vá para o [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página.

1. Mude para o separador de reconhecimento de voz e clique em de **chave de API obter** junto a "Serviços de reconhecimento de voz".

   ![Separador de serviços de reconhecimento de voz](media/index/try-speech-api-free-trial1.png)<br>
   ![Chave de API](media/index/try-speech-api-free-trial2.png)

3. Aceitar os termos e selecione a região no menu pendente.

   ![Concorda com os termos de licenciamento](media/index/try-speech-api-free-trial3.png)

4. Inicie sessão com a sua conta Microsoft, Facebook, LinkedIn ou GitHub. Ou pode inscrever-se numa conta gratuita da Microsoft:

    * Vá para o [portal de contas Microsoft](https://account.microsoft.com/account).
    * Clique em **iniciar sessão com a Microsoft**.

    ![Iniciar sessão](media/index/try-speech-api-free-trial4.png)

    * Quando lhe for pedido para iniciar sessão, clique em "Criar um".

    ![Criar conta nova](media/index/try-speech-api-free-trial5.png)

    * Nos passos que siga, introduza o seu endereço de correio eletrónico ou número de telefone, atribua uma palavra-passe e siga as instruções para verificar a sua nova conta Microsoft.

Depois de iniciar sessão, começa a sua avaliação gratuita. A página Web apresentada lista todos os serviços cognitivos para o qual tem atualmente subscrições de avaliação. Duas chaves de subscrição são apresentadas ao lado de "Serviços de reconhecimento de voz." Pode utilizar qualquer uma das chaves nas suas aplicações.

> [!NOTE]
> Todas as subscrições de avaliação gratuitas estão disponíveis na região EUA oeste. Não se esqueça de utilizar o ponto final correspondente a sua região quando são efetuados pedidos.

## <a name="new-azure-account"></a>Nova conta do Azure

Novas contas do Azure, receber um crédito de serviço de $200 dura 30 dias. Este crédito pode ser utilizado para obter mais explore o serviço de reconhecimento de voz ou iniciar a programação de aplicações.

Para inscrever uma nova conta do Azure, siga estes passos.

1. Vá para o [Azure inscrever-se a página](https://azure.microsoft.com/free/ai/). 

1. Clique em **iniciar livre**.

    ![Iniciar gratuitamente](media/index/try-speech-api-new-azure1.png)

3. Inicie sessão com a sua conta Microsoft. Se não tiver uma:

    * Vá para o [portal de contas Microsoft](https://account.microsoft.com/account).
    * Clique em **iniciar sessão com a Microsoft**.
    * Quando lhe for pedido para iniciar sessão, clique em "Criar um".
    * Nos passos que siga, introduza o seu endereço de correio eletrónico ou número de telefone, atribua uma palavra-passe e siga as instruções para verificar a sua nova conta Microsoft.

1. Introduza o resto das informações do pedido para se inscrever para uma conta. Especifique o seu país e o nome e indique um endereço de correio eletrónico e o número de telefone.

    ![Introduza informações](media/index/try-speech-api-new-azure2.png)

    Verificar a sua identidade, por telefone e fornecendo um número de cartão de crédito, em seguida, aceite o contrato de utilizador do Azure. (Seu cartão de crédito será não ser-lhe faturado.)

    ![Aceitar contrato](media/index/try-speech-api-new-azure3.png)

É criada a sua conta gratuita do Azure. Siga os passos na secção seguinte para iniciar uma subscrição do serviço de reconhecimento de voz.

## <a name="create-a-speech-resource-in-azure"></a>Crie um recurso de reconhecimento de voz no Azure

Para adicionar um recurso de serviço de reconhecimento de voz a sua conta do Azure, siga estes passos.

1. Inicie sessão no [portal do Azure](https://ms.portal.azure.com/) utilizando a sua conta Microsoft.

1. Clique em **crie um recurso** (a verde **+** ícone) na parte superior esquerda do portal.

    ![Criar recurso](media/index/try-speech-api-create-speech1.png)

1. Na nova janela, procure o reconhecimento de voz.

    ![Clique em reconhecimento de voz](media/index/try-speech-api-create-speech2.png)

1. Nos resultados da pesquisa, clique em "Reconhecimento de voz (pré-visualização)."

1. Clique em de **criar** na parte inferior do painel do serviço de reconhecimento de voz.

    ![Clique em criar](media/index/try-speech-api-create-speech3.png)

1. No painel de criação, introduza:

    * Um nome para o novo recurso. O nome ajuda-o a distinguir entre várias subscrições para o mesmo serviço.
    * Selecione a subscrição do Azure que o novo recurso se encontra associado para determinar a forma como são cobradas as taxas.
    * Escolha a região onde será utilizado o recurso. Atualmente, o serviço de reconhecimento de voz está disponível em regiões asiático, Europa do Norte e EUA oeste.
    * Escolha o escalão de preço, qualquer F0 (limitado a subscrição gratuita) ou S0 (subscrição padrão). Clique em **vista completa detalhes de preços** para obter informações completas sobre preços e utilização quotas para cada camada.
    * Criar um novo grupo de recursos para esta subscrição de reconhecimento de voz ou atribuí-la a um existente. Ajuda a que manter as várias subscrições do Azure organizadas de grupos de recursos.
    * Para obter acesso prático para a sua subscrição no futuro, marcar a **afixar ao dashboard** caixa de verificação.
    * Clique em **criar.**

    ![Clique em criar no painel](media/index/try-speech-api-create-speech4.png)

    Pode demorar alguns minutos a criar e implementar o novo recurso de reconhecimento de voz. O painel de início rápido é apresentada com informações sobre o seu novo recurso.

    ![Painel de início rápido](media/index/try-speech-api-create-speech5.png)

1. Clique em de **chaves** ligação no passo 1, no painel de início rápido para apresentar as chaves de subscrição. Cada subscrição tem duas chaves; Pode utilizar na sua aplicação. Clique no botão junto a cada chave para a copiar para a área de transferência para colar no seu código.

> [!NOTE]
> Pode criar qualquer número de subscrições do escalão standard numa ou várias regiões. No entanto, pode criar apenas uma subscrição do escalão gratuito.

## <a name="next-steps"></a>Passos Seguintes

Transferir um SDK e algum código de exemplo para o serviço de reconhecimento de voz.

> [!div class="nextstepaction"]
> [SDKs de reconhecimento de voz](speech-sdk.md)

> [!div class="nextstepaction"]
> [Código de exemplo](samples.md)
