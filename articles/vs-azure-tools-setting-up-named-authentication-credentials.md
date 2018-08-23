---
title: Configurar as credenciais de autenticação com nome | Documentos da Microsoft
description: Saiba como fornecer credenciais que o Visual Studio podem utilizar para autenticar pedidos para o Azure, pelo que pode publicar uma aplicação para o Azure a partir do Visual Studio ou monitorizar um serviço cloud existente.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 61570907-42a1-40e8-bcd6-952b21a55786
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2017
ms.author: ghogen
ms.openlocfilehash: 48c410df1768c2cae7807aa1538d76867a581c71
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061029"
---
# <a name="set-up-named-authentication-credentials"></a>Configurar as credenciais de autenticação com nome

Para publicar uma aplicação no Azure ou para monitorizar um serviço cloud existente, o Visual Studio requer credenciais para autenticar pedidos para o Azure, ou seja, o ID de subscrição do Azure e um certificado X.509 válido de v3 com uma chave de, pelo menos, 2048 bits. Fornecer estas credenciais através de um dos seguintes métodos:

- Selecione Visual Studio **vista > Explorador de servidores**, com o botão direito a **Azure** nó, selecione **ligar a subscrição do Microsoft Azure**e iniciar sessão.
- Crie um ficheiro de subscrição (`.publishsettings`), que contém uma chave pública do certificado. O ficheiro de subscrição pode conter credenciais para a mais do que uma subscrição, conforme descrito neste artigo.

Nota: estas credenciais são diferentes das credenciais utilizadas para autenticar pedidos para serviços de armazenamento do Azure.

## <a name="create-a-subscription-file"></a>Crie um ficheiro de subscrição

No Explorador de servidores, clique com botão direito a **Azure** nó e selecione **gerir e subscrições de filtro**. Em seguida, selecione o **certificados** separador e, em seguida, efetue uma das seguintes ações:

- Selecione **importação** para abrir o **subscrições do Azure do Import Microsoft** caixa de diálogo. Selecione o **ficheiro de transferência de subscrição** associar e, no browser guarde o ficheiro transferido para uma localização temporária. Na caixa de diálogo, navegue até à localização de transferência e, em seguida, importá-lo para utilização na autenticação.
- Escolha uma subscrição ativa e selecione **editar**, que abre uma caixa de diálogo na qual editar uma subscrição existente para utilizar na autenticação.
- Selecione **New** para abrir o **nova subscrição** diálogo caixa e forneça os detalhes necessários. Para carregar o certificado para a cloud service estão apontados na caixa de diálogo, inicie sessão no portal do Azure, navegue para o serviço de nuvem, selecione **definições > certificados de gestão**, selecione **carregar**, em seguida, Especifique o caminho para o `.cer` ficheiro.

Se pretender criar um certificado por conta própria, pode consultar as instruções em [criar e carregar um gerenciamento de certificados para o Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) e, em seguida, carregar manualmente o certificado para o [portal do Azure](https://portal.azure.com/).

## <a name="next-steps"></a>Passos Seguintes

- [Visão geral das aplicações Web](https://docs.microsoft.com/azure/app-service/)
- [Implementar a aplicação no App Service do Azure](https://docs.microsoft.com/azure/app-service/app-service-deploy-local-git) 
- [Implementar o WebJobs com o Visual Studio](https://docs.microsoft.com/azure/app-service/websites-dotnet-deploy-webjobs)
- [Criar e implementar um serviço cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)