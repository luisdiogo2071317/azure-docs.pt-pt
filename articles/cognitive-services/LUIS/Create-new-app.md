---
title: Criar uma nova aplicação com LUIS | Microsoft Docs
description: Criar e gerir as suas aplicações na Web compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 75edd39346995cdef72bb1e1fcb9eaff53d29702
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355754"
---
# <a name="create-an-app"></a>Criar uma aplicação
Criar uma nova aplicação de formas diferentes: 

* [Iniciar](#create-new-app) com uma aplicação em branco e criar entidades, utterances e pendentes.
* [Iniciar](#create-new-app) com uma aplicação em branco e adicione um [domínio prebuilt](luis-how-to-use-prebuilt-domains.md).
* [Importar uma aplicação LUIS](#import-new-app) de um ficheiro JSON que já contenha pendentes, utterances e entidades.

## <a name="what-is-an-app"></a>O que é uma aplicação
A aplicação contém [versões](luis-how-to-manage-versions.md) do seu modelo, bem como qualquer [colaboradores](luis-how-to-collaborate.md) para a aplicação. Quando cria a aplicação, selecione o idioma ([idioma](luis-supported-languages.md)) que **não é possível alterar mais tarde**. 

A versão predefinida de uma nova aplicação é "0.1." 

Pode criar e gerir as suas aplicações num **aplicações My** página. Pode sempre aceder a esta página selecionando **as minhas aplicações** na barra de navegação superior do [LUIS](luis-reference-regions.md) Web site. 

[![](media/luis-create-new-app/apps-list.png "Captura de ecrã da lista de aplicações")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Criar nova aplicação

1. No **aplicações My** página, selecione **criar nova aplicação**.
2. Na caixa de diálogo, o nome da aplicação "TravelAgent".

    ![Criar nova caixa de diálogo de aplicação](./media/luis-create-new-app/create-app.png)

3. Escolha o idioma de aplicação (para a aplicação de TravelAgent, escolha inglês) e, em seguida, selecione **feito**. 

    >[!NOTE]
    >A cultura não pode ser alterada depois da aplicação é criada. 

## <a name="import-new-app"></a>Importar a nova aplicação
Pode definir o nome (máximo de 50 char), a versão (máximo de 10 char) e a descrição de uma aplicação no ficheiro de JSON. Exemplos de ficheiros JSON de aplicação estão disponíveis em [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. No **aplicações My** página, selecione **importação nova aplicação**.
2. No **importação nova aplicação** caixa de diálogo, selecione o ficheiro JSON que define a aplicação de LUIS.

    ![Importar uma nova caixa de diálogo de aplicação](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Aplicação de exportação
1. No **aplicações My** página, selecione as reticências (…) no final da linha de aplicação.

    [![](media/luis-create-new-app/apps-list.png "Captura de ecrã da caixa de diálogo de pop-up de ações de por aplicação")](media/luis-create-new-app/three-dots.png#lightbox)

2. Selecione **exportar aplicação** no menu. 

## <a name="rename-app"></a>Mudar o nome de aplicação

1. No **aplicações My** página, selecione as reticências (…) no final da linha de aplicação. 
2. Selecione **mudar o nome** no menu.
3. Introduza o novo nome da aplicação e selecione **feito**.

## <a name="delete-app"></a>Eliminar aplicação

> [!CAUTION]
> Está a eliminar a aplicação para todos os colaboradores e o proprietário. [Exportar](#export-app) a aplicação antes de eliminá-lo. 

1. No **aplicações My** página, selecione as reticências (…) no final da linha de aplicação. 
2. Selecione **eliminar** no menu.
3. Selecione **Ok** na janela de confirmação.

## <a name="export-endpoint-logs"></a>Exportar registos de ponto final
Os registos contém a consulta, a hora UTC e resposta LUIS JSON.

1. No **aplicações My** página, selecione as reticências (…) no final da linha de aplicação. 
2. Selecione **exportar registos de ponto final** no menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Passos Seguintes

A primeira tarefa na aplicação consiste em [adicionar pendentes](luis-how-to-add-intents.md).