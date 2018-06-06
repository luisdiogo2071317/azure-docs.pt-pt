---
title: Utilizar uma identidade de geridos de VM do Windows para aceder ao armazenamento do Azure
description: Um tutorial que explica o processo de utilizar uma identidade de geridos de VM do Windows para aceder ao armazenamento do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: 9ccc94727a18fbcd77f00000531934be57b8e132
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801369"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Tutorial: Utilizar uma identidade de geridos de VM do Windows para aceder ao armazenamento do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como ativar a identidade de gerido para uma Máquina Virtual do Windows e utilizar essa identidade para aceder ao armazenamento do Azure.  Saiba como:

> [!div class="checklist"]
> * Criar uma máquina virtual do Windows num novo grupo de recursos 
> * Ativar gerida identidade de uma Máquina Virtual (VM) do Windows
> * Criar um contentor de BLOBs numa conta do storage
> * Conceder acesso de geridos identidade da VM do Windows para uma conta de armazenamento 
> * Obter um acesso e utilizá-la para chamar o Storage do Azure 

> [!NOTE]
> Autenticação do Active Directory do Azure para armazenamento do Azure está em pré-visualização pública.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Nesta secção, vai criar uma VM do Windows que mais tarde é concedida uma identidade geridos.

1.  Clique em de **+ /Safari/Chrome criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Introduza as informações da máquina virtual. O **Username** e **palavra-passe** criada aqui é as credenciais que utiliza para início de sessão para a máquina virtual.
4.  Escolha o adequado **subscrição** para a máquina virtual na lista pendente.
5.  Para selecionar um novo **grupo de recursos** gostaria que a máquina virtual para ser criado no, escolha **criar novo**. Quando terminar, clique em **OK**.
6.  Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. No painel de definições, mantenha as predefinições e clique em **OK**.

    ![Texto alternativo da imagem](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>Ativar a identidade gerida na sua VM

Uma identidade da Máquina Virtual gerida permite-lhe obter os tokens de acesso do Azure AD sem necessidade de colocar as credenciais para o seu código. Nos bastidores, ativar a identidade gerido numa máquina Virtual através do portal do Azure duas coisas:-regista a VM com o Azure AD para criar uma identidade gerida e configura a identidade da VM. 

1. Navegue para o grupo de recursos da nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Sob o **definições** categoria, clique em **configuração**.
3. Para ativar a identidade gerido, selecione **Sim**.
4. Clique em **guardar** para aplicar a configuração. 

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Nesta secção, vai criar uma conta de armazenamento. 

1. Clique em de **+ criar um recurso** botão encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **armazenamento**, em seguida, **conta de armazenamento - BLOBs, ficheiro, tabela, fila**.
3. Em **nome**, introduza um nome para a conta de armazenamento.  
4. **Modelo de implementação** e **conta kind** deve ser definido como **do Resource manager** e **armazenamento (v1 de objetivo geral)**. 
5. Certifique-se a **subscrição** e **grupo de recursos** corresponder aqueles que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Criar um contentor de blob e carregar um ficheiro para a conta de armazenamento

Os ficheiros requerem armazenamento de BLOBs, por isso terá de criar um contentor do blob no qual pretende armazenar o ficheiro. Em seguida, carregue um ficheiro para o contentor de blob na conta de armazenamento nova.

1. Navegue de volta para a sua conta de armazenamento criados recentemente.
2. Em **serviço Blob**, clique em **contentores**.
3. Clique em **+ contentor** no topo da página.
4. Em **novo contentor**, introduza um nome para o contentor e, em **nível de acesso público** mantenha o valor predefinido.

    ![Criar contentor de armazenamento](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Utilizando um editor à sua escolha, crie um ficheiro intitulado *Olá world.txt* no seu computador local.  Abra o ficheiro e adicione o texto (sem aspas) "Hello world! :) "e, em seguida, guardá-lo. 
6. Carregar o ficheiro para o contentor recentemente criado ao clicar no nome do contentor, em seguida, **carregar**
7. No **carregar blob** painel, em **ficheiros**, clique no ícone de pasta e procure o ficheiro **hello_world.txt** no seu computador local, selecione o ficheiro, em seguida, clique em **Carregar**.
    ![Carregar ficheiro de texto](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Conceder o acesso VM a um contentor de armazenamento do Azure 

Pode utilizar a identidade da VM de gerido para obter os dados no blob storage do Azure.   

1. Navegue de volta para a sua conta de armazenamento criados recentemente.  
2. Clique em de **(IAM) do controlo de acesso** ligação no painel esquerdo.  
3. Clique em **+ adicionar** na parte superior da página para adicionar uma nova atribuição de função para a VM.
4. Em **função**, na lista pendente, selecione **leitor de dados de BLOBs de armazenamento (pré-visualização)**. 
5. Na lista pendente seguinte, em **atribuir acesso**, escolha **Máquina Virtual**.  
6. Em seguida, certifique-se a subscrição correta está listada no **subscrição** pendente e, em seguida, defina **grupo de recursos** para **todos os grupos de recursos**.  
7. Em **selecione**, escolha a VM e, em seguida, clique em **guardar**. 

    ![Atribuir permissões](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Obter um token de acesso e utilizá-la para chamar o Storage do Azure 

Armazenamento do Azure nativamente autenticação de suporte do Azure AD, para que possa aceitar tokens de acesso diretamente obtidos através de uma identidade geridos. Isto faz parte da integração do Storage do Azure com o Azure AD e é diferente de fornecer credenciais na cadeia de ligação.

Eis um exemplo de código de .net de abrir uma ligação ao armazenamento do Azure utilizando um token de acesso e, em seguida, ao ler os conteúdos do ficheiro que criou anteriormente. Este código tem de executar na VM para conseguir aceder ao ponto final de geridos identidade da VM. .NET framework 4.6 ou superior, é necessário utilizar o método de token de acesso. Substitua o valor de `<URI to blob file>` em conformidade. Pode obter este valor, navegando para ficheiro criado e carregada para o armazenamento de BLOBs e copiar a **URL** em **propriedades** o **descrição geral** página.

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

A resposta contém o conteúdo do ficheiro:

`Hello world! :)`

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu como ativar a máquina virtual do Windows geridos de identidade para aceder ao armazenamento do Azure.  Para saber mais sobre armazenamento do Azure, consulte:

> [!div class="nextstepaction"]
> [Armazenamento do Azure](/azure/storage/common/storage-introduction)



