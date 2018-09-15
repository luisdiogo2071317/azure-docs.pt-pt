---
title: Gerir versões de aplicações do LUIS
titleSuffix: Azure Cognitive Services
description: Versões permitem-lhe criar e publicar modelos diferentes. É uma boa prática clonar o modelo de Active Directory atual para uma versão diferente da aplicação antes de efetuar alterações ao modelo.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: dcb7f47ae7ade4ee0a18801934f36576fc75f4ae
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629479"
---
# <a name="manage-versions"></a>Gerir versões

Versões permitem-lhe criar e publicar modelos diferentes. Uma boa prática é clonar o modelo de Active Directory atual para um diferente [versão](luis-concept-version.md) da aplicação antes de efetuar alterações ao modelo. 

Para trabalhar com as versões, abra a sua aplicação, selecionando o respetivo nome na **as minhas aplicações** página e, em seguida, selecione **gerir** na barra superior, em seguida, selecione **versões** na navegação à esquerda. 

A lista de versões mostram as versões que são publicadas, onde eles são publicados e qual é a versão está atualmente ativa. 

[![](./media/luis-how-to-manage-versions/versions-import.png "Gerir a seção, página de versões")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Clonar uma versão

1. Selecione a versão que pretende clonar em seguida, selecione **Clone** da barra de ferramentas. 

2. Na **versão de Clone** caixa de diálogo, escreva um nome para a nova versão, como "0,2".

   ![Caixa de diálogo de versão do clone](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Versão ID pode ser constituído apenas por carateres, dígitos ou "." e não pode ter mais de 10 caracteres.
 
 Uma nova versão com o nome especificado é criada e definida como a versão do Active Directory.

## <a name="set-active-version"></a>Definir versão Active Directory

Selecione uma versão da lista, em seguida, selecione **tornar Active** da barra de ferramentas. 

[![](./media/luis-how-to-manage-versions/versions-other.png "Gerir a seção, página de versões")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Versão de importação

1. Selecione **versão de importação** da barra de ferramentas. 

2. Na **nova versão de importação** janela pop-up, introduza o novo nome de versão de dez caractere. Apenas terá de definir um ID de versão, se a versão no ficheiro JSON já existe na aplicação.

    ![Gerir a seção, página de versões](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Depois de importar uma versão, a nova versão torna-se a versão do Active Directory.

<a name = "export-version"></a>

## <a name="other-actions"></a>Outras ações

* Para **elimine** uma versão, selecione uma versão da lista, em seguida, selecione **eliminar** da barra de ferramentas. Selecione **Ok**. 
* Para **mudar o nome** uma versão, selecione uma versão da lista, em seguida, selecione **mudar o nome** da barra de ferramentas. Introduza o novo nome e selecione **feito**. 
* Para **exportar** uma versão, selecione uma versão da lista, em seguida, selecione **aplicação de exportação** da barra de ferramentas. O ficheiro é transferido para o computador local. 

