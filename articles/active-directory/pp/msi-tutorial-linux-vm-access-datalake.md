---
title: Como utilizar um Linux VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store
description: Um tutorial que mostra-lhe como utilizar um Linux VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store.
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
ms.openlocfilehash: 358827722e8d77cd91410fae842ad2ba99967d98
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610359"
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Utilize um Linux VM Managed Service Identity (MSI) para aceder ao Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra-lhe como utilizar uma identidade de serviço gerida (MSI) para uma máquina virtual (VM) do Linux para aceder a um Store do Azure Data Lake. Identidades de serviço geridas são geridas automaticamente pelo Azure e ativar a autenticação no serviços que suportam a autenticação do Azure AD, sem a necessidade de inserir as credenciais no seu código. Saiba como:

> [!div class="checklist"]
> * Ativar o MSI numa VM do Linux 
> * Conceder o acesso à sua VM a um Store do Azure Data Lake
> * Obter um token de acesso com a identidade da VM e utilizá-lo para aceder a um Store do Azure Data Lake

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma Máquina Virtual Linux num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Linux. Também pode ativar o MSI numa VM existente.

1. Clique em **criar um recurso** no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Para **tipo de autenticação**, selecione **chave pública SSH** ou **palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão VM.

   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolher uma **subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **grupo de recursos** pretende que a máquina virtual para ser criado no, escolha **criar nova**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **ver todos os** ou alterar o filtro de tipo de disco suportados. No painel de definições, mantenha as predefinições e clique em **OK**.

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM

Um MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a MSI no Azure Resource Manager.  

1. Selecione o **Máquina Virtual** que pretende ativar o MSI em.
2. Na barra de navegação esquerdo, clique em **configuração**.
3. Verá **identidade do serviço gerido**. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não.
4. Certifique-se de que clica **guardar** para guardar a configuração.

   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se desejar verificar que extensões estão nisso **VM do Linux**, clique em **extensões**. Se o MSI é ativado, o **ManagedIdentityExtensionforLinux** aparece na lista.

   ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

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

O MSI de VM pode agora realizar todas as operações em ficheiros na pasta que criou.  Para obter mais informações sobre como gerir o acesso ao Data Lake Store, leia este artigo sobre [controlo de acesso no Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Obter um token de acesso com o MSI de VM e utilizá-la para chamar o sistema de ficheiros do Azure Data Lake Store

Azure Data Lake Store nativamente suporte do Azure AD authentication, para que possa aceitar tokens de acesso diretamente obtidos com o MSI.  Para autenticar para o sistema de ficheiros do Data Lake Store que enviar um token de acesso emitido pelo Azure AD para o endpoint de sistema de ficheiros do Data Lake Store, num cabeçalho de autorização no formato "portador \<ACCESS_TOKEN_VALUE\>".  Para saber mais sobre o suporte de Data Lake Store para autenticação do Azure AD, leia [autenticação com o Data Lake Store com o Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

Neste tutorial, autenticar para o sistema de ficheiros do Data Lake Store pedidos da API de REST com o CURL para fazer o REST.

> [!NOTE]
> Os SDKs do cliente de sistema de ficheiros de Data Lake Store ainda não suportam a identidade do serviço gerido.  Este tutorial será atualizado quando é adicionado suporte para o SDK.

Para concluir estes passos, terá de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda a configurar chaves de seu cliente SSH, veja [como utilizar chaves SSH com Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs do Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal, navegue para a VM do Linux e, no **descrição geral**, clique em **Connect**.  
2. **Ligar** para a VM com o cliente SSH à sua escolha. 
3. Na janela de terminal, com o CURL, fazer um pedido para o ponto de final de MSI local para obter um acesso de token para o sistema de ficheiros do Data Lake Store.  O identificador de recurso do Data Lake Store é "https://datalake.azure.net/."  É importante incluir a barra no identificador de recurso.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Uma resposta com êxito, devolve o token de acesso que é utilizada para autenticar para o Data Lake Store:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Com o CURL, fazer um pedido para o ponto final REST de sistema de ficheiros Data Lake Store para listar as pastas na pasta raiz.  Esta é uma forma simple de verificar a tudo o que está configurado corretamente.  Copie o valor do token de acesso do passo anterior.  É importante que a cadeia de caracteres "Bearer" no cabeçalho de autorização possui uma capital "B".  Pode encontrar o nome do seu Store de Lake de dados no **descrição geral** seção do painel no portal do Azure Data Lake Store.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Uma resposta com êxito é semelhante a:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Agora pode tentar carregar um ficheiro para o Store de Lake de dados.  Primeiro, crie um ficheiro a carregar.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Com o CURL, fazer um pedido para o ponto final REST de sistema de ficheiros Data Lake Store para carregar o ficheiro para a pasta que criou anteriormente.  O carregamento envolve um redirecionamento, e CURL segue o redirecionamento automaticamente. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Uma resposta com êxito é semelhante a:

   ```bash
   HTTP/1.1 100 Continue
   HTTP/1.1 307 Temporary Redirect
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: 756f6b24-0cca-47ef-aa12-52c3b45b954c
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
       
   HTTP/1.1 100 Continue
       
   HTTP/1.1 201 Created
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: af5baa07-3c79-43af-a01a-71d63d53e6c4
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
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