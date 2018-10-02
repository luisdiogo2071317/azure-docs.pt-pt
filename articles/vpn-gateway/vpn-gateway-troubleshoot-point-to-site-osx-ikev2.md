---
title: Resolver problemas de ligações de VPN do Azure ponto a Site de clientes de Mac OS X | Documentos da Microsoft
description: Passos para resolver problemas de ligações de cliente de P2S Mac OS X VPN
services: vpn-gateway
documentationcenter: na
author: anzaman
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: bc8e95b07d984505fc0e5fdad41fe120463b5b0e
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585531"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Resolver problemas de ligações de VPN Point-to-Site de clientes de VPN do Mac OS X

Este artigo ajuda-o a resolver problemas de conectividade de ponto a Site em Mac OS X com o cliente VPN nativo e IKEv2. O cliente VPN no Mac para IKEv2 é bastante básico e não permite uma personalização muito. Existem apenas quatro definições que precisam ser verificados:

* Endereço do Servidor
* ID remoto
* ID do local
* Definições de autenticação
* Versão do SO (10.11 ou superior)


## <a name="VPNClient"></a> Resolver problemas de autenticação com base em certificado
1. Verifique as definições de cliente VPN. Vá para o **definição de rede** pressionando comando + Shift e em seguida, escreva "VPN" para verificar as definições de cliente VPN. Na lista, clique na entrada VPN que tem de ser investigado.

  ![Autenticação baseada em certificado do IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Certifique-se de que o **endereço do servidor** é o FQDN completo e inclui o cloudapp.net.
3. O **remoto ID** deve ser o mesmo que o endereço do servidor (FQDN do Gateway).
4. O **ID de locais** deve ser igual a **assunto** do certificado de cliente.
5. Clique em **definições de autenticação** para abrir a página de definições de autenticação.

  ![Definições de autenticação](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Certifique-se de que **certificado** está selecionada na lista pendente.
7. Clique nas **selecione** botão e certifique-se de que o certificado correto está selecionado. Clique em **OK** para guardar as alterações.

## <a name="ikev2"></a>Resolver problemas de autenticação de nome de utilizador e palavra-passe

1. Verifique as definições de cliente VPN. Vá para o **definição de rede** pressionando comando + Shift e em seguida, escreva "VPN" para verificar as definições de cliente VPN. Na lista, clique na entrada VPN que tem de ser investigado.

  ![Palavra-passe de nome de utilizador de IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Certifique-se de que o **endereço do servidor** é o FQDN completo e inclui o cloudapp.net.
3. O **remoto ID** deve ser o mesmo que o endereço do servidor (FQDN do Gateway).
4. O **Local ID** pode estar vazio.
5. Clique nas **definição de autenticação** botão e certifique-se de que "Nomedeutilizador" está selecionado na lista pendente.

  ![Definições de autenticação](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Certifique-se de que as credenciais corretas são introduzidas.

## <a name="additional"></a>Passos adicionais

Se tentar os passos anteriores e tudo o que está corretamente configurado, transfira [Wireshark](https://www.wireshark.org/#download) e efetuar uma captura de pacotes.

1. Filtre *isakmp* e ver o **IKE_SA** pacotes. Deve ser capaz de examinar os detalhes da proposta de SA sob o **Payload: associação de segurança**. 
2. Certifique-se de que o cliente e o servidor tem um conjunto comum.

  ![pacote](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg)

## <a name="next-steps"></a>Passos Seguintes
Para obter mais ajuda, consulte [Support da Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
