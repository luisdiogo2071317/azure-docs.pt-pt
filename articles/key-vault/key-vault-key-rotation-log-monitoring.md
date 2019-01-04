---
title: Configurar o Azure Key Vault com a rotação de chaves-a-ponto e auditoria - Azure Key Vault | Documentos da Microsoft
description: Utilize este procedimentos para ajudá-lo mãos à obra com monitorização de Cofre de chaves de registos e a rotação de chaves.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: ''
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: barclayn
ms.openlocfilehash: 11506f63089564b5187ebd6177f7187f1faf6655
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999599"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Configurar o Azure Key Vault com auditoria e rotação de chaves

## <a name="introduction"></a>Introdução

Depois de ter um cofre de chaves, pode começar a usá-lo para armazenar chaves e segredos. Seus aplicativos não precisam mais manter suas chaves ou segredos, mas pode solicitar no cofre conforme necessário. Isto permite-lhe atualizar as chaves e segredos sem afetar o comportamento do seu aplicativo, o qual abre uma ampla de possibilidades em torno de sua chave e a gestão de segredos.

>[!IMPORTANT]
> Os exemplos neste artigo são fornecidos apenas para fins ilustrativos. Estas não recomendações pretendem para utilização em produção. 

Este artigo explica o tempo limite:

- Um exemplo de como utilizar o Azure Key Vault para armazenar um segredo. Neste tutorial, o segredo armazenado é a chave de conta de armazenamento do Azure acessada por um aplicativo. 
- Ele também demonstra a implementação de uma rotação agendada dessa chave de conta de armazenamento.
- Ele demonstra como monitorizar os registos de auditoria do Cofre de chaves e emitir alertas quando são feitos inesperados de pedidos.

