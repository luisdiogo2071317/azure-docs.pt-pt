---
title: Registar a sua aplicação - serviços cognitivos do Azure | Microsoft Docs
description: Um passo a passo guiam como registar uma nova aplicação com o serviço de decisão de personalizada do Azure
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo;alekh;marossi
ms.openlocfilehash: 2aa8fbe77c11df4434eefa4c92d8529d5ca1d885
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354517"
---
# <a name="register-your-application"></a>Registar a sua aplicação

Para utilizar o serviço de decisão de personalizado para a sua aplicação, tem de registá-lo no portal. Este artigo explica como.

1. Vá para o [página](https://ds.microsoft.com/) do serviço de decisão personalizada. No Friso, clique em **My Portal**, como realçado na imagem:

    ![A minha Portal](./media/portal.png)

    Se já não tiver sessão iniciada, o portal pede-lhe para iniciar sessão com a [conta Microsoft](https://account.microsoft.com/account). Depois de ter sessão iniciada, o portal apresenta a conta Microsoft no canto superior direito da página.

2. Para registar a sua aplicação, clique em de **nova aplicação** botão.

3. Na caixa de diálogo, escolha um ID de aplicação para a sua aplicação. Serviço de decisão personalizado requer um ID exclusivo para cada aplicação. Se alguém já realizou este ID, o sistema pede-lhe para escolher outra.

4. Especifique um conjunto de ação API. Esta definição é um RSS ou Atom feed que comunica o conteúdo disponível para a aplicação do serviço de decisão personalizada. Introduza um nome para o feed e introduza o URL a partir do qual é servido. Para efetuar este passo mais tarde, clique em de **Feeds** botão e, em seguida, clique em de **feed novo** botão. Um exemplo que cria um feed RSS é descrito posteriormente.

5. Para registar a sua aplicação, selecione o **aplicação personalizada** caixa de verificação no canto inferior esquerdo. Introduza um [cadeia de ligação](../../storage/common/storage-configure-connection-string.md) para a conta de armazenamento do Azure onde os dados da aplicação são registados. Para obter mais informações sobre como criar uma conta de armazenamento, consulte [como criar, gerir ou eliminar uma conta de armazenamento](../../storage/common/storage-create-storage-account.md).

### <a name="next-steps"></a>Passos Seguintes

* Começar a utilizar para otimizar [uma página Web](custom-decision-service-get-started-browser.md) ou [uma aplicação de smartphone](custom-decision-service-get-started-app.md).
* Trabalhar através de um [tutorial](custom-decision-service-tutorial-news.md) para obter um exemplo mais aprofundado.
* Consulte o [referência da API](custom-decision-service-api-reference.md) para saber mais sobre a funcionalidade fornecida.