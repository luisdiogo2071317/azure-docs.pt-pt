---
title: Criar uma conta de vídeo indexador ligada ao Azure | Microsoft Docs
description: Este artigo mostra como criar uma conta de vídeo indexador ligada ao Azure.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: ac9093d41a2e70905ea82c6d11f020696488ff27
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355867"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Criar uma conta de vídeo indexador ligada ao Azure

Quando utilizar uma conta de avaliação gratuita do indexador de vídeo, está limitada pela quota e número de vídeos que pode indexar. Este artigo mostra como criar uma conta de vídeo indexador que está ligada a uma subscrição do Azure, o que liberta a partir destes limites e utiliza o preço pay as you go.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. 

    Se não tiver uma subscrição do Azure ainda, inscreva-se [avaliação gratuita do Azure](https://azure.microsoft.com/free/).

* Um domínio do Azure Active Directory (AD). 

    Se não tiver um domínio do Azure AD, crie este domínio com a sua subscrição do Azure.

* Um utilizador e o membro no seu domínio do Azure AD. Irá utilizar este membro ao ligar a sua conta do indexador de vídeo para o Azure.

    Este utilizador deve cumpram estes critérios:

    * Ser um utilizador do Azure AD com um trabalho ou conta profissional, não uma conta pessoal, como o outlook.com, live.com ou hotmail.com.
        
        ![todos os utilizadores do AAD](./media/create-account/all-aad-users.png)

    *  Ser um membro na sua subscrição do Azure com uma função de proprietário ou contribuinte e administrador de acesso de utilizador de funções.

        ![Controlo de acesso](./media/create-account/access-control-iam.png)

## <a name="connect-to-azure"></a>Ligar ao Azure

1. Inicie sessão com esse utilizador e clique em de **ligar ao Azure** botão:

    ![Ligar ao Azure](./media/create-account/connect-to-azure.png)

2. Quando for apresentada a lista de subscrições, selecione a subscrição que pretende utilizar. 

    ![ligar as vídeo indexador para o Azure](./media/create-account/connect-vi-to-azure-subscription.png)

3. Selecione uma região do Azure a partir de localizações suportadas: EUA oeste, 2, Europa do Norte ou Ásia Oriental.
4. Em **conta de Media Services do Azure**, escolher uma destas opções:

    * Para criar uma nova conta de Media Services, selecione **criar novo grupo de recursos**. Forneça um nome para o grupo de recursos.

        Azure irá criar a sua nova conta na sua subscrição, incluindo uma nova conta de armazenamento do Azure. A nova conta de Media Services tem uma configuração inicial da predefinição com um ponto final de transmissão em fluxo e 10 unidades reservadas de S3.
    * Para utilizar uma conta de Media Services existente, selecione **utilizar recursos existentes**. Na lista de contas, selecione a sua conta.

        Conta de Media Services tem de ter a mesma região que a sua conta do indexador de vídeo. Para minimizar a duração de indexação e débito baixo, ajuste o tipo e número de unidades reservadas para **10 unidades reservadas de S3** na sua conta de Media Services.
    * Para configurar manualmente a ligação, clique o **mudar para a configuração manual** associar e forneça as informações necessárias:

    ![ligar as vídeo indexador para o Azure](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. Quando tiver terminado, escolha **Connect**. Esta operação poderá demorar alguns minutos. 

    Depois de se estiver ligado ao Azure, a sua nova conta de vídeo indexador aparece na lista de conta:

    ![nova conta](./media/create-account/new-account.png)

6. Navegue para a sua nova conta: 

    ![Conta de indexador vídeo](./media/create-account/vi-account.png)

## <a name="considerations"></a>Considerações

Aplicam as seguintes considerações relacionadas de Media Services do Azure:

* Se ligado a uma nova conta de Media Services, verá um novo grupo de recursos, a conta de Media Services e uma conta de armazenamento na sua subscrição do Azure.
* Se ligado a uma nova conta de Media Services, o indexador de vídeo definirá o suporte de dados **unidades reservadas** 10 S3 unidades:

    ![Unidades reservadas de Media Services](./media/create-account/ams-reserved-units.png)

* Se ligado a uma conta de Media Services existente, vídeo indexador não altera o suporte de dados existente **unidades reservadas** configuração.

    Poderá ser necessário ajustar o tipo e número de suportes de dados **unidades reservadas**, de acordo com a carga planeada. Tenha em atenção que, se a carga for elevada e não tem suficiente unidades ou da velocidade, vídeos processamento podem resultar em falhas de tempo limite.

* Se ligado a uma nova conta de Media Services, o indexador de vídeo inicia automaticamente uma **ponto final de transmissão em fluxo** no mesmo:

    ![Ponto final de transmissão em fluxo de Media Services](./media/create-account/ams-streaming-endpoint.png)

* Se ligado a uma conta de Media Services existente, o indexador de vídeo não altera a configuração de pontos finais de transmissão em fluxo. Se não houver nenhuma execução **ponto final de transmissão em fluxo**, não será capaz de vídeos desta conta de Media Services ou o indexador de vídeo.

## <a name="use-video-indexer-apis-v2"></a>Utilize as vídeo indexador APIs v2

Pode interagir programaticamente com a sua conta de avaliação e/ou com as contas de vídeo indexador que estiver ligadas ao azure ao seguir as instruções em: [utilize APIs](video-indexer-use-apis.md).

Deve utilizar o mesmo utilizador do Azure AD que utilizou quando ligar ao Azure.

## <a name="next-steps"></a>Passos Seguintes

[Analise os detalhes do resultado JSON](video-indexer-output-json-v2.md).

