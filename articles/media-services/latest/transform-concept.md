---
title: Transforma e tarefas nos serviços de multimédia do Azure | Microsoft Docs
description: Quando utilizar os Media Services, terá de criar uma transformação para descrever as regras ou especificações para processar os seus vídeos. Este artigo fornece uma descrição geral da transformação é e como utilizá-la.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: d256d87548d54951cb77beffb88bba26a1a3de49
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="transforms-and-jobs"></a>Transformações e tarefas

## <a name="overview"></a>Descrição geral 

A versão mais recente da API de REST de serviços de suporte de dados do Azure (v3) introduz um novo recurso do fluxo de trabalho transformada em modelo para codificação e/ou analisar vídeos, chamados um **transformar**. **Transforma** pode ser utilizado para configurar as tarefas comuns para codificação ou analying vídeos. Cada **transformar** descreve um fluxo de trabalho simple de tarefas para processar os ficheiros de vídeos ou áudio. 

O **transformar** objeto é de receitas e um **tarefa** é o atual pedido de Media Services do Azure aplicam-se de que **transformar** para um determinado conteúdo vídeo ou áudio de entrada. O **tarefa** Especifica informações como a localização da vídeo de entrada e a localização para a saída. Pode especificar a localização da sua utilização de vídeo: URLs HTTP (s), os URLs de SAS ou um caminho para ficheiros localizados localmente ou no Blob storage do Azure. Pode ter até 100 transformações na sua conta de Media Services do Azure e submeter as tarefas nesses transformações. Pode, em seguida, subscrever eventos, tais como alterações de estado de tarefa, utilizando as notificações, integram diretamente com o sistema de notificação de grelha de eventos do Azure. 

Uma vez que esta API é suscitada pelo departamento pelo Gestor de recursos do Azure, pode utilizar modelos do Resource Manager para criar e implementar as transformações na sua conta de Media Services. Controlo de acesso baseado em funções também pode ser definido ao nível do recurso nesta API, permitindo-lhe bloquear o acesso a recursos específicos, como as transformações.

## <a name="typical-workflow-and-example"></a>Fluxo de trabalho normal e de exemplo

1. Criar uma transformação 
2. Submeter tarefas dessa transformação 
3. Transformações de lista 
4. Elimine uma transformação, se não estiver a planear utilizar este "receitas" no futuro. 

Suponha que pretendia, a extrair o primeiro intervalo de todos os seus vídeos como uma imagem em miniatura – serem os passos que deverá efetuar: 

1. Definir a regra para processamento – por exemplo, utilize o primeiro intervalo do vídeo como a miniatura 
2. Em seguida, para cada vídeo que tiver como entrada para o serviço, seria informar o serviço: 
    1. Onde encontrar esse vídeo, e 
    2. Onde pretende escrever a saída – por exemplo, a imagem em miniatura 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ficheiros de fluxo de vídeos](stream-files-dotnet-quickstart.md)
