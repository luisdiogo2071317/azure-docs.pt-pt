---
title: Ativar o TLS segura para o cliente do Storage do Azure | Microsoft Docs
description: Saiba como ativar o TLS 1.2 no cliente do Storage do Azure.
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
ms.openlocfilehash: 5c21df2b3bdeee6ac7c3956fe1cafa4f947dd6dd
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036434"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Ativar o TLS segura para o cliente do Storage do Azure

Quando tiver de auditoria os serviços no Storage do Azure a utilizar com base nos requisitos de segurança, SSL 1.0 e de conformidade mais recente, 2.0, 3.0 e TLS 1.0 são reconhecidos como sendo protocolos de comunicação não conformes.

Foram encontrados SSL 1.0, 2.0 e 3.0 ser vulnerável. Podem tem foi proibidas pelo RFC. TLS 1.0 fica inseguras para utilizar inseguras cifras de bloco (DES CBC e RC2 CBC) e as cifras fluxo (RC4). PCI council sugerida também a migração para versões do TLS superiores. Para obter mais detalhes, pode ver [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Armazenamento do Azure foi parado o SSL 3.0 desde 2015 e utiliza TLS 1.2 pontos finais públicos de HTTPs, mas TLS 1.0 e TLS 1.1 ainda são suportados para compatibilidade com versões anteriores.

Para garantir uma ligação segura e compatível para o armazenamento do Azure, terá de ativar o TLS 1.2 no lado do cliente antes de enviar pedidos para operar o serviço de armazenamento do Azure.

## <a name="enable-tls-12-in-net-client"></a>Ativar o TLS 1.2 no cliente do .NET

Para o cliente a negociação TLS 1.2, sistema operativo e a versão do .NET Framework ambos tem de suportar TLS 1.2. Ver mais detalhes no [suporte para TLS 1.2](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).

O exemplo seguinte mostra como ativar o TLS 1.2 no cliente do .NET.

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

O exemplo seguinte mostra como ativar o TLS 1.2 no cliente do PowerShell.

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

## <a name="verify-tls-12-connection"></a>Certifique-se a ligação de TLS 1.2

Pode utilizar o Fiddler para verificar se é utilizada, na verdade, a TLS 1.2. Abra o Fiddler para iniciar a capturar o tráfego de rede do cliente, em seguida, executar acima exemplo. Em seguida, pode encontrar a versão do TLS na ligação que torna o exemplo.

Captura de ecrã seguinte é um exemplo para a verificação.

![captura de ecrã de verificar a versão do TLS no Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Consulte também

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
