---
title: Configurar um projeto do serviço cloud do Azure com o Visual Studio | Documentos da Microsoft
description: Saiba como configurar um projeto do serviço cloud do Azure no Visual Studio, dependendo dos requisitos para esse projeto.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/06/2017
ms.author: ghogen
ms.openlocfilehash: f119f4c758fcc7d89e5790ba9e3a223dc3a726c5
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969245"
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Configurar um projeto do serviço cloud do Azure com o Visual Studio
Pode configurar um projeto do serviço cloud do Azure, dependendo dos requisitos para esse projeto. Pode definir propriedades do projeto para as seguintes categorias:

- **Publicar um serviço em nuvem no Azure** -pode definir uma propriedade para se certificar de que um serviço cloud existente implementado no Azure não é eliminado acidentalmente.
- **Executar ou depurar um serviço em nuvem no computador local** -pode selecionar uma configuração de serviço para utilizar e indique se pretende iniciar o emulador de armazenamento do Azure.
- **Validar um pacote de serviço de nuvem quando for criado** -pode optar por tratar quaisquer avisos como erros, para que possa garantir que o pacote de serviço cloud implementa sem quaisquer problemas. 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a>Passos para configurar um projeto do serviço cloud do Azure
1. Abrir ou criar um projeto de serviço em nuvem no Visual Studio

1. Na **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **propriedades**.
   
1. Na página de propriedades do projeto, selecione o **desenvolvimento** separador.

    ![Menu de propriedades do projeto](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Definir **Prompt antes de eliminar uma implementação existente** ao **verdadeiro**. Esta definição ajuda a garantir que não elimina acidentalmente a uma implementação existente no Azure

1. Selecione o desejado **configuração do serviço** para indicar que a configuração do serviço que pretende utilizar quando executa ou depura o seu serviço cloud localmente. Para obter mais informações sobre como modificar uma configuração de serviço para uma função, veja [como configurar as funções para um serviço cloud do Azure com o Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Definir **emulador de armazenamento do Azure começar** ao **verdadeiro** para iniciar o emulador de armazenamento do Azure, quando executa ou depura o seu serviço cloud localmente.

1. Definir **os avisos como erros** ao **verdadeiro** para se certificar de que não é possível publicar se existem erros de validação do pacote.

1. Definir **utilizar as portas de projeto web** ao **verdadeiro** para se certificar de que a função da web usa a mesma porta sempre que for iniciado localmente no IIS Express.

1. Na barra de ferramentas da Visual Studio, selecione **guardar**.

## <a name="next-steps"></a>Passos Seguintes
- [Configurar um projeto do Azure utilizar várias configurações de serviço](vs-azure-tools-multiple-services-project-configurations.md)

