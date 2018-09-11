---
title: 'Gerar e exportar certificados para ligações ponto a Site: Linux: CLI: Azure | Documentos da Microsoft'
description: Criar um certificado de raiz autoassinado, exportar a chave pública e gerar certificados de cliente com a Linux (strongSwan) da CLI.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: 8647b3b3eda980dbd5d5ec368b6b4b13949ecaf1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305745"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-linux-strongswan-cli"></a>Gerar e exportar certificados para ligações ponto a Site com o Linux strongSwan CLI

As ligações ponto a Site utilizam certificados para autenticar. Este artigo mostra-lhe como criar um certificado de raiz autoassinados e gerar certificados de cliente utilizando a CLI de Linux e strongSwan. Se estiver à procura para obter instruções de certificado diferente, consulte a [Powershell](vpn-gateway-certificates-point-to-site.md) ou [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) artigos.

> [!NOTE]
> Os passos neste artigo requerem strongSwan.
>

A configuração de computador utilizada para obter os passos para este artigo foi o seguinte:

| | |
|---|---|
|**Computador**| Ubuntu Server 16.04<br>ID_LIKE = debian<br>PRETTY_NAME = "16.04.4 de Ubuntu LTS"<br>VERSION_ID = "16.04" |
|**Dependências**| Apt-get install strongswan ikev2 strongswan-Plug-in-eap-tls<br>Apt-get install libstrongswan-padrão-plug-ins |

## <a name="install-strongswan"></a>Instalar strongSwan

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

Para obter informações sobre como instalar strongSwan através da GUI, consulte os passos a [configuração do cliente](point-to-site-vpn-client-configuration-azure-cert.md#install) artigo.

## <a name="generate-keys-and-certificate"></a>Gerar chaves e certificados

1. Gere o certificado da AC.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```
2. Imprima o certificado da AC no formato base64. Este é o formato que seja suportado pelo Azure. Será mais tarde carregá-lo para o Azure como parte da sua configuração de P2S.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```
3. Gere o certificado de utilizador.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```
4. Gere um pacote de p12 que contém o certificado de utilizador. Este pacote será utilizada nos passos seguintes ao trabalhar com o [ficheiros de configuração de cliente](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```

## <a name="next-steps"></a>Passos Seguintes

Continuar com a sua configuração de ponto a Site para [criar e instalar ficheiros de configuração de cliente VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).