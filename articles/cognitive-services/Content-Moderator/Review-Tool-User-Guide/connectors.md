---
title: Utilizar conectores Moderator conteúdo do Azure para aceder a outras APIs | Microsoft Docs
description: Saiba como de acesso a outras APIs para os fluxos de trabalho de conteúdo Moderator utilizando conectores.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: d8114457e7079ca8772cab830bd011dcddf372f5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351602"
---
# <a name="connectors"></a>Conectores

Fluxos de trabalho de conteúdo Moderator do Azure podem utilizar outras APIs, para além do conteúdo Moderator APIs. Acesso a outras APIs utilizando um conector na Moderator conteúdo. O conector oferece uma ligação para as outras APIs.

Conteúdo Moderator inclui estes conectores predefinido:

* API de Emoções
* API Face
* Serviço de nuvem PhotoDNA

![Conectores disponíveis Moderator de conteúdo](images/connectors-1.png)

## <a name="verify-your-credentials"></a>Verificar as suas credenciais 

Antes de definir um fluxo de trabalho, certifique-se de que tem as credenciais válidas para o conector de API que pretende utilizar:

1.  Na ferramenta de revisão Dashboard, selecione **definições** > **conectores**.

  ![Conectores de selecionados Moderator conteúdos](images/connectors-2.png)

2.  Selecione o **editar** símbolo junto do conector que pretende verificar as credenciais para.

  ![Conteúdo Moderator selecione o símbolo de edição](images/connectors-3.png)

3.  É apresentada a chave de subscrição. Se efetuar todas as edições, selecione **guardar** quando tiver terminado.

  ![Página de conteúdo Moderator editar conectores](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>Adicionar um conector

1.  Antes de adicionar um conector, tem uma chave de subscrição. Na ferramenta de revisão Dashboard, selecione **definições** > **credenciais**. Selecione e copie o valor que está a ser o **Ocp-Admin-Subscription-Key** caixa.

2.  Selecione **conectores**. Selecione um dos conectores disponíveis que são apresentados na ferramenta de revisão Dashboard. Em seguida, selecione **Connect**. 

  ![Página de conteúdo Moderator adicionar conector](images/connectors-5.png)

3.  No **Ocp-Admin-Subscription-Key** caixa, cole a chave que copiou. Em seguida, selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como utilizar conectores para [definir fluxos de trabalho personalizados](workflows.md).
