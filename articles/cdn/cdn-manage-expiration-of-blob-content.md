---
title: "Gerir a expiração do Blob storage do Azure numa rede de entrega de conteúdo do Azure | Microsoft Docs"
description: "Saiba mais sobre as opções para controlar o time-to-live para os blobs na CDN do Azure a colocação em cache."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/1/2018
ms.author: mazha
ms.openlocfilehash: bafb04a1a19c4436d8f6c1c21700e9463334b3de
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Gerir a expiração do Blob storage do Azure numa rede de entrega de conteúdo do Azure
> [!div class="op_single_selector"]
> * [Conteúdo Web do Azure](cdn-manage-expiration-of-cloud-service-content.md)
> * [Armazenamento de Blobs do Azure](cdn-manage-expiration-of-blob-content.md)
> 
> 

O [serviço de armazenamento de BLOBs](../storage/common/storage-introduction.md#blob-storage) no armazenamento do Azure é uma das várias origens baseado no Azure integrada com rede de entrega de conteúdos (CDN) do Azure. Qualquer conteúdo de blob acessível publicamente pode ser colocadas em cache na CDN do Azure depois de decorrido o time-to-live (TTL). O valor de TTL é determinado pelo `Cache-Control` cabeçalho de resposta HTTP do servidor de origem. Este artigo descreve as várias formas que pode definir o `Cache-Control` cabeçalho de um blob no Storage do Azure.

Também pode controlar as definições de cache do portal do Azure através da definição [CDN regras a colocação em cache](#setting-cache-control-headers-by-using-caching-rules). Se criar uma regra de colocação em cache e defina o respetivo comportamento de colocação em cache para **substituir** ou **ignorar a cache**, as definições de colocação em cache origem fornecidos pelo abordadas neste artigo são ignoradas. Para obter informações sobre os conceitos gerais de colocação em cache, consulte [funciona como colocação em cache](cdn-how-caching-works.md).

> [!TIP]
> Pode optar por não definir nenhum valor de TTL no blob. Neste caso, o CDN do Azure aplica automaticamente uma predefinição TTL de sete dias, a menos que configurou regras no portal do Azure a colocação em cache. Esta predefinição TTL só se aplica a otimizações de entrega web geral. Para otimizações de ficheiros grandes, o TTL predefinido é um dia e para o suporte de dados otimizações de transmissão em fluxo, o TTL predefinido é de um ano.
> 
> Para obter mais informações sobre como funciona o CDN do Azure para acelerar o acesso para os blobs e outros ficheiros, consulte [descrição geral da rede de entrega de conteúdos de Azure](cdn-overview.md).
> 
> Para obter mais informações sobre o armazenamento de Blobs do Azure, consulte [introdução ao Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Definir cabeçalhos Cache-Control ao utilizar a CDN regras a colocação em cache
O método preferencial para definir um blob `Cache-Control` cabeçalho consiste em utilizar regras de colocação em cache no portal do Azure. Para obter mais informações sobre a CDN regras a colocação em cache, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

> [!NOTE] 
> Regras de colocação em cache só estão disponíveis para **CDN do Azure da Verizon padrão** e **CDN do Azure da Akamai padrão** perfis. Para **CDN do Azure da Verizon Premium** perfis, tem de utilizar o [motor de regras da CDN do Azure](cdn-rules-engine.md) no **gerir** portal para uma funcionalidade semelhante.

**Para navegar para a página regras de colocação em cache CDN**:

1. No portal do Azure, selecione um perfil CDN, em seguida, selecione o ponto final do blob.

2. No painel esquerdo em definições, selecione **regras a colocação em cache**.

   ![Botão de regras de colocação em cache CDN](./media/cdn-manage-expiration-of-blob-content/cdn-caching-rules-btn.png)

   O **regras a colocação em cache** é apresentada a página.

   ![Página de colocação em cache de CDN](./media/cdn-manage-expiration-of-blob-content/cdn-caching-page.png)


**Para definir os cabeçalhos Cache-Control de um serviço de armazenamento de BLOBs, através de regras de colocação em cache global:**

1. Em **Global regras a colocação em cache**, defina **comportamento de colocação em cache de cadeia de consulta** para **ignorar cadeias de consulta** e defina **comportamento de colocação em cache** para  **Substituir**.
      
2. Para **duração de expiração da Cache**, introduza 3600 no **segundos** caixa ou 1 no **horas** caixa. 

   ![Exemplo de regras de colocação em cache global de CDN](./media/cdn-manage-expiration-of-blob-content/cdn-global-caching-rules-example.png)

   Esta regra de colocação em cache global define uma duração da cache de uma hora e afeta todos os pedidos para o ponto final. Substitui qualquer `Cache-Control` ou `Expires` cabeçalhos de HTTP são enviados pelo servidor de origem especificado pelo ponto final.   

3. Selecione **Guardar**.
 
**Para definir um blob Cache-Control cabeçalhos do ficheiro utilizando regras personalizadas de colocação em cache:**

1. Em **personalizada de colocação em cache regras**, crie duas condições de correspondência:

     A. Para a primeira condição de correspondência, defina **correspondem à condição** para **caminho** e introduza `/blobcontainer1/*` para **corresponde ao valor**. Definir **comportamento de colocação em cache** para **substituir** e introduza 4 no **horas** caixa.

    B. Para a segunda condição de correspondência, defina **correspondem à condição** para **caminho** e introduza `/blobcontainer1/blob1.txt` para **corresponde ao valor**. Definir **comportamento de colocação em cache** para **substituir** e introduza 2 no **horas** caixa.

    ![Exemplo de regras de colocação em cache personalizado de CDN](./media/cdn-manage-expiration-of-blob-content/cdn-custom-caching-rules-example.png)

    A primeira regra de colocação em cache personalizada define uma duração da cache de quatro horas para os ficheiros blob a `/blobcontainer1` pasta no servidor de origem especificado pelo seu ponto final. A segunda regra substitui a primeira regra para o `blob1.txt` apenas ficheiro de blob e define a duração da cache de duas horas para a mesma.

2. Selecione **Guardar**.


## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Definir cabeçalhos Cache-Control utilizando o Azure PowerShell
[O Azure PowerShell](/powershell/azure/overview) é uma das formas mais rápidas e mais poderosas para administrar os serviços do Azure. Utilize o `Get-AzureStorageBlob` cmdlet para obter uma referência para o blob, em seguida, defina o `.ICloudBlob.Properties.CacheControl` propriedade. 

Por exemplo:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Também pode utilizar o PowerShell para [gerir os seus perfis da CDN e pontos finais](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Definir cabeçalhos Cache-Control utilizando o .NET
Para especificar um blob `Cache-Control` cabeçalho com o código de .NET, utilize o [biblioteca de clientes do Storage do Azure para .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) para definir o [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) propriedade.

Por exemplo:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer <container name> = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob <blob name> = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Existem mais exemplos de código do .NET no [exemplos de armazenamento de Blobs do Azure para .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Definir cabeçalhos Cache-Control através da utilização de outros métodos

### <a name="azure-storage-explorer"></a>Explorador do Storage do Azure
Com [Explorador de armazenamento do Azure](https://azure.microsoft.com/en-us/features/storage-explorer/), pode ver e editar os recursos do blob storage, incluindo propriedades, como o *CacheControl* propriedade. 

Para atualizar o *CacheControl* propriedade de um blob com o Explorador de armazenamento do Azure:
   1. Selecione um blob, em seguida, selecione **propriedades** no menu de contexto. 
   2. Desloque para baixo até o *CacheControl* propriedade.
   3. Introduza um valor, em seguida, selecione **guardar**.


![Propriedades do Explorador de armazenamento do Azure](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Interface de Linha de Comandos do Azure
Com o [Interface de linha de comandos do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) (CLI), pode gerir recursos do blob do Azure na linha de comandos. Para configurar o cabeçalho de cache-control ao carregar um blob com a CLI do Azure, definir o *cacheControl* propriedade utilizando o `-p` mudar. O exemplo seguinte mostra como definir o valor de TTL para uma hora (3600 segundos):
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\<blob name> <container name> <blob name>
```

### <a name="azure-storage-services-rest-api"></a>REST API dos serviços de armazenamento do Azure
Pode utilizar o [REST API dos serviços de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx) para definir explicitamente o *x-ms-BLOBs-cache-control* propriedade utilizando as seguintes operações num pedido:
  
   - [Colocar Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Colocar a lista de bloqueios](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Defina as propriedades de Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>O cabeçalho de Cache-Control de teste
Pode facilmente verificar as definições de TTL dos blobs. Com o seu browser [ferramentas de programador](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), teste o blob inclui o `Cache-Control` cabeçalho de resposta. Também pode utilizar uma ferramenta como [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), ou [Fiddler](http://www.telerik.com/fiddler) para examinar os cabeçalhos de resposta.

## <a name="next-steps"></a>Próximos Passos
* [Saiba como gerir a expiração de conteúdo do serviço em nuvem na CDN do Azure](cdn-manage-expiration-of-cloud-service-content.md)
* [Saiba mais sobre conceitos a colocação em cache](cdn-how-caching-works.md)

