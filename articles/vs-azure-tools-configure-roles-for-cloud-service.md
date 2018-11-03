---
title: Configure as funções para um serviço cloud do Azure com o Visual Studio | Documentos da Microsoft
description: Saiba como definir e configurar funções para serviços cloud do Azure com o Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 4405476a2f006ca8e4e565a8cdd2f6c12e2ed684
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969476"
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Configurar funções do serviço cloud do Azure com o Visual Studio
Um serviço cloud do Azure pode ter um ou mais trabalho, ou funções da web. Para cada função, terá de definir como essa função é configurada e também configurar como essa função é executado. Para saber mais sobre as funções nos serviços cloud, veja o vídeo [introdução aos serviços Cloud do Azure](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

As informações do serviço em nuvem são armazenadas nos seguintes ficheiros:

- **Servicedefinition. Csdef** -o ficheiro de definição de serviço Especifica as definições de tempo de execução para o seu serviço em nuvem, incluindo a quais funções são necessárias, pontos finais e tamanho da máquina virtual. Nenhum dos dados armazenados no `ServiceDefinition.csdef` pode ser alterado quando a sua função é executada.
- **Serviceconfiguration. Cscfg** - o ficheiro de configuração do serviço configura o número de instâncias de uma função é executado e os valores das definições definidas para uma função. Os dados armazenados no `ServiceConfiguration.cscfg` podem ser alteradas durante a execução de sua função.

Para armazenar valores diferentes para as definições que controlam como uma função é executado, pode definir várias configurações de serviço. Pode utilizar uma configuração de serviço diferentes para cada ambiente de implantação. Por exemplo, pode definir a cadeia de ligação de conta de armazenamento para utilizar o emulador de armazenamento do Azure local numa configuração de serviço local e criar outra configuração de serviço para utilizar o armazenamento do Azure na cloud.

Quando cria um serviço cloud do Azure no Visual Studio, duas configurações de serviço são criadas automaticamente e adicionadas ao seu projeto do Azure:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Configurar um serviço cloud do Azure
Pode configurar um serviço cloud do Azure no Explorador de soluções no Visual Studio, conforme mostrado nas etapas a seguir:

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, clique com o botão direito no projeto e, no menu de contexto, selecione **propriedades**.
   
    ![Menu de contexto de projeto do Solution Explorer](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Na página de propriedades do projeto, selecione o **desenvolvimento** separador. 

    ![Página de propriedades do projeto - separador de desenvolvimento](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. Na **configuração do serviço** , selecione o nome da configuração do serviço que pretende editar. (Se desejar fazer alterações em todas as configurações de serviço para esta função, selecione **todas as configurações de**.)
   
    > [!IMPORTANT]
    > Se optar por uma configuração de serviço específico, algumas propriedades estão desativadas porque só pode ser definidos para todas as configurações. Para editar estas propriedades, tem de selecionar **todas as configurações de**.
    > 
    > 
   
    ![Lista de configuração do serviço para um serviço cloud do Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a>Alterar o número de instâncias de função
Para melhorar o desempenho do seu serviço cloud, pode alterar o número de instâncias de uma função em execução, com base no número de utilizadores ou a carga esperada para uma determinada função. É criada uma máquina de virtual separada para cada instância de uma função quando o serviço em nuvem é executado no Azure. Isso afeta a faturação para a implementação deste serviço de nuvem. Para obter mais informações sobre a faturação, consulte [compreender a sua fatura do Microsoft Azure](billing/billing-understand-your-bill.md).

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto. Sob o **funções** nó, com a função que pretende atualizar e, no menu de contexto, selecione o botão direito **propriedades**.

    ![Menu de contexto da função de Explorer Azure de solução](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecione o **configuração** separador.

    ![Separador de configuração](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. Na **configuração do serviço** , selecione a configuração de serviço que pretende atualizar.
   
    ![Lista de configuração do serviço](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. Na **contagem de instâncias** texto, introduza o número de instâncias que pretende iniciar para esta função. Cada instância é executado numa máquina virtual separada, quando publica o serviço em nuvem para o Azure.

    ![A atualizar a contagem de instâncias](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. No Visual Studio, barra de ferramentas, selecione **guardar**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Gerir cadeias de ligação para as contas de armazenamento
Pode adicionar, remover ou modificar cadeias de ligação para as configurações de serviço. Por exemplo, pode pretender obter uma cadeia de ligação de local de uma configuração de serviço local que tem um valor de `UseDevelopmentStorage=true`. Também poderá configurar uma configuração de serviço cloud que utiliza uma conta de armazenamento no Azure.

> [!WARNING]
> Ao introduzir as informações de chave de conta de armazenamento do Azure para uma cadeia de ligação de conta de armazenamento, essas informações são armazenadas localmente no ficheiro de configuração do serviço. No entanto, estas informações não atualmente são armazenadas como texto encriptado.
> 
> 

Ao utilizar um valor diferente para cada configuração de serviço, não é necessário que utilizar cadeias de ligação diferentes no seu serviço cloud ou modifique o código quando publicar o seu serviço cloud no Azure. Pode utilizar o mesmo nome para a cadeia de ligação no seu código e o valor é diferente, consoante a configuração de serviço que selecionar ao criar o seu serviço cloud ou quando o publicar.

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto. Sob o **funções** nó, com a função que pretende atualizar e, no menu de contexto, selecione o botão direito **propriedades**.

    ![Menu de contexto da função de Explorer Azure de solução](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecione o **definições** separador.

    ![Separador Definições](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. Na **configuração do serviço** , selecione a configuração de serviço que pretende atualizar.

    ![Configuração do serviço](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Para adicionar uma cadeia de ligação, selecione **Adicionar definição**.

    ![Adicionar cadeia de ligação](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Depois de adicionada a nova configuração à lista, atualize a linha na lista com as informações necessárias.

    ![Nova cadeia de ligação](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Nome** -introduza o nome que pretende utilizar para a cadeia de ligação.
    - **Tipo** - selecione **cadeia de ligação** na lista pendente.
    - **Valor** -pode optar por introduzir a cadeia de ligação diretamente para o **valor** células ou selecione as reticências (...) para funcionar no **criar cadeia de ligação de armazenamento** caixa de diálogo.  

1. Na **criar cadeia de ligação de armazenamento** caixa de diálogo, selecione uma opção para **estabelecer ligação utilizando**. Em seguida, siga as instruções para a opção de que selecionar:

    - **Emulador de armazenamento do Microsoft Azure** -se selecionar esta opção, as restantes definições na caixa de diálogo são desativadas uma vez que se aplicam apenas para o Azure. Selecione **OK**.
    - **Sua assinatura** - se selecionar esta opção, utilize a lista pendente para selecionar e iniciar sessão numa conta Microsoft ou adicionar uma conta Microsoft. Selecione uma conta de armazenamento e de subscrição do Azure. Selecione **OK**.
    - **Introduzir manualmente credenciais** -introduza o nome da conta de armazenamento e a chave primária ou segundo. Selecione uma opção para **ligação** (HTTPS é recomendado para a maioria dos cenários.) Selecione **OK**.

1. Para eliminar uma cadeia de ligação, selecione a cadeia de ligação e, em seguida, selecione **definição remover**.

1. No Visual Studio, barra de ferramentas, selecione **guardar**.

## <a name="programmatically-access-a-connection-string"></a>Acessar programaticamente uma cadeia de ligação

Os passos seguintes mostram como acessar programaticamente uma cadeia de ligação com c#.

1. Adicione o seguinte usando diretivas num arquivo c# aonde vai para utilizar a definição:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. O código a seguir ilustra um exemplo de como acessar uma cadeia de ligação. Substitua o &lt;ConnectionStringName > espaço reservado com o valor apropriado. 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Adicionar definições personalizadas para utilizar no seu serviço cloud do Azure
Definições personalizadas no arquivo de configuração de serviço permitem-lhe adicionar um nome e valor para uma cadeia de caracteres para uma configuração de serviço específico. Pode optar por utilizar esta definição para configurar uma funcionalidade no seu serviço cloud, ao ler o valor da definição e utilizar este valor para controlar a lógica em seu código. Pode alterar estes valores de configuração do serviço sem precisar recriar seu pacote de serviço ou quando seu serviço cloud está em execução. Seu código pode verificar notificações de quando as alterações de configuração. Ver [RoleEnvironment.Changing eventos](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Pode adicionar, remover ou modificar definições personalizadas para as configurações de serviço. Pode desejar valores diferentes para essas cadeias de caracteres para configurações de serviço diferentes.

Ao utilizar um valor diferente para cada configuração de serviço, não é necessário usar cadeias de caracteres diferentes no seu serviço cloud ou modifique o código quando publicar o seu serviço cloud no Azure. Pode utilizar o mesmo nome para a cadeia de caracteres em seu código e o valor é diferente, consoante a configuração de serviço que selecionar ao criar o seu serviço cloud ou quando o publicar.

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto. Sob o **funções** nó, com a função que pretende atualizar e, no menu de contexto, selecione o botão direito **propriedades**.

    ![Menu de contexto da função de Explorer Azure de solução](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecione o **definições** separador.

    ![Separador Definições](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. Na **configuração do serviço** , selecione a configuração de serviço que pretende atualizar.

    ![Lista de configuração do serviço](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Para adicionar uma definição personalizada, selecione **Adicionar definição**.

    ![Adicionar definição personalizada](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Depois de adicionada a nova configuração à lista, atualize a linha na lista com as informações necessárias.

    ![Nova definição personalizada](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Nome** -introduza o nome da definição.
    - **Tipo** - selecione **cadeia** na lista pendente.
    - **Valor** -introduza o valor da definição. Pode optar por introduzir o valor diretamente para o **valor** da célula ou selecione as reticências (...) para introduzir o valor no **Editar cadeia** caixa de diálogo.  

1. Para eliminar uma definição personalizada, selecione a definição e, em seguida, selecione **definição remover**.

1. No Visual Studio, barra de ferramentas, selecione **guardar**.

## <a name="programmatically-access-a-custom-settings-value"></a>Aceder programaticamente ao valor de uma definição personalizada
 
Os passos seguintes mostram como acessar programaticamente uma definição personalizada usando o c#.

1. Adicione o seguinte usando diretivas num arquivo c# aonde vai para utilizar a definição:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. O código a seguir ilustra um exemplo de como acessar uma definição personalizada. Substitua o &lt;SettingName > espaço reservado com o valor apropriado. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Gerir o armazenamento local para cada instância de função
Pode adicionar armazenamento de sistema de ficheiros local para cada instância de uma função. Os dados armazenados em que o armazenamento não está acessível por outras instâncias da função para que os dados são armazenados ou por outras funções.  

1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio.

1. Na **Explorador de soluções**, expanda o nó do projeto. Sob o **funções** nó, com a função que pretende atualizar e, no menu de contexto, selecione o botão direito **propriedades**.

    ![Menu de contexto da função de Explorer Azure de solução](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecione o **armazenamento Local** separador.

    ![Guia de armazenamento local](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. Na **configuração do serviço** lista, certifique-se de que **todas as configurações de** está selecionado como as definições de armazenamento local são aplicadas a todas as configurações de serviço. Qualquer outro valor resulta em todos os campos de entrada na página a ser desativado. 

    ![Lista de configuração do serviço](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. Para adicionar uma entrada de armazenamento local, selecione **adicionar armazenamento no Local**.

    ![Adicionar armazenamento local](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Assim que a nova entrada de armazenamento local foi adicionada à lista, atualize a linha na lista com as informações necessárias.

    ![Nova entrada de armazenamento local](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Nome** -introduza o nome que pretende utilizar para o novo armazenamento local.
    - **Tamanho (MB)** -introduza o tamanho em MB que sejam necessárias para o novo armazenamento local.
    - **Limpeza na Reciclagem de função** -Selecione esta opção para remover os dados no novo armazenamento local quando a máquina virtual para a função é reciclada.

1. Para eliminar uma entrada de armazenamento local, selecione a entrada e, em seguida, selecione **remover armazenamento Local**.

1. No Visual Studio, barra de ferramentas, selecione **guardar**.

## <a name="programmatically-accessing-local-storage"></a>Por meio de programação ao aceder ao armazenamento local

Esta secção ilustra como acessar programaticamente o armazenamento local usando a linguagem c# ao escrever um arquivo de texto de teste `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-to-local-storage"></a>Gravar um arquivo de texto no armazenamento local

O código seguinte mostra um exemplo de como escrever um arquivo de texto para o armazenamento local. Substitua o &lt;LocalStorageName > espaço reservado com o valor apropriado. 

    ```csharp
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-to-local-storage"></a>Encontrar um ficheiro escrito no armazenamento local

Para ver o ficheiro criado pelo código na seção anterior, siga estes passos:
    
1.  Na área de notificação Windows, faça duplo clique no ícone do Azure e, no menu de contexto, selecione **mostrar IU do emulador de computação**. 

    ![Mostrar o emulador de computação do Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Selecione a função da web.

    ![Emulador de computação do Azure](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. Sobre o **emulador de computação do Microsoft Azure** menu, selecione **ferramentas** > **armazenamento local abra**.

    ![Item de menu abrir arquivo local](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Quando abre a janela do Windows Explorer, introduza "MyLocalStorageTest.txt ' para o **pesquisa** caixa de texto e selecione **Enter** para iniciar a pesquisa. 

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre os projetos do Azure no Visual Studio lendo [configurando um projeto do Azure](vs-azure-tools-configuring-an-azure-project.md). Saiba mais sobre o esquema do serviço cloud lendo [referência de esquema](https://msdn.microsoft.com/library/azure/dd179398).

