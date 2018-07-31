---
title: Como utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Data Lake Store
description: Um tutorial que lhe mostra como utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Data Lake Store.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: f5d4a5e26ecf4bde286a5163bf5ec7da492e474d
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247918"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-data-lake-store"></a>Tutorial: Utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Data Lake Store

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra-lhe como utilizar uma Identidade de Serviço Gerida para que uma máquina virtual (VM) do Windows aceda ao Azure Data Lake Store. As Identidades de Serviço Geridas são geridas automaticamente pelo Azure e permitem-lhe fazer a autenticação em serviços que suportam a autenticação do Azure AD, sem ser necessário inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar a Identidade de Serviço Gerida numa VM do Windows 
> * Conceder o acesso à sua VM ao Azure Data Lake Store
> * Obter um token de acesso com a identidade da VM e utilizá-lo para aceder ao Azure Data Lake Store

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Neste tutorial, vamos criar uma nova VM do Windows.  Também pode ativar a Identidade de Serviço Gerida numa VM existente.

1. Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3. Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4. Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **Grupo de Recursos** no qual irá criar a sua máquina virtual, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

   ![Texto alternativo da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Ativar a Identidade de Serviço Gerida na sua VM 

Uma Identidade de Serviço Gerida de VM permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. A ativação da Identidade de Serviço Gerida informa o Azure para criar uma identidade gerida para a sua VM. Em segundo plano, a ativação da Identidade de Serviço Gerida faz duas coisas: regista a sua VM no Azure Active Directory para criar a respetiva identidade gerida e configura a identidade na VM.

1. Selecione a **Máquina Virtual** na qual quer ativar a Identidade de Serviço Gerida.  
2. Na barra de navegação esquerda, clique em **Configuração**. 
3. Vai ver a **Identidade de Serviço Gerida**. Para registar e ativar a Identidade de Serviço Gerida, selecione **Sim**; se desejar desativá-la, selecione Não. 
4. Certifique-se de que clica em **Guardar** para guardar a configuração.  
   ![Texto alternativo da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar as extensões que estão nesta VM, clique em **Extensões**. Se a Identidade de Serviço Gerida estiver ativada, **ManagedIdentityExtensionforWindows** aparece na lista.

   ![Texto alternativo da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Conceder acesso à VM ao Azure Data Lake Store

Agora, pode conceder o acesso à VM a ficheiros e pastas no Azure Data Lake Store.  Para este passo, pode utilizar um Data Lake Store existente ou criar um novo.  Para criar um novo Data Lake Store com o portal do Azure, siga este [Guia de início rápido do Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Também existem inícios rápidos que utilizam a CLI do Azure e o Azure PowerShell na [Documentação do Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview).

No seu Data Lake Store, crie uma nova pasta e conceda à Identidade de Serviço Gerida da VM permissão para ler, escrever e executar ficheiros nessa pasta:

1. No portal do Azure, clique em **Data Lake Store** no painel de navegação esquerdo.
2. Clique no Data Lake Store que pretende utilizar neste tutorial.
3. Clique em **Data Explorer** na barra de comandos.
4. A pasta raiz do Data Lake Store fica selecionada.  Clique em **Acesso** na barra de comandos.
5. Clique em **Adicionar**.  No campo **Selecionar**, introduza o nome da sua VM, por exemplo **DevTestVM**.  Clique para selecionar a sua VM nos resultados da pesquisa e clique em **Selecionar**.
6. Clique em **Selecionar Permissões**.  Selecione **Ler** e **Executar**, adicione a **Esta pasta**e adicione como **Apenas uma permissão de acesso**.  Clique em **OK**.  A permissão deve ser adicionada com êxito.
7. Feche o painel **Acesso**.
8. Neste tutorial, vai criar uma nova pasta.  Clique em **Nova Pasta** na barra de comandos e dê um nome à pasta, por exemplo **TestFolder**.  Clique em **OK**.
9. Clique na pasta que criou e clique em **Acesso** na barra de comandos.
10. Como no passo 5, clique em **Adicionar**, no campo **Selecionar** introduza o nome da sua VM, selecione-a e clique em **Selecionar**.
11. Como no passo 6, clique em **Selecionar Permissões**, selecione **Ler**, **Escrever** e **Executar**, adicione a **Esta pasta** e adicione como **Uma entrada de permissão de acesso e uma entrada de permissão predefinida**.  Clique em **OK**.  A permissão deve ser adicionada com êxito.

A Identidade de Serviço Gerida da VM pode agora realizar todas as operações nos ficheiros da pasta que criou.  Para obter mais informações sobre como gerir o acesso ao Data Lake Store, leia este artigo sobre [Controlo de Acesso no Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-managed-service-identity-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Obter um token de acesso com a Identidade de Serviço Gerida da VM e utilizá-lo para chamar o sistema de ficheiros do Azure Data Lake Store

O Azure Data Lake Store suporta nativamente a autenticação do Azure AD, para que possa aceitar diretamente tokens de acesso obtidos com a Identidade de Serviço Gerida.  Para autenticar para o sistema de ficheiros do Data Lake Store, envia um token de acesso emitido pelo Azure AD para o ponto final do sistema de ficheiros do Data Lake Store, num cabeçalho de autorização no formato "Bearer <ACCESS_TOKEN_VALUE>".  Para saber mais sobre o suporte do Data Lake Store para a autenticação do Azure AD, leia [Autenticação com o Data Lake Store através do Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory)

> [!NOTE]
> Os SDKs cliente do sistema de ficheiros do Data Lake Store ainda não suportam a Identidade de Serviço Gerida.  Este tutorial será atualizado quando for adicionado suporte para o SDK.

Neste tutorial, vai autenticar para a API REST do sistema de ficheiros do Data Lake Store com o PowerShell, para fazer pedidos REST. Para utilizar a Identidade de Serviço Gerida da VM para a autenticação, terá de fazer os pedidos a partir da VM.

1. No portal, navegue para **Máquinas Virtuais**, aceda à sua VM do Windows e, na **Descrição Geral** clique em **Ligar**.
2. Introduza o seu **Nome de Utilizador** e a **Palavra-passe** que adicionou quando criou a VM do Windows. 
3. Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o **PowerShell** na sessão remota. 
4. Através de `Invoke-WebRequest` do PowerShell, faça um pedido ao ponto final da Identidade de Serviço Gerida local para obter um token de acesso para o Azure Data Lake Store.  O identificador de recurso do Data Lake Store é "https://datalake.azure.net/".  O Data Lake faz uma correspondência exata no identificador de recurso e a barra à direita é importante.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatalake.azure.net%2F' -Method GET -Headers @{Metadata="true"}
   ```
    
   Converta a resposta de um objeto JSON num objeto do PowerShell. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extraia o token de acesso da resposta.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. Através de “Invoke-WebRequest” do PowerShell, faça um pedido ao ponto final REST do Data Lake Store para listar as pastas na pasta raiz.  Esta é uma forma simples de verificar se tudo está configurado corretamente.  É importante que a cadeia "Bearer" no cabeçalho de autorização possua um "B" maiúsculo.  Pode encontrar o nome do seu Data Lake Store na secção **Descrição Geral** do painel Data Lake Store no portal do Azure.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Uma resposta com êxito é semelhante à seguinte:

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

6. Agora, pode tentar carregar um ficheiro para o Data Lake Store.  Primeiro, crie um ficheiro para carregar.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Através de `Invoke-WebRequest` do PowerShell, faça um pedido ao ponto final REST do Data Lake Store para carregar o ficheiro para a pasta que criou anteriormente.  Este pedido é feito em dois passos.  No primeiro passo, faz o pedido e obtém um redirecionamento para onde o ficheiro deve ser carregado.  No segundo, carrega o ficheiro.  Não se esqueça de definir o nome da pasta e do ficheiro adequadamente, se tiver utilizado valores diferentes aos deste tutorial. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Se inspecionar o valor de `$HdfsRedirectResponse`, deve ser semelhante à seguinte resposta:

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

   Conclua o carregamento, enviando um pedido para o ponto final de redirecionamento:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Uma resposta com êxito é semelhante à seguinte:

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

Ao utilizar outras APIs do sistema de ficheiros do Data Lake Store pode anexar a ficheiros, transferir ficheiros, etc.

Parabéns!  Autenticou para o sistema de ficheiros do Data Lake Store com uma Identidade de Serviço Gerida de VM.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar uma Identidade de Serviço Gerida para uma máquina virtual do Windows aceder ao Azure Data Lake Store. Para saber mais sobre o Azure Data Lake Store, veja:

> [!div class="nextstepaction"]
>[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview)
