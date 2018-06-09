---
title: incluir ficheiro
description: incluir ficheiro
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 06/08/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: e7f3f80c886f90a8bc3ae8c38e7d101c506439a6
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35250227"
---
Para eliminar um contentor de volume, tem de
 - Elimine os volumes no contentor de volume. Se o contentor de volume tiver associados volumes, colocar os volumes offline pela primeira vez. Siga os passos no [colocar offline um volume](../articles/storsimple/storsimple-8000-manage-volumes-u2.md#take-a-volume-offline). Depois dos volumes estão offline, podem eliminá-los. 
 - Elimine as políticas de cópia de segurança associadas e instantâneos de nuvem. Verifique se o contentor de volume possui associados instantâneos de nuvem e de políticas de cópia de segurança. Se assim for, em seguida, [eliminar as políticas de cópia de segurança](../articles/storsimple/storsimple-8000-manage-backup-policies-u2.md#delete-a-backup-policy). Isto também irá eliminar os instantâneos de nuvem. 
 
Quando o contentor de volume não tem nenhum volumes associados, políticas de cópia de segurança e instantâneos de nuvem, pode eliminá-lo. Execute o seguinte procedimento para eliminar um contentor de volume.

#### <a name="to-delete-a-volume-container"></a>Para eliminar um contentor de volume
1. Aceda ao seu serviço Gestor de Dispositivos do StorSimple e clique em **Dispositivos**. Selecione e clique no dispositivo e, em seguida, aceda a **definições > Gerir > contentores de Volume**.

    ![Painel de contentores de volume](./media/storsimple-8000-create-volume-container/createvolumecontainer2.png)

2. Na lista de tabela de contentores de volume, selecione o contentor de volume que pretende eliminar, clique com o botão direito **...**  e, em seguida, selecione **eliminar**.

    ![Eliminar o contentor de volume](./media/storsimple-8000-delete-volume-container/deletevolumecontainer1.png)

3. Se tiver um contentor de volume não volumes associados, políticas de cópia de segurança e instantâneos de nuvem, em seguida, pode ser eliminado. Quando lhe for pedido para confirmação, reveja e selecione a caixa de verificação indicar o impacto de que pretende eliminar o contentor de volume. Clique em **eliminar** , em seguida, eliminar o contentor de volume.

    ![Confirmar eliminação](./media/storsimple-8000-delete-volume-container/deletevolumecontainer2.png)

A lista de contentores de volume é atualizada para refletir o contentor de volume eliminado.

![](./media/storsimple-8000-delete-volume-container/deletevolumecontainer5.png)


