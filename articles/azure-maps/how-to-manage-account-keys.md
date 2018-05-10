---
title: Como gerir a sua conta de mapas do Azure e as chaves | Microsoft Docs
description: Pode utilizar o portal do Azure para gerir a sua conta Azure Maps e gerir as chaves de acesso.
services: azure-maps
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 091b91b51f7c880a1edbf65a0e2c83560d4bc38b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-manage-your-azure-maps-account-and-keys"></a>Como gerir a sua conta de mapas do Azure e as chaves

Pode gerir a sua conta do Azure Maps e chaves através do portal do Azure. Assim que tiver uma conta e uma chave, pode implementar as APIs no seu web site ou aplicação móvel.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-new-account"></a>Criar uma conta nova

1. Inicie sessão no [portal do Azure](http://portal.azure.com).

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.

2. Procure e selecione **Maps**, em seguida, clique em **criar**.

3. Introduza as informações para a sua nova conta. 

![Introduza as informações de conta no portal](./media/how-to-manage-account-keys/new-account-portal.png)

## <a name="manage-keys-on-the-account-page"></a>Gerir chaves na página de conta

Depois de criar uma conta, obter duas chaves geradas aleatoriamente. Utilize as teclas para se autenticarem nas APIs do Maps quando pretender obter dados de mapa ou criar uma nova instância de mapa de JavaScript. 

Pode encontrar as suas chaves no portal do Azure. Navegue até à sua conta, em seguida, selecione **chaves** no menu.

![Gerir chaves de conta no portal](./media/how-to-manage-account-keys/account-keys-portal.png)

Nesta página pode copiar as suas chaves ou gerar novos. 

## <a name="delete-an-account"></a>Eliminar uma conta

Pode eliminar uma conta do portal do Azure. Navegue para a página de descrição geral da conta e selecione **eliminar**.

![Eliminar a conta no portal](./media/how-to-manage-account-keys/account-delete-portal.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba como utilizar o [mapeia API de gestão](https://docs.microsoft.com/rest/api/maps-management/accounts) para criar, atualizar e eliminar contas de mapas. 