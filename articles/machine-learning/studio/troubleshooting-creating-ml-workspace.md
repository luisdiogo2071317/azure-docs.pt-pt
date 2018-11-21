---
title: 'Resolução de problemas: Criar e ligar a uma área de trabalho do Machine Learning | Documentos da Microsoft'
description: Soluções para problemas comuns de criar e ligar a uma área de trabalho do Azure Machine Learning
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: f86018c78de842717bf387c007fdd4c7cd048ef1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265207"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Guia de resolução de problemas: criar e ligar a uma área de trabalho do Machine Learning
Este guia fornece soluções para alguns encontrados com freqüência desafios quando estiver a configurar áreas de trabalho do Azure Machine Learning.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Proprietário da área de trabalho
Para abrir uma área de trabalho no Machine Learning Studio, precisa estar conectado à Account Microsoft que utilizou para criar a área de trabalho ou terá de receber um convite de proprietário para associar a área de trabalho. No portal do Azure pode gerir a área de trabalho, que inclui a capacidade para configurar o acesso.

Para obter mais informações sobre como gerir uma área de trabalho, consulte [gerir uma área de trabalho do Azure Machine Learning].

[Gerir uma área de trabalho do Azure Machine Learning]: manage-workspace.md

## <a name="allowed-regions"></a>Regiões permitidas
Machine Learning está atualmente disponível num número limitado de regiões. Se a sua subscrição não incluir uma destas regiões, poderá ver a mensagem de erro, "Tem não existem subscrições nas regiões permitidas."

Para pedir que uma região de ser adicionado à sua subscrição, criar um novo pedido de suporte da Microsoft a partir do portal do Azure, escolha **faturação** como o tipo de problema e siga as instruções para submeter o pedido.

## <a name="storage-account"></a>Conta de armazenamento
O serviço Machine Learning tem uma conta de armazenamento para armazenar dados. Pode utilizar uma conta de armazenamento existente ou pode criar uma nova conta de armazenamento ao criar a nova área de trabalho do Machine Learning (se tiver de quota para criar uma nova conta de armazenamento).

Depois de criar a nova área de trabalho do Machine Learning, pode iniciar sessão Machine Learning Studio, utilizando a conta da Microsoft que utilizou para criar a área de trabalho. Se encontrar a mensagem de erro, "Área de trabalho não encontrada" (semelhante à seguinte captura de ecrã), utilize os seguintes passos para eliminar os cookies do browser.

![Área de trabalho não foi encontrada][screen3]

**Para eliminar cookies do browser**

1. Se usar o Internet Explorer, clique nas **ferramentas** no canto superior direito e selecione **opções da Internet**.  

![Opções da Internet][screen4]

2. Sob o **gerais** separador, clique em **eliminar...**

![Separador Geral][screen5]

3. Na **eliminar histórico de navegação** caixa de diálogo caixa, certifique-se **Cookies e dados do site** está selecionada e clique em **eliminar**.

![Eliminar cookies][screen6]

Depois dos cookies são eliminados, reinicie o navegador e, em seguida, vá para o [Microsoft Azure Machine Learning](https://studio.azureml.net) página. Quando lhe for pedido um nome de utilizador e palavra-passe, introduza a mesma conta Microsoft que utilizou para criar a área de trabalho.

## <a name="comments"></a>Comentários

Nosso objetivo é tornar a experiência de Machine Learning o mais direto possível. Publique quaisquer comentários e problemas com o [fórum do Azure Machine Learning](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) para ajudar-na atendê-lo melhor.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
