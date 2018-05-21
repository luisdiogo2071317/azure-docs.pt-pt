---
title: Renovar um certificado de Gateway de aplicação do Azure
description: Saiba como renovar um certificado associado a um serviço de escuta do gateway de aplicação.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/17/2018
ms.author: victorh
ms.openlocfilehash: b125f707e8de17764701e981736a53492e5e756c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="renew-application-gateway-certificates"></a>Renovar os certificados de Gateway de aplicação

Em algum momento, terá de renovar os certificados, se tiver configurado o gateway de aplicação para a encriptação de SSL.

Pode renovar um certificado associado a um serviço de escuta com o portal do Azure ou o Azure PowerShell:

## <a name="azure-portal"></a>Portal do Azure

Para renovar um certificado de serviço de escuta do portal, navegue para os serviços de escuta do gateway de aplicação. Clique no serviço de escuta que tenha um certificado que tem de ser renovado e, em seguida, clique em **renovar ou editar o certificado selecionado**.

![Renovar o certificado](media/renew-certificate/ssl-cert.png)

Carregar o novo certificado PFX, atribua um nome, escreva a palavra-passe e, em seguida, clique em **guardar**.

## <a name="azure-powershell"></a>Azure PowerShell

Para renovar o certificado com o Azure PowerShell, utilize o seguinte cmdlet:

```PowerShell
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

## <a name="next-steps"></a>Passos Seguintes

Para saber como configurar a descarga de SSL com o Gateway de aplicação do Azure, consulte [configurar a descarga de SSL](application-gateway-ssl-portal.md)
