---
title: Criar uma nova aplicação com os LUIS | Documentos da Microsoft
description: Criar e gerir as suas aplicações na página de Web de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/17/2018
ms.author: v-geberr
ms.openlocfilehash: 998a85720f5707fbf6ed4c5cfa3ed0dab5d1cc0e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932911"
---
# <a name="create-an-app"></a>Criar uma aplicação
Criar uma nova aplicação de formas diferentes: 

* [Iniciar](#create-new-app) com uma aplicação vazia e crie objetivos, expressões e entidades.
* [Inicie](#create-new-app) com uma aplicação vazia e adicione um [domínio pré-criado](luis-how-to-use-prebuilt-domains.md).
* [Importar uma aplicação do LUIS](#import-new-app) de um ficheiro JSON que já contenha intenções, expressões e entidades.

## <a name="what-is-an-app"></a>O que é uma aplicação
A aplicação contiver [versões](luis-how-to-manage-versions.md) do seu modelo, bem como qualquer [colaboradores](luis-how-to-collaborate.md) para a aplicação. Ao criar a aplicação, selecione a cultura ([linguagem](luis-supported-languages.md)) que **não é possível alterar mais tarde**. 

A versão predefinida de uma nova aplicação é "0.1". 

Pode criar e gerir as suas aplicações no **as minhas aplicações** página. Pode sempre aceder a esta página selecionando **meus aplicativos** na barra de navegação superior do [LUIS](luis-reference-regions.md) Web site. 

[![](media/luis-create-new-app/apps-list.png "Captura de ecrã da lista de aplicações")](media/luis-create-new-app/apps-list.png#lightbox)

## <a name="create-new-app"></a>Criar nova aplicação

1. No **as minhas aplicações** página, selecione **criar nova aplicação**.
2. Na caixa de diálogo, nome de seu aplicativo "TravelAgent".

    ![Criar nova caixa de diálogo da aplicação](./media/luis-create-new-app/create-app.png)

3. Escolha sua cultura de aplicação (para aplicação de TravelAgent, selecione inglês) e, em seguida, selecione **feito**. 

    >[!NOTE]
    >Não é possível alterar a cultura, assim que o aplicativo for criado. 

## <a name="import-new-app"></a>Importar a nova aplicação
Pode definir o nome (no máximo, 50 char), a versão (no máximo, 10 char) e a descrição de uma aplicação no ficheiro JSON. Exemplos de ficheiros JSON da aplicação estão disponíveis em [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/Examples-BookFlight).

1. No **as minhas aplicações** página, selecione **importar a nova aplicação**.
2. Na **importar a nova aplicação** caixa de diálogo, selecione o ficheiro JSON da definição da aplicação do LUIS.

    ![Importar uma nova caixa de diálogo de aplicação](./media/luis-create-new-app/import-app.png)

## <a name="export-app"></a>Exportar a aplicação
1. No **as minhas aplicações** página, selecione as reticências (***...*** ) no final da linha da aplicação.

    [![](media/luis-create-new-app/apps-list.png "Captura de ecrã da caixa de diálogo pop-up de ações por aplicação")](media/luis-create-new-app/three-dots.png#lightbox)

2. Selecione **aplicação de exportação** no menu. 

## <a name="rename-app"></a>Mudar o nome da aplicação

1. No **as minhas aplicações** página, selecione as reticências (***...*** ) no final da linha da aplicação. 
2. Selecione **mudar o nome** no menu.
3. Introduza o novo nome da aplicação e selecione **feito**.

## <a name="delete-app"></a>Eliminar aplicação

> [!CAUTION]
> Está a eliminar a aplicação para todos os funcionários e o proprietário. [Exportar](#export-app) a aplicação antes de o eliminar. 

1. No **as minhas aplicações** página, selecione as reticências (***...*** ) no final da linha da aplicação. 
2. Selecione **eliminar** no menu.
3. Selecione **Ok** na janela de confirmação.

## <a name="export-endpoint-logs"></a>Exportar registos de ponto final
Os registos contêm a consulta, UTC e a resposta JSON de LUIS.

1. No **as minhas aplicações** página, selecione as reticências (***...*** ) no final da linha da aplicação. 
2. Selecione **exportar registos de ponto final** no menu.

```
Query,UTC DateTime,Response
text i'm driving and will be 30 minutes late to the meeting,02/13/2018 15:18:43,"{""query"":""text I'm driving and will be 30 minutes late to the meeting"",""intents"":[{""intent"":""None"",""score"":0.111048922},{""intent"":""SendMessage"",""score"":0.987501}],""entities"":[{""entity"":""i ' m driving and will be 30 minutes late to the meeting"",""type"":""Message"",""startIndex"":5,""endIndex"":58,""score"":0.162995353}]}"
```

## <a name="next-steps"></a>Passos Seguintes

Sua primeira tarefa na aplicação é [adicionar intenções](luis-how-to-add-intents.md).