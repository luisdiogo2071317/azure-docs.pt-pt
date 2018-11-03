---
title: Aceder a nuvens privadas do Azure com o Visual Studio | Documentos da Microsoft
description: Saiba como aceder aos recursos de nuvem privada com o Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: ghogen
ms.openlocfilehash: 7067a9fddbb10f3c94d1104a5b81aeeaad6d0291
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969336"
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Aceder a nuvens privadas do Azure com o Visual Studio

Por predefinição, o Visual Studio suporta pontos finais REST de cloud do Azure. Neste artigo, saiba como utilizar o certificado de sua nuvem privada para aceder e interagir com a nuvem privada a partir do Visual Studio.

1. No portal do Azure para a nuvem privada, transfira o ficheiro de definições de publicação ou contacte o administrador para um ficheiro de definições de publicação. (O ficheiro tem a extensão `.publishsettings`.)

1. No Visual Studio **Explorador de servidores**, clique com botão direito a **Azure** nó e selecione **gerir e subscrições de filtro**.

    ![Gerir o comando de subscrições](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Na **gerir subscrições do Microsoft Azure** caixa de diálogo, selecione a **certificados** separador, em seguida, selecione **importar**.

    ![Importação de certificados do Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Na **Import Microsoft Azure subscrições** caixa de diálogo, selecione **procurar**.

    ![Procure o botão na caixa de diálogo de subscrições do Azure de Microsoft de importação](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. Na **aberto** caixa de diálogo, navegue para o diretório onde guardou o ficheiro de definições de publicação, selecione o ficheiro e, em seguida, selecione **aberto**.

    ![Selecione o ficheiro de definições de publicação](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Quando devolvidos para o **Import Microsoft Azure subscrições** caixa de diálogo, selecione **importação**.

    ![Importar o ficheiro de definições de publicação](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Os certificados são importados a partir do ficheiro de definições de publicação para Visual Studio e, agora pode interagir com os seus recursos de nuvem privada.

