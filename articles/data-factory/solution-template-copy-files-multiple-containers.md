---
title: Copiar ficheiros de vários contentores com o Azure Data Factory | Documentos da Microsoft
description: Saiba como utilizar um modelo de solução para copiar ficheiros de vários contentores com o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/1/2018
ms.openlocfilehash: aa5f32594c295ab6a8e60af8359370f64f75a72d
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967415"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Copiar ficheiros de vários contentores com o Azure Data Factory

O modelo de solução descrito neste artigo ajuda-o a copiar ficheiros de vários ficheiros, contentores ou registos entre arquivos de ficheiros. Por exemplo, talvez queira migrar seu o data lake do AWS S3 para o Azure Data Lake Store. Ou talvez queira replicar tudo de uma conta de armazenamento de Blobs do Azure para outra conta de armazenamento de Blobs do Azure. Este modelo foi concebido para estes casos de utilização.

Se pretender copiar ficheiros de um único contentor ou o bucket, é mais eficiente para utilizar o **ferramenta de cópia de dados** para criar um pipeline com uma única atividade de cópia. Este modelo é mais do que precisa para este simples caso de utilização.

## <a name="about-this-solution-template"></a>Sobre este modelo de solução

Este modelo enumera os contentores de seu arquivo de armazenamento de origem e, em seguida, as cópias dos contentores de armazenamento de origem armazenam para o arquivo de destino. 

O modelo contém três atividades:
-   R **GetMetadata** atividade para analisar o arquivo de armazenamento de origem e obter a lista de contentores.
-   Uma **ForEach** atividade para obter a lista de contentor a partir do **GetMetadata** atividade e, em seguida, iterar sobre a lista e passar a cada contentor para a atividade de cópia.
-   R **cópia** atividade para copiar cada contentor a partir da loja de armazenamento de origem para o arquivo de destino.

O modelo define dois parâmetros:
-   O parâmetro *SourceFilePath* é o caminho do seu arquivo de origem de dados, onde pode obter uma lista dos contentores ou registos. Na maioria dos casos, o caminho é o diretório de raiz, que contém várias pastas de contentor. O valor padrão desse parâmetro é `/`.
-   O parâmetro *DestinationFilePath* é o caminho onde serão copiados os ficheiros no seu arquivo de destino. O valor padrão desse parâmetro é `/`.

## <a name="how-to-use-this-solution-template"></a>Como utilizar este modelo de solução

1. Aceda ao modelo **copiar vários contentores de arquivos entre arquivos de ficheiros**e criar um **nova ligação** ao seu arquivo de armazenamento de origem. O arquivo de armazenamento de origem é o local onde pretende copiar ficheiros de vários contentores ou registos.

    ![Criar uma nova ligação para a origem](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Criar uma **nova ligação** ao seu arquivo de armazenamento de destino.

    ![Criar uma nova ligação para o destino](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Clique em **Utilize este modelo**.

    ![Utilizar este modelo](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Verá o pipeline disponível no painel, conforme mostrado no exemplo a seguir:

    ![Mostrar o pipeline](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Clique em **depurar**, introduza **parâmetros** e clique em **concluir**.

    ![Executar o pipeline](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Reveja o resultado.

    ![O resultado da revisão](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao Azure Data Factory](introduction.md)
