---
title: Utilizar uma Identidade Gerida de VM do Linux para aceder ao Armazenamento do Azure
description: Um tutorial que explica o processo de utilização de uma Identidade Gerida Atribuída ao Sistema numa VM do Linux, para aceder ao Armazenamento do Azure.
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
ms.date: 04/09/2018
ms.author: daveba
ms.openlocfilehash: d4daccfdcb2bc11831e960aa20533e32801db946
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049342"
---
# <a name="tutorial-use-a-linux-vms-managed-identity-to-access-azure-storage"></a>Tutorial: utilizar uma Identidade Gerida de VM do Linux para aceder ao Armazenamento do Azure 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Este tutorial mostra-lhe como criar e utilizar uma Identidade Gerida de VM do Linux para aceder ao Armazenamento do Azure. Saiba como:

> [!div class="checklist"]
> * Criar uma máquina virtual do Linux num novo grupo de recursos
> * Ativar a Identidade Gerida numa Máquina Virtual (VM) do Linux
> * Criar um contentor de blobs numa conta de armazenamento
> * Conceder acesso à Identidade Gerida de VM do Linux a um contentor de Armazenamento do Azure
> * Obter um token de acesso e utilizá-lo para chamar o Armazenamento do Azure

> [!NOTE]
> A autenticação do Azure Active Directory para o Armazenamento do Azure está em pré-visualização pública.

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com) antes de continuar.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Para executar os exemplos de script da CLI neste tutorial, tem duas opções:

- Utilizar o [Azure Cloud Shell](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão **Experimentar**, localizado no canto superior direito de cada bloco de código.
- [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior), se preferir utilizar uma consola CLI local.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Linux num novo grupo de recursos

Nesta secção, vai criar uma VM do Linux à qual será concedida mais tarde uma Identidade Gerida.

1. Selecione o botão **Novo**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Em **Tipo de autenticação**, selecione **Chave SSH pública** ou **Palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão na VM.

   ![Painel "Básico" para a criação de uma máquina virtual](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Na lista **Subscrição**, selecione uma subscrição para a máquina virtual.
5. Para selecionar um novo grupo de recursos no qual pretende que a máquina virtual seja criada, selecione **Grupo de recursos** > **Criar novo**. Quando terminar, selecione **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e selecione **OK**.

## <a name="enable-managed-identity-on-your-vm"></a>Ativar a Identidade Gerida na sua VM

Uma Identidade Gerida de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. Nos bastidores, ativar a Identidade Gerida numa Máquina Virtual através do portal do Azure faz duas coisas: regista a sua VM no Azure AD para criar uma identidade gerida e configura a identidade na VM.

1. Navegue para o grupo de recursos da sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Na categoria **Definições**, clique em **Configuração**.
3. Para ativar a Identidade Gerida, selecione **Sim**.
4. Clique em **Guardar** para aplicar a configuração. 

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Nesta secção, vai criar uma conta de armazenamento. 

1. Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Armazenamento** e, em seguida, **Conta de armazenamento – blob, ficheiro, tabela, fila**.
3. Em **Nome**, introduza um nome para a conta de armazenamento.  
4. O **Modelo de implementação** e o **Tipo de conta** devem ser definidos como **Gestor de recursos** e **Armazenamento (fins gerais v1)**. 
5. Certifique-se de que a **Subscrição** e o **Grupo de Recursos** correspondem aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Criar um contentor de blobs e carregar um ficheiro para a conta de armazenamento

Os ficheiros requerem armazenamento de blobs, por isso tem de criar um contentor de blobs para armazenar o ficheiro. Em seguida, carregue um ficheiro para o contentor de blobs na nova conta de armazenamento.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Em **Serviço Blob**, clique em **Contentores**.
3. Clique em **+ Contentor** na parte superior da página.
4. Em **Novo contentor**, introduza um nome para o contentor e, em **Nível de acesso público**, mantenha o valor predefinido.

    ![Criar contentor de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Com um editor à sua escolha, crie um ficheiro com o nome *hello world.txt* no seu computador local.  Abra o ficheiro e adicione o texto (sem as aspas) "Hello world! :) "e, em seguida, guarde-o. 

6. Carregue o ficheiro para o contentor recentemente criado ao clicar no nome do contentor e, em seguida, em **Carregar**
7. No painel **Carregar blob**, em **Ficheiros**, clique no ícone de pasta e procure o ficheiro **hello_world.txt** no seu computador local, selecione o ficheiro e, em seguida, clique em **Carregar**.

    ![Carregar ficheiro de texto](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Conceder à VM o acesso a um contentor do Armazenamento do Azure 

Pode utilizar a identidade gerida da VM para obter os dados no blob de armazenamento do Azure.   

1. Navegue de volta para a sua conta de armazenamento recentemente criada.  
2. Clique na ligação **Controlo de acesso (IAM)** no painel esquerdo.  
3. Clique em **+ Adicionar** na parte superior da página para adicionar uma nova atribuição de função à sua VM.
4. Em **Função**, na lista pendente, selecione **Leitor de Dados do Armazenamento de Blobs (Pré-visualização)**. 
5. Na lista pendente seguinte, em **Atribuir acesso a**, selecione **Máquina Virtual**.  
6. Em seguida, certifique-se de que a subscrição adequada está listada na lista pendente **Subscrição** e, em seguida, defina **Grupo de Recursos** para **Todos os grupos de recursos**.  
7. Em **Selecionar**, selecione a VM e, em seguida, clique em **Guardar**.

    ![Atribuir permissões](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Obter um token de acesso e utilizá-lo para chamar o Armazenamento do Azure

O Armazenamento do Azure suporta nativamente a autenticação do Azure AD, para que possa aceitar diretamente tokens de acesso obtidos com uma Identidade Gerida. Isto faz parte da integração do Armazenamento do Azure no Azure AD e é diferente de fornecer as credenciais na cadeia de ligação.

Para concluir os passos seguintes, precisa de trabalhar a partir da VM que criou anteriormente e é necessário um cliente SSH para ligar à mesma. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [Subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda para configurar as chaves do seu cliente SSH, veja [Como utilizar chaves SSH com o Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) ou [Como criar e utilizar um par de chaves SSH públicas e privadas para VMs do Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Linux e, em seguida, na página **Descrição Geral**, clique em **Ligar**. Copie a cadeia de ligação para ligar à sua VM.
2. **Ligue** à VM com o cliente SSH que escolher. 
3. Na janela de terminal, com o CURL, faça um pedido ao ponto final da Identidade Gerida local para obter um token de acesso para o Armazenamento do Azure.
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true
    ```
4. Utilize o token de acesso para aceder ao Armazenamento do Azure, por exemplo, para ler o conteúdo do ficheiro de exemplo que carregou anteriormente para o contentor. Substitua os valores `<STORAGE ACCOUNT>`, `<CONTAINER NAME>` e `<FILE NAME>` pelos valores que especificou anteriormente, e `<ACCESS TOKEN>` pelo token devolvido no passo anterior.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   A resposta inclui o conteúdo do ficheiro:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a ativar uma Identidade Gerida de máquina virtual do Linux para aceder ao Armazenamento do Azure.  Para saber mais sobre o Armazenamento do Azure, veja:

> [!div class="nextstepaction"]
> [Armazenamento do Azure](/azure/storage/common/storage-introduction)
