---
title: Ativar a CLI do Azure para utilizadores do Azure Stack | Documentos da Microsoft
description: Saiba como utilizar a interface de linha de comandos para várias plataformas (CLI) para gerir e implementar recursos no Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2018
ms.author: mabrigg
ms.openlocfilehash: c2827a4badd61aeb8de556795834dee39769e85e
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53554508"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Ativar a CLI do Azure para utilizadores do Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode fornecer o certificado de AC de raiz para os utilizadores do Azure Stack para que eles possam utilizar a CLI do Azure nos seus computadores de desenvolvimento. Os seus utilizadores terão o certificado para gerir os recursos através da CLI.

* **O certificado de raiz da AC do Azure Stack** é necessária se os utilizadores utilizam a CLI a partir de uma estação de trabalho fora o Development Kit do Azure Stack.  

* **O ponto final de aliases de máquina virtual** fornece um alias, como "UbuntuLTS" ou "Win2012Datacenter", que faz referência a um publicador de imagem, oferta, SKU e versão como um único parâmetro durante a implantação de VMs.  

As secções seguintes descrevem como obter estes valores.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exporte o certificado de raiz de AC do Azure Stack

Pode encontrar o certificado de raiz da AC do Azure Stack no kit de desenvolvimento e numa máquina de virtual de inquilino que está em execução no ambiente do kit de desenvolvimento. Para exportar o certificado de raiz do Azure Stack no formato PEM, inicie sessão no seu kit de desenvolvimento ou a máquina virtual de inquilino e execute o seguinte script:

```powershell
$label = "<Your Azure Stack CA root certificate name>"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Configurar o ponto final de aliases de máquina virtual

Operadores do Azure Stack devem configurar um ponto final acessível publicamente que aloja um arquivo de alias da máquina virtual. O ficheiro de alias da máquina virtual é um ficheiro JSON que fornece um nome comum para uma imagem. Esse nome é especificado, em seguida, quando uma VM é implementada como um parâmetro de CLI do Azure.  

Antes de adicionar uma entrada para um ficheiro de alias, certifique-se de que [baixe imagens do Azure Marketplace](azure-stack-download-azure-marketplace-item.md), ou ter [publicado a sua própria imagem personalizada](azure-stack-add-vm-image.md). Se publicar uma imagem personalizada, anote as informações do publicador, oferta, SKU e versão que especificou durante a publicação. Se for uma imagem do marketplace, pode ver as informações utilizando o ```Get-AzureVMImage``` cmdlet.  

R [ficheiro de exemplo de alias](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) com imagem comum muitos aliases está disponível. Pode utilizá-la como um ponto de partida. Hospedar esse arquivo num espaço onde os clientes CLI podem aceder ao mesmo. Uma forma é hospedar o arquivo numa conta de armazenamento de BLOBs e partilhe o URL com os seus utilizadores:

1. Transfira o [ficheiro de exemplo](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) do GitHub.
2. Crie uma nova conta de armazenamento no Azure Stack. Quando isso é feito, crie um novo contentor de Blobs. Definir a política de acesso para "public".  
3. Carregar o ficheiro JSON para o novo contentor. Quando isso for concluído, pode ver o URL do blob, selecionando o nome do blob e, em seguida, selecionar o URL de propriedades do blob.

## <a name="next-steps"></a>Passos Seguintes

- [Implementar modelos com a CLI do Azure](azure-stack-deploy-template-command-line.md)
- [Ligar com o PowerShell](azure-stack-connect-powershell.md)
- [Gerir permissões de utilizador](azure-stack-manage-permissions.md)
