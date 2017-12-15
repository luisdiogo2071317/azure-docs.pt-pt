---
title: "Gerir uma área de trabalho do Machine Learning | Microsoft Docs"
description: "Gerir o acesso a áreas de trabalho do Azure Machine Learning, implementar e gerir serviços de web da ML API"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2e4b2869b6eac9670853832d58bc37f1cb0ed001
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Gerir uma área de trabalho do Azure Machine Learning

> [!NOTE]
> Para obter informações sobre a gestão de serviços Web no portal de serviços Web Machine Learning, consulte [gira um serviço Web utilizando o portal de serviços Web do Azure Machine Learning](manage-new-webservice.md).
> 
> 

Pode gerir áreas de trabalho do Machine Learning no portal do Azure.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

Para gerir uma área de trabalho no portal do Azure:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) utilizando uma conta de administrador de subscrição do Azure.
2. Na caixa de pesquisa na parte superior da página, introduza "machine learning áreas de trabalho" e, em seguida, selecione **áreas de trabalho do Machine Learning**.
3. Clique em da área de trabalho que pretende gerir.

Além das informações de gestão de recursos padrão e as opções disponíveis, pode:

- Vista **propriedades** - esta página apresenta as informações de área de trabalho e recursos e pode alterar o grupo de recursos e subscrição esta área de trabalho está associado.
- **Ressincronizar chaves armazenamento** -a área de trabalho mantém chaves para a conta de armazenamento. Se a conta de armazenamento alterações chaves, em seguida, pode clicar em **ressincronizar chaves** para sincronizar as chaves com área de trabalho.

Para gerir os serviços web associados a esta área de trabalho, utilize o portal de serviços Web Machine Learning. Consulte [gira um serviço Web utilizando o portal de serviços Web do Azure Machine Learning](manage-new-webservice.md) para obter informações completas.

> [!NOTE]
> Para implementar ou gerir novos serviços web tem de ser atribuída uma função de contribuinte ou de administrador na subscrição em que o serviço web é implementado. Se convidar outro utilizador para uma área de trabalho de aprendizagem, tem de atribuir-lhes uma função de administrador ou de Contribuidor na subscrição antes de poderem implementar ou gerir serviços web. 
> 
>Para obter mais informações sobre a definição de permissões de acesso, consulte [atribuições de acesso de vista para utilizadores e grupos no portal do Azure](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="next-steps"></a>Passos seguintes
* Saiba mais sobre [implementar Machine Learning com modelos do Azure Resource Manager](deploy-with-resource-manager-template.md). 