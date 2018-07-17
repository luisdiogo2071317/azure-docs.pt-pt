---
title: Utilizar uma Identidade Gerida de VM do Windows para aceder ao Armazenamento do Azure
description: Um tutorial que explica o processo de utilização de uma Identidade Gerida de VM do Windows para aceder ao Armazenamento do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: a9b0ddbd8d0a348d00e57526e6d248e59a8a1e79
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903962"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Tutorial: Utilizar uma Identidade Gerida de VM do Windows para aceder ao Armazenamento do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como ativar a Identidade Gerida para uma Máquina Virtual do Windows e utilizar essa identidade para aceder ao Armazenamento do Azure.  Saiba como:

> [!div class="checklist"]
> * Criar uma máquina virtual do Windows num novo grupo de recursos 
> * Ativar a Identidade Gerida numa Máquina Virtual (VM) do Windows
> * Criar um contentor de blobs numa conta de armazenamento
> * Conceder o acesso da Identidade Gerida da VM do Windows a uma conta de armazenamento 
> * Obter um token de acesso e utilizá-lo para chamar o Armazenamento do Azure 

> [!NOTE]
> A autenticação do Active Directory do Azure para o Armazenamento do Azure está em pré-visualização pública.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Nesta secção, vai criar uma VM do Windows à qual será concedida mais tarde uma Identidade Gerida.

1.  Clique no botão **+/Criar novo serviço**, no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4.  Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **Grupo de Recursos** no qual gostaria que a máquina virtual fosse criada, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>Ativar a Identidade Gerida na sua VM

Uma Identidade Gerida de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem ter de colocar as credenciais no código. Nos bastidores, ativar a Identidade Gerida numa Máquina Virtual através do portal do Azure faz duas coisas: regista a sua VM no Azure AD para criar uma identidade gerida e configura a identidade na VM. 

1. Navegue para o grupo de recursos da sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Na categoria **Definições**, clique em **Configuração**.
3. Para ativar a Identidade Gerida, selecione **Sim**.
4. Clique em **Guardar** para aplicar a configuração. 

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Nesta secção, vai criar uma conta de armazenamento. 

1. Clique no botão **+ Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Armazenamento** e, em seguida, **Conta de armazenamento – blob, ficheiro, tabela, fila**.
3. Em **Nome**, introduza um nome para a conta de armazenamento.  
4. O **Modelo de implementação** e o **Tipo de conta** devem ser definidos como **Gestor de recursos** e **Armazenamento (fins gerais v1)**. 
5. Certifique-se de que a **Subscrição** e o **Grupo de Recursos** correspondem aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Criar um contentor de blobs e carregar um ficheiro para a conta de armazenamento

Os ficheiros requerem armazenamento de blobs, por isso tem de criar um contentor de blobs para armazenar o ficheiro. Em seguida, carregue um ficheiro para o contentor de blobs na nova conta de armazenamento.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Em **Serviço Blob**, clique em **Contentores**.
3. Clique em **+ Contentor** na parte superior da página.
4. Em **Novo contentor**, introduza um nome para o contentor e, em **Nível de acesso público**, mantenha o valor predefinido.

    ![Criar contentor de armazenamento](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Com um editor à sua escolha, crie um ficheiro intitulado *hello world.txt* no seu computador local.  Abra o ficheiro e adicione o texto (sem as aspas) "Hello world! :) "e, em seguida, guarde-o. 
6. Carregue o ficheiro para o contentor recentemente criado ao clicar no nome do contentor e, em seguida, em **Carregar**
7. No painel **Carregar blob**, em **Ficheiros**, clique no ícone de pasta e procure o ficheiro **hello_world.txt** no seu computador local, selecione o ficheiro e, em seguida, clique em **Carregar**.
    ![Carregar ficheiro de texto](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Conceder o acesso da VM a um contentor do Armazenamento do Microsoft Azure 

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

Segue-se um exemplo de código .Net de abrir uma ligação ao Armazenamento do Azure com um token de acesso e, em seguida, ler o conteúdo do ficheiro que criou anteriormente. Este código deve ser executado na VM para poder aceder ao ponto final da Identidade Gerida da VM. É necessário o .NET Framework 4.6 ou superior para utilizar o método de token de acesso. Substitua o valor de `<URI to blob file>` em conformidade. Pode obter este valor ao navegar para o ficheiro que criou e carregou para o armazenamento de blobs e copiar o **URL** em **Propriedades** para a página **Descrição Geral**.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");
           
            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);
        
            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }            
    }
}
```

A resposta inclui o conteúdo do ficheiro:

`Hello world! :)`

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a ativar uma Identidade Gerida de máquina virtual do Windows para aceder ao Armazenamento do Azure.  Para saber mais sobre o Armazenamento do Azure, veja:

> [!div class="nextstepaction"]
> [Armazenamento do Azure](/azure/storage/common/storage-introduction)



