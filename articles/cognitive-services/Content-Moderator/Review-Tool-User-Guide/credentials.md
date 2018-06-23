---
title: As credenciais no Azure conteúdo Moderator | Microsoft Docs
description: Gerir conteúdo Moderator as credenciais a utilizar com as APIs.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 4531fa4c8bbb7bb9c1daeaaac6f9e7078dae250a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351518"
---
# <a name="manage-credentials"></a>Gerir credenciais

As suas credenciais de conteúdo Moderator são criados nas seguintes localizações:

- [O portal do Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [A ferramenta de revisão Moderator conteúdo](http://contentmoderator.cognitive.microsoft.com/)

Este artigo explica onde encontrá-los e como se relacionam entre si.

## <a name="the-azure-portal"></a>O portal do Azure

No dashboard do portal do Azure, selecione a sua conta Moderator conteúdo. Em **gestão de recursos**, selecione **chaves**. Para copiar a chave, selecione o ícone para a direita da chave.

![Conteúdo Moderator chaves no portal do Azure](images/credentials-azure-portal-keys.PNG)

### <a name="how-to-use-your-azure-account-with-the-review-tool"></a>Como utilizar a sua conta do Azure com a ferramenta de revisão
Para utilizar a sua chave do Azure com a revisão APIs, copie o ID de recurso indicado no **propriedades** ecrã na captura de ecrã seguinte e introduza-o no ecrã de credenciais a ferramenta de revisão do **indicados de recurso na lista de permissões** campos conforme é mostrado a seguir **ID de recurso** secção. 

Para utilizar a chave do Azure para os fluxos de trabalho disponíveis no conteúdo Moderator, introduza-o no **Ocp-Apim-Subscription-Key** campo no **definições de fluxo de trabalho** secção como é mostrado a seguir  **Fluxos de trabalho** secção.

> [!NOTE]
> Depois de substituir os dois locais na ferramenta de revisão com o ID de recurso e a chave da sua subscrição do Azure, o **avaliação Ocp-Apim-Subscription-Key** apresentado nas credenciais ecrã deixa de ser utilizado, mas está sempre disponível.
> A chave de avaliação limita-a para 5000 transações máximas por mês, 1 pedidos por segundo (RPS).

![ID de recurso Moderator conteúdo no portal do Azure](images/credentials-azure-portal-resourceid.PNG)


## <a name="the-review-tool"></a>A ferramenta de revisão

Na revisão ferramenta Dashboard, o **definições** separador, selecione **credenciais**.

![Conteúdo Moderator as credenciais na ferramenta de revisão](images/credentials-trial-resource-workflow.PNG)

A secção seguinte analisa a imagem anterior mais detalhadamente:


### <a name="api"></a>API

As listas de parte primeiro o **reveja o ponto final de API**, **equipa ID**e o **Ocp-Apim-Subscription-Key (chave de avaliação de conteúdo Moderator)** gerado como parte da sua equipa de revisão criação. Utilizá-los para chamar todos os APIs de Moderator conteúdo, incluindo a API de revisão.

Tenha também em atenção o identificador de região para o ponto final de API. Por exemplo, **westus** é a região no "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Chave de conteúdo Moderator na ferramenta de revisão](images/credentials-trialkey.PNG)


### <a name="resource-id"></a>ID do Recurso

O segundo parte é iniciado enviados como vazio com nenhum ID de recurso. **Para utilizar a sua chave de subscrição do Azure com a API de revisão, navegue para o ecrã de ID de recurso, conforme mostrado anteriormente e copie-o para o campo mostrado**. Atingiu o **'+'** para guardar o seu ID de recurso.

> [!NOTE]
> Região da sua subscrição Moderator conteúdo deve corresponder à região a equipa de revisão para reconhecer a sua equipa e aceder aos dados de equipa. Por exemplo, nas imagens nesta página, o **EUA oeste** região **(4)** contém a subscrição do Azure de Moderator conteúdo e a sua equipa de revisão.

![ID de recurso Moderator conteúdo na ferramenta de revisão](images/credentials-resourceids.PNG)


### <a name="workflows"></a>Fluxos de trabalho

A parte terceira mostra informações utilizadas para executar fluxos de trabalho. Começa que mostra a chave de avaliação gerado automaticamente por predefinição. 

**A atualização com a chave do Azure ao obter uma subscrição do Azure**. Os outros dois campos permitem utilizando o termo e imagem lista em operações de texto de ecrã e avaliar imagem respetivamente.

![Credenciais de fluxo de trabalho Moderator conteúdas na ferramenta de revisão](images/credentials-workflow.PNG)


## <a name="next-steps"></a>Passos Seguintes

* Saiba como utilizar as credenciais de Moderator conteúdo na sua [fluxos de trabalho](workflows.md).
