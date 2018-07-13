---
title: Como utilizar um Windows VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store
description: Um tutorial que mostra-lhe como utilizar um Windows VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: daef85164793dd6183c41604f200864aabadf8d8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610886"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Utilizar um Windows VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra-lhe como utilizar uma identidade de serviço gerida (MSI) para uma máquina virtual de Windows (VM) para aceder a um Store do Azure Data Lake. Identidades de serviço geridas são geridas automaticamente pelo Azure e ativar a autenticação no serviços que suportam a autenticação do Azure AD, sem a necessidade de inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar o MSI num VM do Windows 
> * Conceder o acesso à sua VM a um Store do Azure Data Lake
> * Obter um token de acesso com a identidade da VM e utilizá-lo para aceder a um Store do Azure Data Lake

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows.  Também pode ativar o MSI numa VM existente.

1. Clique em **criar um recurso** no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3. Introduza as informações da máquina virtual. O **nome de utilizador** e **palavra-passe** criado, Eis aqui as credenciais que utiliza para início de sessão para a máquina virtual.
4. Selecione o elemento adequado **subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **grupo de recursos** no qual criar a sua máquina virtual, escolha **criar nova**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM 

Um MSI de VM permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Ativar o MSI informa o Azure para criar uma identidade gerida para a sua VM. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI no Azure Resource Manager.

1. Selecione o **Máquina Virtual** que pretende ativar o MSI em.  
2. Na barra de navegação esquerdo, clique em **configuração**. 
3. Verá **identidade do serviço gerido**. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não. 
4. Certifique-se de que clica **guardar** para guardar a configuração.  
   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar e certifique-se de que extensões estão nesta VM, clique em **extensões**. Se estiver ativado MSI, em seguida, **ManagedIdentityExtensionforWindows** aparece na lista.

   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Conceder o acesso VM ao Azure Data Lake Store

