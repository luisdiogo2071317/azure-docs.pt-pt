---
title: Gerir funções de serviços cloud do Azure com o Visual Studio | Documentos da Microsoft
description: Saiba como adicionar e remover funções nos serviços cloud do Azure com o Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 023d70fd1f1ae2f79483f44a84cfedd5d1e39f3f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055892"
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Gerir funções de serviços cloud do Azure com o Visual Studio
Depois de ter criado o seu serviço cloud do Azure, pode adicionar novas funções a ele ou remover as funções existentes do mesmo. Também pode importar um projeto existente e convertê-lo a uma função. Por exemplo, pode importar uma aplicação web ASP.NET e designá-lo como uma função da web.

## <a name="adding-a-role-to-an-azure-cloud-service"></a>Adicionar uma função de um serviço cloud do Azure
Os seguintes passos guiá-lo por meio de adicionar uma função web ou de trabalho a um projeto de serviço cloud do Azure no Visual Studio.

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto

1. Com o botão direito a **funções** nó para exibir o menu de contexto. No menu de contexto, selecione **adicionar**, em seguida, selecione uma função web existente ou a função de trabalho da solução atual ou criar um projeto de função web ou de trabalho. Também pode selecionar um projeto apropriado, como um projeto de aplicativo web do ASP.NET e associá-lo a um projeto de função.

    ![Opções de menu para adicionar uma função a um projeto de serviço cloud do Azure](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Remover uma função a partir de um serviço cloud do Azure
Os seguintes passos guiá-lo por meio de remover uma função web ou de trabalho de um projeto do serviço cloud do Azure no Visual Studio.

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto

1. Expanda a **funções** nó.

1. Com o botão direito no nó que pretende remover e, no menu de contexto, selecione **remover**. 

    ![Opções de menu para adicionar uma função de um serviço cloud do Azure](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a>Adicionar novamente uma função a um projeto de serviço cloud do Azure
Se remover uma função de seu projeto de serviço em nuvem, mas posteriormente, decidir adicionar a função de volta ao projeto, apenas a declaração de função e os atributos básicos, tais como pontos finais e informações de diagnóstico, são adicionados. Sem recursos adicionais ou referências são adicionadas para o `ServiceDefinition.csdef` ficheiro ou para o `ServiceConfiguration.cscfg` ficheiro. Se pretender adicionar esta informação, terá de adicioná-la manualmente novamente para esses arquivos.

Por exemplo, poderá remover uma função de serviço da web e posteriormente optar por adicionar esta função de volta para a sua solução. Se fizer isso, ocorrerá um erro. Para evitar este erro, é necessário adicionar os `<LocalResources>` elemento mostrado na seguinte XML para o `ServiceDefinition.csdef` ficheiro. Utilize o nome da função de serviço da web que foi adicionado novamente para o projeto como parte do atributo de nome para o **<LocalStorage>** elemento. Neste exemplo, o nome da função de serviço da web é **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Passos Seguintes
- [Configure as funções para um serviço cloud do Azure com o Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)
