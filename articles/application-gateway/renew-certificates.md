---
title: Renovar um certificado de Gateway de aplicação do Azure
description: Saiba como renovar um certificado associado a um serviço de escuta do gateway de aplicação.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/18/2018
ms.author: victorh
ms.openlocfilehash: b44a57fe8ebcc985d3ab66ea04936a1558d00863
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598271"
---
# <a name="renew-application-gateway-certificates"></a>Renovar os certificados de Gateway de aplicação

Em algum momento, terá de renovar os certificados, se tiver configurado o gateway de aplicação para a encriptação de SSL.

Pode renovar um certificado associado a um serviço de escuta com o portal do Azure, Azure PowerShell ou a CLI do Azure:

## <a name="azure-portal"></a>Portal do Azure

Para renovar um certificado de serviço de escuta do portal, navegue para os serviços de escuta do gateway de aplicação. Clique no serviço de escuta que tenha um certificado que tem de ser renovado e, em seguida, clique em **renovar ou editar o certificado selecionado**.

![Renovar o certificado](media/renew-certificate/ssl-cert.png)

Carregar o novo certificado PFX, atribua um nome, escreva a palavra-passe e, em seguida, clique em **guardar**.

## <a name="azure-powershell"></a>Azure PowerShell

Para renovar o certificado com o Azure PowerShell, utilize o seguinte cmdlet:

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-azureRmApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password
```
## <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Passos Seguintes

Para saber como configurar a descarga de SSL com o Gateway de aplicação do Azure, consulte [configurar a descarga de SSL](application-gateway-ssl-portal.md)
