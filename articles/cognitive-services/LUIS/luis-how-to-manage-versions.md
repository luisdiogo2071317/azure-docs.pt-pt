---
title: Gerir versões de aplicações de LUIS no Azure | Documentos da Microsoft
description: Saiba como gerir versões em aplicativos de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: ef5dadd94d3612500d3092bdbd601fdaa12d1701
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868025"
---
# <a name="manage-versions"></a>Gerir versões

Sempre que trabalha no modelo, criar outra [versão](luis-concept-version.md) da aplicação. 

## <a name="set-active-version"></a>Definir versão Active Directory
Para trabalhar com as versões, abra a sua aplicação, selecionando o respetivo nome na **as minhas aplicações** página e, em seguida, selecione **definições** na barra superior.

![Página de versões](./media/luis-how-to-manage-versions/settings.png)

O **definições** página permite-lhe configurar as definições para todo o aplicativo, incluindo versões e colaboradores. 

## <a name="clone-a-version"></a>Clonar uma versão
1. Sobre o **definições** página, após as seções de definições da aplicação e funcionários, localizar a linha com a versão que pretende clonar. Selecione as reticências (***...*** ) botão na extremidade direita. 

    ![Propriedades de linha de versão](./media/luis-how-to-manage-versions/version-section.png)

2. Selecione **Clone** da lista.

    ![Escolha de propriedades de linha de versão](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. Na **versão de Clone** caixa de diálogo, escreva um nome para a nova versão, como "0,2".

   ![Caixa de diálogo de versão do clone](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Versão ID pode ser constituído apenas por carateres, dígitos ou "." e não pode ter mais de 10 caracteres.
 
 Uma nova versão com o nome especificado é criada e definida como a versão do Active Directory.
 
  ![Versão é criada e adicionada à lista](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Conforme mostrado na imagem anterior, uma versão publicada está associada uma marca de coloridos, que indica o tipo de bloco em que foi publicado: produção (verde), o teste (vermelho) e como ambos (preto). As datas de treinamento e publicação são apresentadas para cada versão publicada.

## <a name="set-active-version"></a>Definir versão Active Directory
1. No **definições** página, além do **versões** , selecione as reticências (***...*** ) botão na extremidade direita.

2. Na lista pop-up, selecione **definida como activa**.

    ![Definir versão Active Directory](./media/luis-how-to-manage-versions/set-active-version.png)

    A versão do Active Directory é realçada por uma cor azul claro, conforme mostrado na captura de ecrã seguinte:

    ![A versão do Active Directory](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Versão de importação
Pode importar uma versão de um ficheiro JSON. Depois de importar uma versão, a nova versão torna-se a versão do Active Directory.

**Para importar uma versão:**

1. Sobre o **definições** página, selecione **nova versão de importação** botão.

    ![Botão Importar](./media/luis-how-to-manage-versions/import-version.png)

2. Selecione **procurar** e escolha o ficheiro JSON.

    ![Caixa de diálogo de versão de importação](./media/luis-how-to-manage-versions/import-version-dialog.png)

Apenas terá de definir um ID de versão, se a versão no ficheiro JSON já existe na aplicação.

## <a name="export-version"></a>Versão de exportação
Pode exportar uma versão para um ficheiro JSON.

**Para exportar uma versão:**

1. No **definições** página, além do **versões** , selecione as reticências (***...*** ) botão na extremidade direita.

2. Selecione **exportar** na lista pop-up de ações e selecione onde pretende guardar o ficheiro.

## <a name="delete-a-version"></a>Eliminar uma versão
Pode eliminar versões, mas deve manter, pelo menos, uma versão da aplicação. Pode eliminar todas as versões, exceto a versão do Active Directory. 

1. No **definições** página, além do **versões** , selecione as reticências (***...*** ) botão na extremidade direita.

2. Selecione **eliminar** na lista pop-up de ações e selecione onde pretende guardar o ficheiro.

    ![Eliminar confirmação de versão](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Mudar o nome de uma versão
Pode mudar o nome de versões, desde que o nome da versão não está já em utilização.  

1. No **definições** página, além do **versões** , selecione as reticências (***...*** ) botão na extremidade direita.

2. Selecione **mudar o nome** na lista pop-up de ações.

3. Introduza o nome da nova versão e selecione **feito**.

    ![Mudar o nome de confirmação de versão](./media/luis-how-to-manage-versions/rename-popup.png) 
