---
title: Gerir versões LUIS aplicações do Azure | Microsoft Docs
description: Saiba como gerir versões em aplicações de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: 672f7991be0fc236e39daf7d1ce1d6080b31815b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351703"
---
# <a name="manage-versions"></a>Gerir versões

Sempre que funciona em modelo, criar outra [versão](luis-concept-version.md) da aplicação. 

## <a name="set-active-version"></a>Definir versão Active Directory
Para trabalhar com as versões, abra a aplicação, selecionando o respetivo nome nos **aplicações My** página e, em seguida, selecione **definições** na barra superior.

![Página de versões](./media/luis-how-to-manage-versions/settings.png)

O **definições** página permite-lhe configurar as definições da aplicação completa, incluindo versões e colaboradores. 

## <a name="clone-a-version"></a>Clonar uma versão
1. No **definições** página, após as secções de definições de aplicações e os colaboradores, determinar a linha com a versão que pretende clonar. Selecione as reticências (…) na extremidade direita. 

    ![Propriedades de linha de versão](./media/luis-how-to-manage-versions/version-section.png)

2. Selecione **Clone** da lista.

    ![Opção de propriedades de linha de versão](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. No **versão Clone** caixa de diálogo, escreva um nome para a nova versão, tais como "0,2".

   ![Caixa de diálogo de versão do clone](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Versão ID pode ser composto apenas por carateres, dígitos ou '.' e não pode ter mais de 10 carateres de comprimento.
 
 Uma nova versão com o nome especificado é criada e definida como a versão do Active Directory.
 
  ![Versão é criada e adicionada à lista](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Como é mostrado na imagem anterior, uma versão publicada está associada uma marca de coloridos, que indica o tipo de ranhura em que foi publicado: produção (verde), de transição (vermelho) e de ambos (preto). As datas de formação e publicação são apresentadas para cada versão publicada.

## <a name="set-active-version"></a>Definir versão Active Directory
1. No **definições** na página de **versões** lista, selecione as reticências (…) na extremidade direita.

2. Na lista de pop-up, selecione **definida como activa**.

    ![Definir versão Active Directory](./media/luis-how-to-manage-versions/set-active-version.png)

    A versão do Active Directory é realçada por uma cor azul leve, conforme mostrado na captura de ecrã seguinte:

    ![A versão do Active Directory](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Versão de importação
Pode importar uma versão de um ficheiro JSON. Depois de importar uma versão, a nova versão torna-se a versão do Active Directory.

**Para importar uma versão:**

1. No **definições** página, selecione **nova versão de importação** botão.

    ![Botão Importar](./media/luis-how-to-manage-versions/import-version.png)

2. Selecione **procurar** e escolha o ficheiro JSON.

    ![Caixa de diálogo de versão de importação](./media/luis-how-to-manage-versions/import-version-dialog.png)

Só tem de definir um ID de versão, se a versão no ficheiro de JSON já existe na aplicação.

## <a name="export-version"></a>Versão de exportação
Pode exportar uma versão para um ficheiro JSON.

**Para exportar uma versão:**

1. No **definições** na página de **versões** lista, selecione as reticências (…) na extremidade direita.

2. Selecione **exportar** na lista de pop-up de ações e selecione onde pretende guardar o ficheiro.

## <a name="delete-a-version"></a>Eliminar uma versão
Pode eliminar versões, mas tem de manter, pelo menos, uma versão da aplicação. Pode eliminar todas as versões, exceto a versão do Active Directory. 

1. No **definições** na página de **versões** lista, selecione as reticências (…) na extremidade direita.

2. Selecione **eliminar** na lista de pop-up de ações e selecione onde pretende guardar o ficheiro.

    ![Eliminar a confirmação da versão](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Mudar o nome de uma versão
Pode mudar o nome de versões, desde que o nome da versão não está já em utilização.  

1. No **definições** na página de **versões** lista, selecione as reticências (…) na extremidade direita.

2. Selecione **mudar o nome** na lista de pop-up de ações.

3. Introduza o nome da nova versão e selecione **feito**.

    ![Mudar o nome de confirmação da versão](./media/luis-how-to-manage-versions/rename-popup.png) 