Agora pode conceder o acesso VM para ficheiros e pastas num Store do Azure Data Lake.  Para este passo, pode utilizar um Store de Lake de dados existente ou crie um novo.  Para criar um novo Store de Lake de dados com o portal do Azure, siga isso [guia de início rápido do Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Também existem inícios rápidos que utilizam a CLI do Azure e o Azure PowerShell no [documentação do Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-overview.md).

No seu Store de Lake de dados, crie uma nova pasta e conceder permissão de MSI de VM para ler, escrever e executar ficheiros nessa pasta:

1. No portal do Azure, clique em **Data Lake Store** no painel de navegação esquerdo.
2. Clique a Store de Lake de dados que pretende utilizar para este tutorial.
3. Clique em **Data Explorer** na barra de comandos.
4. A pasta raiz do Store de Lake dados está selecionada.  Clique em **acesso** na barra de comandos.
5. Clique em **Adicionar**.  Na **selecionar** campo, introduza o nome da sua VM, por exemplo **DevTestVM**.  Clique para selecionar a sua VM resultados da pesquisa, em seguida, clique em **selecione**.
6. Clique em **selecionar permissões**.  Selecione **leitura** e **Execute**, adicione ao **esta pasta**e adicionar como **apenas uma permissão de acesso**.  Clique em **OK**.  A permissão deve ser adicionada com êxito.
7. Fechar o **acesso** painel.
8. Neste tutorial, crie uma nova pasta.  Clique em **nova pasta** na barra de comandos e dar a nova pasta um nome, por exemplo **TestFolder**.  Clique em **OK**.
9. Clique na pasta que criou, em seguida, clique em **acesso** na barra de comandos.
10. Semelhante para o passo 5, clique em **Add**, na **selecione** campo introduza o nome da sua VM, selecione-o e clique em **selecionar**.
11. Semelhante ao passo 6, clique em **selecionar permissões**, selecione **leitura**, **escrever**, e **Execute**, adicionar a **esta pasta**e adicionar como **uma entrada de permissão de acesso e uma entrada de permissão predefinida**.  Clique em **OK**.  A permissão deve ser adicionada com êxito.

O MSI de VM pode agora realizar todas as operações em ficheiros na pasta que criou.  Para obter mais informações sobre como gerir o acesso ao Data Lake Store, leia este artigo sobre [controlo de acesso no Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Obter um token de acesso com o MSI de VM e utilizá-la para chamar o sistema de ficheiros do Azure Data Lake Store

Azure Data Lake Store nativamente suporte do Azure AD authentication, para que possa aceitar tokens de acesso diretamente obtidos com o MSI.  Para autenticar para o sistema de ficheiros do Data Lake Store, envia um token de acesso emitido pelo Azure AD para o endpoint de sistema de ficheiros do Data Lake Store, num cabeçalho de autorização no formato "Bearer < ACCESS_TOKEN_VALUE >".  Para saber mais sobre o suporte de Data Lake Store para autenticação do Azure AD, leia [autenticação com o Data Lake Store com o Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

> [!NOTE]
> Os SDKs do cliente de sistema de ficheiros de Data Lake Store ainda não suportam a identidade do serviço gerido.  Este tutorial será atualizado quando é adicionado suporte para o SDK.

Neste tutorial, autenticar para o sistema de ficheiros do Data Lake Store pedidos da API de REST com o PowerShell para fazer o REST. Para utilizar o MSI de VM para a autenticação, terá de fazer as solicitações da VM.

1. No portal, navegue para **máquinas virtuais**, aceda à sua VM do Windows e no **descrição geral** clique **Connect**.
2. Introduza no seu **nome de utilizador** e **palavra-passe** para que adicionou ao criar a VM do Windows. 
3. Agora que já criou um **conexão de área de trabalho remoto** com a máquina virtual, abra **PowerShell** na sessão remota. 
4. Através do PowerShell `Invoke-WebRequest`, fazer um pedido para o ponto de final de MSI local para obter um acesso de token para o Azure Data Lake Store.  O identificador de recurso do Data Lake Store é "https://datalake.azure.net/".  Data Lake faz uma correspondência exata no identificador do recurso e a barra é importante.

   ```powershell
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://datalake.azure.net/"} -Headers @{Metadata="true"}
   ```
    
   Converta a resposta de um objeto JSON para um objeto do PowerShell. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extraia o token de acesso da resposta.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. Através "Invoke-WebRequest' do PowerShell, fazer um pedido ao ponto final REST do Data Lake Store para listar as pastas na pasta raiz.  Esta é uma forma simple de verificar a tudo o que está configurado corretamente.  É importante que a cadeia de caracteres "Bearer" no cabeçalho de autorização possui uma capital "B".  Pode encontrar o nome do seu Store de Lake de dados no **descrição geral** seção do painel no portal do Azure Data Lake Store.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Uma resposta com êxito é semelhante a:

   ```powershell
   StatusCode        : 200
   StatusDescription : OK
   Content           : {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY", "blockSize":0,"accessTime":1507934941392, "modificationTime":1507944835699,"replication":0, "permission":"770","ow..."
   RawContent        : HTTP/1.1 200 OK
                       Pragma: no-cache
                       x-ms-request-id: b4b31e16-e968-46a1-879a-3474aa7d4528
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict-Transport-Security: ma...
   Forms             : {}
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b4b31e16-e968-46a1-879a-3474aa7d4528],
                       [x-ms-webhdfs-version, 17.04.22.00], [Status, 0x0]...}
   Images            : {}
   InputFields       : {}
   Links             : {}
   ParsedHtml        : System.__ComObject
   RawContentLength  : 556
   ```

6. Agora pode tentar carregar um ficheiro para o Store de Lake de dados.  Primeiro, crie um ficheiro a carregar.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Através do PowerShell `Invoke-WebRequest`, fazer um pedido ao ponto final REST do Data Lake Store para carregar o ficheiro para a pasta que criou anteriormente.  Este pedido necessárias duas etapas.  A primeira etapa, fazer um pedido e obtenha um redirecionamento para onde o ficheiro deve ser carregado.  Na segunda etapa, na verdade, carregue o ficheiro.  Não se esqueça de definir o nome da pasta e ficheiro adequadamente, se utilizou os valores diferentes, que neste tutorial. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Se inspecionar o valor de `$HdfsRedirectResponse` deve ser semelhante a seguinte resposta:

   ```powershell
   PS C:\> $HdfsRedirectResponse

   StatusCode        : 307
   StatusDescription : Temporary Redirect
   Content           : {}
   RawContent        : HTTP/1.1 307 Temporary Redirect
                       Pragma: no-cache
                       x-ms-request-id: b7ab492f-b514-4483-aada-4aa0611d12b3
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosn...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b7ab492f-b514-4483-aada-4aa0611d12b3], 
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

   Conclua o carregamento ao enviar um pedido para o ponto final de redirecionamento:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Aspeto de resposta com êxito:

   ```powershell
   StatusCode        : 201
   StatusDescription : Created
   Content           : {}
   RawContent        : HTTP/1.1 201 Created
                       Pragma: no-cache
                       x-ms-request-id: 1e70f36f-ead1-4566-acfa-d0c3ec1e2307
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, 1e70f36f-ead1-4566-acfa-d0c3ec1e2307],
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

Utilizar outro sistema de ficheiros do Data Lake Store APIs, pode acrescentar a ficheiros, transferi os ficheiros e muito mais.

Parabéns!  Autenticado para o sistema de ficheiros do Data Lake Store com um MSI de VM.

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Para a gestão de operações Data Lake Store utiliza o Azure Resource Manager.  Para obter mais informações sobre como utilizar um MSI de VM para autenticar para o Resource Manager, leia [usar um Linux VM Managed Service Identity (MSI) para acessar o Resource Manager](../managed-service-identity/msi-tutorial-linux-vm-access-arm.md).
- Saiba mais sobre [autenticação com o Data Lake Store com o Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Saiba mais sobre [operações de sistema de ficheiros no Azure Data Lake Store com a REST API](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) ou o [APIs de sistema de ficheiros do WebHDFS](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Saiba mais sobre [controlo de acesso no Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.