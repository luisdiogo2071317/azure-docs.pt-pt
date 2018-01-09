---
title: "Configurar o Postman para chamadas de API de REST dos serviços de suporte de dados do Azure"
description: "Saiba como configurar o Postman para chamadas de API de REST dos serviços de suporte de dados."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2017
ms.author: juliako
ms.openlocfilehash: a24b73a93bddbeb5b56ddfdf604fa99faccea442
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Configurar o Postman para chamadas de API de REST dos serviços de suporte de dados

Este tutorial mostra como configurar **Postman** pelo que pode ser utilizado para chamar as APIs REST de serviços de suporte de dados do Azure (AMS). O tutorial mostra como importar ficheiros de ambiente e a coleção para **Postman**. A coleção contém definições agrupadas de pedidos HTTP chamar APIs REST do serviços de suporte de dados do Azure (AMS). O ficheiro de ambiente contém variáveis que são utilizadas por coleção.

Neste ambiente e a coleção é utilizada nos artigos que mostram como atingir várias tarefas com as APIs REST do serviços de suporte de dados do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Instalar o [Postman](https://www.getpostman.com/) cliente REST para executar as APIs REST mostrado em algumas dos tutoriais de REST de AMS. 

    Estamos a utilizar **Postman** , mas qualquer ferramenta REST seria adequada. Outras alternativas são: **Visual Studio Code** com o plug-in REST ou **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Configurar o ambiente 

1. Crie um ficheiro. JSON que contém as variáveis de ambiente utilizadas nos tutoriais de AMS. Nome de ficheiro **AzureMediaServices.postman_environment.json**. Abra o ficheiro e cole o código que define o ambiente do Postman de [listagem este código](postman-environment.md). 
2. Abra o **Postman**.
3. À direita do ecrã, selecione o **gerir ambiente** opção.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-create-env.png)
4. Do **gerir ambiente** caixa de diálogo, clique em **importação**.
5. Procurar e selecionar o **AzureMediaServices.postman_environment.json** ficheiro.
6. O **AzureMedia** ambiente é adicionado.
7. Feche a caixa de diálogo.
8. Selecione o **AzureMedia** ambiente.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Configurar a coleção

1. Criar um ficheiro. JSON que contém o **Postman** coleção com todas as operações que são necessários para carregar um ficheiro aos Media Services. Nome de ficheiro **AzureMediaServicesOperations.postman_collection.json**. Abra o ficheiro e cole o código que define o **Postman** coleção da [listagem este código](postman-collection.md).
2. Clique em **importar** para importar o ficheiro de coleção.
3. Escolha o **AzureMediaServicesOperations.postman_collection.json** ficheiro.

    ![Carregar um ficheiro](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Passos Seguintes

Veja o [carregar recursos](media-services-rest-upload-files.md) artigo.  
