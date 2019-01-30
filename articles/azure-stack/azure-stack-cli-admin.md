---
title: Ativar a CLI do Azure para utilizadores do Azure Stack | Documentos da Microsoft
description: Saiba como utilizar a interface de linha de comandos para várias plataformas (CLI) para gerir e implementar recursos no Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
ms.date: 01/15/2019
ms.author: mabrigg
ms.lastreviewed: 01/15/2019
ms.openlocfilehash: 0d6a93b396e90f3bc27a1eb879556d774075c86f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243305"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>Ativar a CLI do Azure para utilizadores do Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode fornecer o certificado de AC de raiz para os utilizadores do Azure Stack para que eles possam utilizar a CLI do Azure nos seus computadores de desenvolvimento. Os utilizadores tem do certificado para gerir os recursos através da CLI.

 - **O certificado de raiz da AC do Azure Stack** é necessária se os utilizadores utilizam a CLI a partir de uma estação de trabalho fora o Development Kit do Azure Stack.  

 - **O ponto final de aliases de máquina virtual** fornece um alias, como "UbuntuLTS" ou "Win2012Datacenter", que faz referência a um publicador de imagem, oferta, SKU e versão como um único parâmetro durante a implantação de VMs.  

As secções seguintes descrevem como obter estes valores.

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exporte o certificado de raiz de AC do Azure Stack

Se estiver a utilizar um sistema integrado, não terá de exportar o certificado de raiz da AC. Terá de exportar o certificado de raiz de AC numa ferramenta do Azure Stack Development Kit (ASDK).

Para exportar o certificado de raiz ASDK no formato PEM, inicie sessão e execute o seguinte script:

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

Operadores do Azure Stack devem configurar um ponto final acessível publicamente que aloja um arquivo de alias da máquina virtual. O ficheiro de alias da máquina virtual é um ficheiro JSON que fornece um nome comum para uma imagem. Irá utilizar o nome ao implementar uma VM como um parâmetro de CLI do Azure.  

Antes de adicionar uma entrada para um ficheiro de alias, certifique-se de que [baixe imagens do Azure Marketplace](azure-stack-download-azure-marketplace-item.md) ou ter [publicado a sua própria imagem personalizada](azure-stack-add-vm-image.md). Se publicar uma imagem personalizada, anote as informações do publicador, oferta, SKU e versão que especificou durante a publicação. Se for uma imagem do marketplace, pode ver as informações utilizando o ```Get-AzureVMImage``` cmdlet.  

R [ficheiro de exemplo de alias](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) com imagem comum muitos aliases está disponível. Pode utilizá-la como um ponto de partida. Hospedar esse arquivo num espaço onde os clientes CLI podem aceder ao mesmo. Uma forma é hospedar o arquivo numa conta de armazenamento de BLOBs e partilhe o URL com os seus utilizadores:

1. Transfira o [ficheiro de exemplo](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) do GitHub.
2. Crie uma conta de armazenamento no Azure Stack. Quando isso é feito, crie um contentor de Blobs. Definir a política de acesso para "public".  
3. Carregar o ficheiro JSON para o novo contentor. Quando isso for concluído, pode ver o URL do blob. Selecione o nome do blob e, em seguida, selecionar o URL de propriedades do blob.

## <a name="next-steps"></a>Passos Seguintes

- [Implementar modelos com a CLI do Azure](azure-stack-deploy-template-command-line.md)
- [Ligar com o PowerShell](azure-stack-connect-powershell.md)
- [Gerir permissões de utilizador](azure-stack-manage-permissions.md)