> [!NOTE]
> Este tutorial destina-se não a explicar em detalhes a configuração inicial do seu Cofre de chaves. Para obter estas informações, consulte o artigo [Introdução ao Cofre de Chaves do Azure](key-vault-get-started.md). Para obter instruções de Interface de linha de comandos para várias plataformas, consulte [gerir Key Vault com a CLI](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Configurar o Cofre de Chaves

Para ativar uma aplicação para obter um segredo do Key Vault, tem primeiro de criar o segredo e carregue-o para o cofre. Isso pode ser feito ao iniciar uma sessão do PowerShell do Azure e iniciar sessão na sua conta do Azure com o seguinte comando:

```powershell
Connect-AzureRmAccount
```

Na janela pop-up do browser, introduza o seu nome de utilizador da conta do Azure e a palavra-passe. PowerShell irá obter todas as subscrições que estão associadas esta conta. PowerShell utiliza a primeira por predefinição.

Se tiver várias subscrições, poderá ter de especificar que foi utilizado para criar o seu Cofre de chaves. Introduza o seguinte para ver as subscrições da sua conta:

```powershell
Get-AzureRmSubscription
```

Para especificar a subscrição que está associada ao Cofre de chaves que irá registar, introduza:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Uma vez que este artigo demonstra o armazenamento de uma chave de conta de armazenamento como um segredo, tem de obter essa chave de conta de armazenamento.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Depois de recuperar o seu segredo (no caso, a chave de conta de armazenamento), tem de converter isso numa cadeia segura e, em seguida, criar um segredo com esse valor no seu Cofre de chaves.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Em seguida, obtém o URI para o segredo que criou. Isto é utilizado num passo posterior ao chamar o Cofre de chaves para recuperar o seu segredo. Execute o seguinte comando do PowerShell e anote o valor de ID, o que é o URI secreto:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Configurar a aplicação

Agora que tem um segredo armazenado, pode utilizar o código para recuperar e utilizá-lo. Existem alguns passos necessários para atingir esse objetivo. O primeiro passo e o mais importante é a registar a sua aplicação no Azure Active Directory e, em seguida, informar Key Vault informações sobre os aplicativos para que ela pode permitir pedidos a partir da sua aplicação.

> [!NOTE]
> A aplicação tem de ser criada no mesmo inquilino do Azure Active Directory como o seu Cofre de chaves.
>
>

1. Navegue para o Azure Active Directory.
2. Escolha **registos das aplicações** 
3. Escolher **novo registo de aplicação** para adicionar uma aplicação para o Azure Active Directory.

    ![Abrir aplicações no Azure Active Directory](./media/keyvault-keyrotation/azure-ad-application.png)

4. Na **Create** secção deixe o tipo de aplicativo como **aplicação WEB e/ou API WEB** e dê um nome ao seu aplicativo. Dê o seu aplicativo uma **URL de início de sessão no**. Isso pode ser qualquer coisa que pretende para esta demonstração.

    ![Criar registo de aplicação](./media/keyvault-keyrotation/create-app.png)

5. Depois do aplicativo é adicionado ao Azure Active Directory, será reencaminhado para a página de aplicativo. Selecione **definições** e, em seguida, selecione propriedades. Copiar o **ID da aplicação** valor. É necessária em passos posteriores.

Em seguida, gere uma chave para a sua aplicação, para que possa interagir com o Azure Active Directory. Pode criar uma chave em ao navegar para o **chaves** secção sob **definições**. Tome nota da chave recentemente gerado através da aplicação do Azure Active Directory para utilização num passo posterior. Tenha em atenção que a chave não estará disponível depois de navegar para fora desta secção. 

![Chaves da aplicação do Azure Active Directory](./media/keyvault-keyrotation/create-key.png)

Antes de estabelecer todas as chamadas do seu aplicativo para o Cofre de chaves, tem de pedir ao Cofre de chaves sobre seu aplicativo e as respetivas permissões. O comando seguinte utiliza o nome do cofre e o ID da aplicação da sua aplicação do Azure Active Directory e concessões **obter** acesso ao seu Cofre de chaves para a aplicação.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Neste momento, está pronto para começar a criar as suas chamadas de aplicação. Na sua aplicação, tem de instalar os pacotes de NuGet necessários para interagir com o Azure Key Vault e o Azure Active Directory. Na consola do Gestor de pacotes do Visual Studio, introduza os seguintes comandos. No momento da edição deste artigo, a versão atual do pacote do Azure Active Directory é 3.10.305231913, pelo que pode confirmar a versão mais recente e atualizar em conformidade.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

No código da aplicação, crie uma classe para manter o método para a autenticação do Azure Active Directory. Neste exemplo, essa classe é chamada **Utils**. Adicione o seguinte ao utilizar a instrução:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Em seguida, adicione o método seguinte para recuperar o token JWT do Azure Active Directory. Para facilidade de manutenção, pode querer mover os valores de cadeia de caracteres codificada para a configuração da web ou aplicação.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Adicione o código necessário para chamar o Cofre de chaves e obter o seu valor secreto. Primeiro tem de adicionar o seguinte usando a instrução:

```csharp
using Microsoft.Azure.KeyVault;
```

Adicione as chamadas de método para invocar o Cofre de chaves e obter o seu segredo. Nesse método, forneça o segredo do URI que guardou no passo anterior. Observe o uso do **GetToken** método a partir do **Utils** classe criada anteriormente.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Quando executar o aplicativo, deverá estar agora autenticar no Azure Active Directory e, em seguida, obter seu valor secreto no Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Rotação de chaves com a automatização do Azure

Existem várias opções para implementar uma estratégia de rotação para valores que armazenar como segredos do Cofre de chaves do Azure. Segredos podem ser girados como parte de um processo manual, pode ser girados por meio de programação utilizando chamadas de API ou pode ser girados por meio de um script de automação. Para os fins deste artigo, irá utilizar combinada com a automatização do Azure do Azure PowerShell para alterar uma chave de acesso da conta de armazenamento do Azure. Em seguida, atualizar um segredo do Cofre de chaves com essa nova chave.

Para permitir a automatização do Azure definir valores secretos no seu Cofre de chaves, tem de obter o ID de cliente para a ligação com o nome AzureRunAsConnection, que foi criada quando que estabeleceu a sua instância de automatização do Azure. Pode encontrar este ID escolhendo **ativos** da sua instância de automatização do Azure. A partir daí, escolha **conexões** e, em seguida, selecione a **AzureRunAsConnection** princípio do serviço. Anote o **ID da aplicação**.

![ID de cliente de automatização do Azure](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Na **ativos**, escolha **módulos**. Partir **módulos**, selecione **galeria**e, em seguida, procure e **importar** atualizado de versões de cada um dos seguintes módulos:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> No momento da edição deste artigo, apenas os módulos mencionados anteriormente precisava ser atualizado com o seguinte script. Se achar que a tarefa de automatização está a falhar, confirme que importou todos os módulos necessários e as respetivas dependências.
>
>

Depois de ter obtido o ID da aplicação para a sua ligação de automatização do Azure, tem de pedir ao seu Cofre de chaves que esta aplicação tem acesso para atualizar os segredos no cofre. Isso pode ser feito com o seguinte comando do PowerShell:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Em seguida, selecione **Runbooks** sob a sua instância de automatização do Azure e, em seguida, selecione **adicionar um Runbook**. Selecione **Criação Rápida**. Nome do runbook e selecione **PowerShell** como o tipo de runbook. Tem a opção para adicionar uma descrição. Por fim, clique em **criar**.

![Criar runbook](./media/keyvault-keyrotation/Create_Runbook.png)

Cole o seguinte script do PowerShell no painel do editor para o seu runbook novo:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

No painel do editor, escolha **painel de teste** para testar o seu script. Depois do script é executado sem erros, pode selecionar **publicar**, e, em seguida, pode aplicar uma agenda para o runbook novamente no painel de configuração de runbook.

## <a name="key-vault-auditing-pipeline"></a>Pipeline de auditoria do Key Vault
Quando configurar um cofre de chaves, pode ativar a auditoria para recolher registos de pedidos de acesso efetuados ao Cofre de chaves. Estes registos são armazenados numa conta de armazenamento do Azure designada e podem ser puxados, monitorizados e analisados. O cenário a seguir utiliza as funções do Azure, Azure logic apps e os registos de auditoria do Cofre de chaves para criar um pipeline para enviar um e-mail quando uma aplicação que corresponde ao ID da aplicação da aplicação web obtém segredos do cofre.

Em primeiro lugar, tem de ativar o registo no seu Cofre de chaves. Isso pode ser feito por meio dos seguintes comandos do PowerShell (todos os detalhes podem ser vistos em [registo do Cofre de chave](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Depois de esta opção estiver ativada, de registos de auditoria início recolher para a conta de armazenamento designada. Estes registos contém eventos sobre como e quando os seus cofres de chaves são acedidos e por quem.

> [!NOTE]
> Pode acessar suas informações de registo a 10 minutos após a operação de Cofre de chaves. Normalmente, será ainda mais rápido.
>
>

A próxima etapa é [criar uma fila do Azure Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Isso é onde são enviados por push registos de auditoria do Cofre de chaves. Quando as mensagens de registo de auditoria estão na fila, a aplicação lógica escolhe-los e atua nos mesmos. Crie um barramento de serviço com os seguintes passos:

1. Crie um espaço de nomes do Service Bus (se já tiver um que pretende utilizar para este, avance para o passo 2).
2. Navegue para o service bus no portal do Azure e selecione o espaço de nomes que pretende criar a fila.
3. Selecione **criar um recurso**, **Enterprise Integration**, **do Service Bus**e, em seguida, introduza os detalhes necessários.
4. Selecione as informações de ligação do Service Bus ao selecionar o espaço de nomes e clicar em **informações da ligação**. Precisará dessa informação para a próxima seção.

Em seguida, [criar uma função do Azure](../azure-functions/functions-create-first-azure-function.md) para consultar os registos do Cofre de chaves na conta de armazenamento e recolher eventos de novo. Esta será uma função que é acionada com base numa agenda.

Para criar uma função do Azure, escolha **criar um recurso**, procure no marketplace _Function App_e clique em **criar**. Durante a criação, pode utilizar um plano de alojamento existente ou crie um novo. Também pode optar pela de alojamento dinâmico. Obter mais detalhes sobre as opções de hospedagem de função podem ser encontrados em [como dimensionar as funções do Azure](../azure-functions/functions-scale.md).

Quando é criada a função do Azure, navegue até à mesma e escolha um temporizador função e C\#. Em seguida, clique em **criar esta função**.

![Painel de início de funções do Azure](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Sobre o **desenvolver** separador, substitua o código de csx pelo seguinte:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Certifique-se substituir as variáveis no código anterior para apontar para a sua conta de armazenamento onde os registos do Cofre de chaves são escritos, o service bus que criou anteriormente e o caminho específico para os registos de armazenamento do Cofre de chaves.
>
>

A função escolhe o ficheiro de registo mais recente da conta de armazenamento onde os registos do Cofre de chaves são escritos, capta os eventos mais recentes desse ficheiro e envia-as para uma fila do Service Bus. Uma vez que um único ficheiro pode ter vários eventos, deve criar um ficheiro de sync.txt que a função também analisa para determinar o carimbo de data / hora do último evento que foi capturado. Isto garante que não a emitir o mesmo evento várias vezes. Este ficheiro de sync.txt contém um carimbo para o último evento foi encontrado. Os registos, quando carregado, tem de ser ordenados com base no carimbo de hora para garantir que eles estão ordenados corretamente.

Para essa função, podemos fazer referência a duas bibliotecas adicionais que não estão disponíveis prontos a utilizar nas funções do Azure. Para inclui-las, precisamos de funções do Azure para selecioná-los usando o NuGet. Escolha o **ver ficheiros** opção.

![Ver a opção de ficheiros](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

E adicione um arquivo chamado Project com o seguinte conteúdo:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```

Após **guardar**, as funções do Azure irá transferir os binários necessários.

Mude para o **integrar** separador e dê o parâmetro de temporizador um nome significativo para utilizar dentro da função. No código anterior, ele espera que o temporizador seja chamado *myTimer*. Especifique um [expressão CRON](../app-service/webjobs-create.md#CreateScheduledCRON) da seguinte forma: 0 \* \* \* \* \* para o temporizador que fará com que a função ser executado uma vez um minuto.

No mesmo **integrar** separador, adicione uma entrada do tipo **armazenamento de Blobs do Azure**. Isto irá apontar para o ficheiro de sync.txt que contém o carimbo de hora do último evento examinou pela função. Isso vai estar disponível dentro da função com o nome de parâmetro. No código anterior, a entrada de armazenamento de Blobs do Azure espera que o nome do parâmetro seja *inputBlob*. Selecione a conta de armazenamento onde o ficheiro de sync.txt irá residir (pode ser o mesmo ou outra conta de armazenamento). No campo do caminho, forneça o caminho onde o arquivo reside no formato {container-name}/path/to/sync.txt.

Adicionar uma saída do tipo *armazenamento de Blobs do Azure* saída. Isto irá apontar para o arquivo de sync.txt que definiu na entrada. Isso é utilizado pela função para escrever o carimbo de hora do último evento examinou. Este parâmetro a ser chamado de espera que o código precedente *outputBlob*.

Neste momento, a função está pronta. Certifique-se mudar para o **desenvolver** separador e guarde o código. A janela de saída para os erros de compilação e corrija-os em conformidade. Se o código é compilado, em seguida, o código deve agora ser a verificar os registos do Cofre de chaves a cada minuto e no envio de quaisquer novos eventos para a fila de barramento de serviço definido. Deverá ver informações de registo escrever para a janela de registo sempre que a função é acionada.

### <a name="azure-logic-app"></a>Aplicação lógica do Azure

Em seguida tem de criar uma aplicação de lógica do Azure que seleciona os eventos que a função é enviar por push para a fila do Service Bus, analisa o conteúdo e envia um e-mail com base numa condição que está a ser correspondida.

[Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) ao aceder **New > aplicação lógica**.

Depois de criada a aplicação lógica, navegar até ele e escolha **editar**. No editor de aplicação lógica, escolha **fila do Service Bus** e introduza as credenciais do Service Bus para ligá-lo para a fila.

![Barramento de serviço de aplicações lógicas do Azure](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Em seguida selecione **adicionar uma condição**. Na condição, mude para o editor avançado e introduza o código seguinte, substituindo APP_ID com o APP_ID real da sua aplicação web:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Esta expressão devolve essencialmente **false** se o *appid* do evento de entrada (que é o corpo da mensagem do Service Bus) não é o *appid* da aplicação.

Agora, crie uma ação sob **se não, não fazer nada**.

![Ação de escolha de aplicação de lógica do Azure](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Para a ação, escolha **Office 365 - enviar e-mail**. Preencha os campos para criar uma mensagem de e-mail para enviar quando a condição definida devolve **false**. Se não tiver o Office 365, pode encarar alternativas para atingir os mesmos resultados.

Neste ponto, tem um pipeline ponto a ponto que procura por registos de auditoria do novo cofre de chaves, uma vez a uma minuto. Novos registos que encontra, emite a uma fila do service bus. A aplicação lógica é acionada quando uma nova mensagem, coloca na fila. Se o *appid* dentro do evento não coincide com o ID de aplicação do aplicativo de chamada, envia uma mensagem de e-mail.
