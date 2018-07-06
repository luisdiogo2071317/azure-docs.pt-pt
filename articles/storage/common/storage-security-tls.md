---
title: Ativar o TLS segura para o cliente de armazenamento do Azure | Documentos da Microsoft
description: Saiba como ativar o TLS 1.2 no cliente do armazenamento do Azure.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/25/2018
ms.author: fryu
ms.openlocfilehash: 6c313b6015a8a6dcc4ca5befb5fef70b047d0410
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866530"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Ativar o TLS seguro para o cliente do Armazenamento do Azure

Quando precisa auditar seus serviços usando o armazenamento do Azure com base na conformidade mais recente e requisitos de segurança, SSL 1.0, 2.0, 3.0 e TLS 1.0 são reconhecidos como protocolos de comunicação em não conformidade.

Foram encontrados SSL 1.0, 2.0 e 3.0 vulneráveis. Eles têm sido proibidos por RFC. TLS 1.0 torna-se inseguro para usar o bloco cifrado inseguro (DES CBC e RC2 CBC) e cifras de Stream (RC4). Conselho PCI sugeridas, também, a migração para versões do TLS superiores. Para obter mais detalhes, pode ver [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

O armazenamento do Azure foi parado o SSL 3.0 desde 2015 e utiliza o TLS 1.2 no pontos finais públicos do HTTPs, mas o TLS 1.0 e TLS 1.1 ainda são suportados para compatibilidade com versões anteriores.

Para garantir uma ligação segura e em conformidade para o armazenamento do Azure, terá de ativar o TLS 1.2 no lado do cliente antes de enviar pedidos para operar o serviço de armazenamento do Azure.

## <a name="enable-tls-12-in-net-client"></a>Ativar o TLS 1.2 no cliente do .NET

Para o cliente negociar o TLS 1.2, o sistema operacional e o .NET Framework versão ambos tem oferecer suporte a TLS 1.2. Veja mais detalhes no [suporte para TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

O exemplo a seguir mostra como ativar o TLS 1.2 no cliente .NET.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>Ativar o TLS 1.2 no cliente do PowerShell

O exemplo a seguir mostra como ativar o TLS 1.2 no cliente PowerShell.

```powershell

# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGropuName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzureStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers

```

## <a name="verify-tls-12-connection"></a>Verificar a ligação de TLS 1.2

Para verificar se o TLS 1.2 é usado, na verdade, pode usar o Fiddler. Abra o Fiddler para começar a capturar o tráfego de rede do cliente, em seguida, executar acima de exemplo. Em seguida, pode encontrar a versão do TLS na ligação que o exemplo faz.

Captura de ecrã seguinte é um exemplo para a verificação.

![captura de ecrã de verificação da versão do TLS no Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Consulte também

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [Ativar o TLS no cliente de Java](https://www.java.com/en/configure_crypto.html)
