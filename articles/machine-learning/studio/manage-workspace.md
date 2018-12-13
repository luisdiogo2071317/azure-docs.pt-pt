---
title: Gerir uma área de trabalho do Machine Learning Studio | Documentos da Microsoft
description: Gerir o acesso a áreas de trabalho do Azure Machine Learning e implementar e gerir os serviços da web de API de ML
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: 8c5dfd82a7bf0d1985869c8de4e3b313ef885947
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270778"
---
# <a name="manage-an-azure-machine-learning-studio-workspace"></a>Gerir uma área de trabalho do Azure Machine Learning Studio

> [!NOTE]
> Para obter informações sobre o gerenciamento de serviços da Web no portal de serviços Web Machine Learning, consulte [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md).
> 
> 

Pode gerir áreas de trabalho do Machine Learning no portal do Azure.



## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

Para gerir uma área de trabalho no portal do Azure:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) através de uma conta de administrador de subscrição do Azure.
2. Na caixa de pesquisa na parte superior da página, introduza "machine learning áreas de trabalho" e, em seguida, selecione **áreas de trabalho do Machine Learning**.
3. Clique a área de trabalho que pretende gerir.

Além das informações de gestão de recursos padrão e as opções disponíveis, pode:

- Modo de exibição **propriedades** - esta página apresenta as informações da área de trabalho e recursos, e pode alterar o grupo de recursos e subscrição ligada com esta área de trabalho.
- **Ressincronizar chaves de armazenamento** -a área de trabalho mantém chaves para a conta de armazenamento. Se a conta de armazenamento é alterado as chaves, em seguida, pode clicar em **ressincronizar chaves** para sincronizar as chaves com a área de trabalho.

Para gerir os serviços web associados a esta área de trabalho, utilize o portal de serviços Web Machine Learning. Ver [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md) para obter informações completas.

> [!NOTE]
> Para implementar ou gerir os novos serviços web tem de ser atribuída uma função de Contribuidor ou administrador da subscrição em que o serviço web é implementado. Se convidar outro utilizador para uma área de trabalho de aprendizagem automática, deve atribuí-las a uma função de Contribuidor ou administrador da subscrição para poder implementar ou gerir os serviços web. 
> 
>Para obter mais informações sobre a definição de permissões de acesso, consulte [gerir o acesso com RBAC e o portal do Azure](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [implementar o Machine Learning com modelos do Azure Resource Manager](deploy-with-resource-manager-template.md). 