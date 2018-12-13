---
title: Utilizar o site do indexador de vídeo para personalizar um modelo de pessoa - Azure
titlesuffix: Azure Media Services
description: Este artigo mostra como personalizar um modelo de pessoa com o site do Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 321db75b1a286e7de1e38ed3b382ee7dc9fe5902
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285383"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Personalizar um modelo de pessoa com o site do Video Indexer

O Video Indexer suporta a deteção de rostos e reconhecimento de celebridade para conteúdo de vídeo. O recurso de reconhecimento de celebridade abrange aproximadamente os rostos de um milhão com base na origem de dados frequentemente pedidos como IMDB, Wikipedia e influenciadores principais do LinkedIn. Rostos que não são reconhecidos pela funcionalidade de reconhecimento de celebridade são detetados; No entanto, eles são deixados sem nome. Depois de carregar o vídeo para o indexador de vídeos e obtenha os resultados, pode voltar atrás e nomear os rostos que não foram reconhecidos. Depois de identificar um rosto com um nome, o mostrador e o nome seja adicionados ao modelo de pessoa da sua conta. Indexador de vídeos, em seguida, irá reconhecer este rostos nos vídeos futuros e vídeos nos últimos.

Pode utilizar o site de indexador de vídeos para editar rostos que foram detetados um vídeo, conforme descrito neste tópico. Também pode utilizar a API, conforme descrito em [personalizar modelo de pessoa com APIs](customize-person-model-with-api.md).

## <a name="edit-a-face"></a>Editar um rosto

> [!NOTE]
> Os nomes são exclusivos para modelos de uma pessoa, para que se nome dois diferentes enfrenta o mesmo nome, o indexador de vídeos visualiza os rostos como a mesma pessoa e converge-las assim que a reindexar o seu vídeo. Se vir que o indexador de vídeos detetou várias ocorrências diferentes do mostrador da mesma, dar-lhes o mesmo nome e reindexar o seu vídeo.
> Pode atualizar um rosto que Video Indexer reconhecida como uma celebridade com um novo nome. O novo nome que dar irá ter precedência sobre o reconhecimento de celebridade incorporada.

1. Aceda ao site do [Video Indexer](https://www.videoindexer.ai/) e inicie sessão.
2. Procure um vídeo que pretende ver e editar na sua conta.
3. Para editar um rosto no seu vídeo, vá para o **Insights** separador e clique no ícone de lápis no canto superior direito da janela.

    ![Personalizar o modelo da pessoa](./media/customize-face-model/customize-face-model.png)

4. Clique em qualquer um dos rostos detetados e alterar os respetivos nomes de "#X desconhecido" (ou o nome que anteriormente era atribuído para o rosto).
5. Depois de escrever o novo nome, clique no ícone de verificação junto ao nome do novo. Isto irá guardar o novo nome e reconhecer e dê o nome todas as ocorrências deste face nos seus outros vídeos atuais e no futuro vídeos que carrega. Reconhecimento de rosto nos seus outros vídeos atuais poderá demorar algum tempo a entrar em vigor, como se trata de um processo em lotes. 

    ![Guardar a atualização](./media/customize-face-model/save-update.png)

## <a name="delete-a-face"></a>Eliminar um rosto

Para eliminar um rosto detetado no seu vídeo, vá para o **Insights** painel e clique no ícone de lápis no canto superior direito do painel. Clique nas **eliminar** opção sob o nome do mostrador da. Esta ação irá remover que detetados face do vídeo. A face ainda será detetada aos outros vídeos em que aparecem, mas pode eliminar a face desses vídeos também depois que eles têm sido indexados. O mostrador da também continuará a existir no modelo de pessoa da sua conta, se denominar antes de o eliminar.

## <a name="next-steps"></a>Passos Seguintes

[Personalizar o modelo de pessoa com APIs](customize-person-model-with-api.md)
