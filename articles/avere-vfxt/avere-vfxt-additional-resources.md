---
title: Links adicionais sobre Avere vFXT para o Azure
description: Ligações para informações adicionais sobre Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 65e764d843f9e87adee4cf94c1d22b02db80eda0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958845"
---
# <a name="additional-documentation"></a>Documentação adicional

Este artigo tem ligações para documentação adicional sobre a interface de gestão do painel de controlo de Avere e tópicos relacionados. 

## <a name="avere-cluster-documentation"></a>Documentação do cluster Avere

Documentação do cluster de Avere adicional pode ser encontrada no site em <http://library.averesystems.com/>. Estes documentos podem ajudar a compreender os recursos do cluster e como configurar as respetivas definições. 

* O [guia de criação do Cluster FXT](<http://library.averesystems.com/#fxt_cluster>) foi concebido para clusters constituído por nós de hardware físico, mas algumas informações no documento são relevantes para os clusters de vFXT também. Em particular, os administradores de cluster do novo vFXT podem beneficiar de ler estas secções:
  * [Personalizar o suporte e as definições da monitorização](<http://library.averesystems.com/create_cluster/4_8/html/config_support.html#config-support>) explica como personalizar as definições de carregamento de suporte e ativar a monitorização remota. 
  * [Configurar VServers e espaço de nomes Global](<http://library.averesystems.com/create_cluster/4_8/html/config_vserver.html#config-vserver>) tem informações sobre como criar um espaço de nomes com clientes.
  * [Configurar o DNS para o cluster Avere](<http://library.averesystems.com/create_cluster/4_8/html/config_network.html#dns-overview>) explica como configurar o DNS round robin.
  * [Adicionar armazenamento de back-end](<http://library.averesystems.com/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) documenta como adicionar se filtram de núcleo.

* O [guia de configuração de Cluster](<http://library.averesystems.com/#operations>) é uma referência completa de definições e as opções para um cluster de Avere. Um cluster de vFXT utiliza um subconjunto destas opções, mas se aplicam a maior parte as mesmas páginas de configuração.

* O [guia de Dashboard](<http://library.averesystems.com/#operations>) explica como utilizar o cluster de funcionalidades do painel de controle Avere de monitorização.

## <a name="vfxt-creation-and-management-documentation"></a>documentação de criação e gestão de vFXT

Um guia completo para usar vfxt.py, a criação do cluster na cloud e o utilitário de gestão, é fornecido no GitHub: [Cloud de gestão de clusters com vfxt.py](https://github.com/Azure/AvereSDK/blob/master/docs/README.md).  
