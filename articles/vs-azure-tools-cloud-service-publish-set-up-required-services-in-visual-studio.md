---
title: Preparar para publicar ou implementar um serviço de Cloud a partir do Visual Studio | Documentos da Microsoft
description: Saiba os procedimentos para configurar serviços de nuvem e de armazenamento de contas e configurar a sua aplicação do Azure.
services: visual-studio-online
author: ghogen
manager: douge
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: cbd72198dbdbe2dd49f398e98806773cbb11d0f7
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969392"
---
# <a name="prepare-to-publish-or-deploy-a-cloud-service-from-visual-studio"></a>Preparar para publicar ou implementar um serviço de cloud a partir do Visual Studio

Para publicar um projeto de serviço em nuvem, tem de configurar os seguintes serviços conforme descrito neste artigo:

* R **serviço em nuvem** para executar suas funções no ambiente do Azure e 
* R **conta de armazenamento** que fornece acesso aos serviços Blob, filas e tabelas.

## <a name="create-a-cloud-service"></a>Criar um serviço cloud

Um serviço em nuvem executa suas funções no ambiente do Azure. Pode criar um serviço em nuvem no Visual Studio ou através da [portal do Azure](https://portal.azure.com/) conforme descrito nas seções a seguir.

### <a name="create-a-cloud-service-from-visual-studio"></a>Criar um serviço em nuvem a partir do Visual Studio

1. Com um projeto de serviço em nuvem criado anteriormente, com o botão direito de projeto, selecione **publicar**.
1. Se necessário, inicie sessão com a Microsoft ou uma conta organizacional associado à sua subscrição do Azure, em seguida, selecione **próxima** para avançar para o **definições** página.
1. R **criar serviço de nuvem e de conta de armazenamento** é apresentada a caixa de diálogo (se não estiver, selecione **criar nova** do **serviço em nuvem** lista).
1. Introduza um nome de maiúsculas e minúsculas do serviço em nuvem, o que faz parte da sua URL e tem de ser exclusivo. Também escolher uma região ou o grupo de afinidade e selecionar uma opção de replicação.

### <a name="create-a-cloud-service-through-the-azure-portal"></a>Criar um serviço em nuvem através do portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Selecione **serviços Cloud (clássico)** no lado esquerdo da página.
1. Selecione **+ adicionar**, em seguida, forneça as informações necessárias (DNS nome, subscrição, grupo de recursos e localização). Não é necessário carregar um pacote neste momento porque, fazê-lo mais tarde no Visual Studio.
1. Selecione **criar** para concluir o processo.

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Uma conta de armazenamento fornece acesso aos serviços Blob, filas e tabelas. Pode criar uma conta de armazenamento através do Visual Studio ou o [portal do Azure](https://portal.azure.com/).

### <a name="create-a-storage-account-from-visual-studio"></a>Criar uma conta de armazenamento a partir do Visual Studio

1. Na **Explorador de soluções** com um projeto de serviço em nuvem criado anteriormente, localize a **serviços ligados** nó dentro de um projeto de função, botão direito do mouse e selecione **Adicionar serviço ligado**. (No Visual Studio 2015 com o botão direito a **armazenamento** nó e selecione **criar conta de armazenamento**.)
1. Na **serviços ligados** lista que é apresentada, selecione **armazenamento na Cloud com o armazenamento do Azure**.
1. Na caixa de diálogo a armazenamento do Azure que aparece, selecione **+ criar nova conta de armazenamento**, o que exibe uma caixa de diálogo em que especificar a sua subscrição, um nome fo a conta, um preço camada, grupo de recursos e localização.
1. Selecione **criar** quando terminar. A nova conta de armazenamento é apresentada na lista de contas de armazenamento disponível na sua subscrição.
1. Selecione essa conta e selecione **adicionar**.

### <a name="create-a-storage-account-through-the-azure-portal"></a>Criar uma conta de armazenamento através do portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Selecione **+ novo** no canto superior esquerdo.
1. Selecione **armazenamento** em "Azure Marketplace,", em seguida, **conta de armazenamento - blob, ficheiro, tabela, fila** do lado direito.
1. Indique as informações necessárias (nome, modelo de implementação e assim por diante.
1. Selecione **criar** para concluir o processo.

## <a name="configure-your-app-to-use-the-storage-account"></a>Configurar a sua aplicação para utilizar a conta de armazenamento

Depois de criar uma conta de armazenamento, ligar à mesma a partir do Visual Studio automaticamente atualiza as configurações de serviço para o projeto, incluindo URLs e as chaves de acesso.

Se criou um serviço em nuvem do Visual Studio com o **Adicionar serviço ligado**, pode verificar as ligações ao abrir `ServiceConfiguration.Cloud.cscfg` e `ServiceConfiguration.Local.cscfg`.

Se tiver criado um serviço em nuvem através do portal do Azure, siga as mesmas etapas em [criar uma conta de armazenamento a partir do Visual Studio](#create-a-storage-account-from-visual-studio) , mas selecione a conta existente em vez de criar uma nova. Visual Studio, em seguida, atualiza a configuração para.

Para configurar as definições de utilizam manualmente, as páginas de propriedade no Visual Studio para a função aplicável no seu projeto de serviço cloud (a função com o botão direito e selecione **propriedades**). Para obter mais informações, consulte [configurar uma cadeia de ligação para uma conta de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-multiple-services-project-configurations#configuring-a-connection-string-to-a-storage-account).

### <a name="about-access-keys"></a>Sobre chaves de acesso

O portal do Azure mostra os URLs que pode utilizar para aceder a recursos em cada um dos serviços de armazenamento do Azure e também as chaves de acesso primária e secundária para a sua conta. Utilize estas chaves para autenticar pedidos feitos contra os serviços de armazenamento.

A chave de acesso secundária fornece o mesmo acesso à sua conta de armazenamento como a chave de acesso primária e é gerada como uma cópia de segurança a chave de acesso primária estiver comprometida. Além disso, é recomendável que regenerar as chaves de acesso em intervalos regulares. É possível modificar uma definição da cadeia de ligação para utilizar a chave secundária enquanto volta a gerar a chave primária, em seguida, pode modificá-la para utilizar a chave primária regenerada enquanto volta a gerar a chave secundária.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a publicação de aplicações para o Azure a partir do Visual Studio, veja [publicação de um serviço de nuvem usando as ferramentas do Azure](vs-azure-tools-publishing-a-cloud-service.md).
