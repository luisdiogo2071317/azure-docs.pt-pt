---
title: Resolver problemas de ligações de VPN do Azure ponto a Site a partir de clientes Mac OS X | Microsoft Docs
description: Passos para resolver problemas de ligações de cliente de VPN do P2S Mac OS X
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
ms.openlocfilehash: 1cf8195cbf65f27c71a4db18c0c61c8a25673acd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Resolver problemas de ligações VPN ponto a Site a partir de clientes de VPN do Mac OS X

Este artigo ajuda-o a resolver problemas de conetividade de ponto a Site do Mac OS X com o cliente VPN nativo e IKEv2. O cliente VPN no Mac para IKEv2 é muito básico e não permite a muito personalização. Existem definições apenas quatro que precisem de ser marcada:

* Endereço do Servidor
* ID remoto
* ID de local
* Definições de autenticação
* Versão do SO (10.11 ou superior)


## <a name="VPNClient"></a> Resolver problemas de autenticação baseada em certificado
1. Verifique as definições de cliente VPN. Vá para o **a definição de rede** premindo comando + Shift e, em seguida, escreva "VPN" para verificar as definições de cliente VPN. Na lista, clique na entrada VPN que tem de ser investigado.

  ![Autenticação baseada em certificado IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Certifique-se de que o **endereço do servidor** é o FQDN completo e inclui o cloudapp.net.
3. O **ID remoto** deve ser o mesmo que o endereço do servidor (FQDN do Gateway).
4. O **Local ID** deve ser o mesmo que o **requerente** do certificado de cliente.
5. Clique em **definições de autenticação** para abrir a página de definições de autenticação.

  ![Definições de autenticação](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Certifique-se de que **certificado** está selecionado na lista pendente.
7. Clique em de **selecione** botão e certifique-se de que o certificado correto está selecionado. Clique em **OK** para guardar as alterações.

## <a name="ikev2"></a>Resolver problemas de autenticação de nome de utilizador e palavra-passe

1. Verifique as definições de cliente VPN. Vá para o **a definição de rede** premindo comando + Shift e, em seguida, escreva "VPN" para verificar as definições de cliente VPN. Na lista, clique na entrada VPN que tem de ser investigado.

  ![Palavra-passe de nome de utilizador de IKEv2](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Certifique-se de que o **endereço do servidor** é o FQDN completo e inclui o cloudapp.net.
3. O **ID remoto** deve ser o mesmo que o endereço do servidor (FQDN do Gateway).
4. O **Local ID** pode estar vazio.
5. Clique em de **definição de autenticação** botão e certifique-se de que "Nomedeutilizador" está selecionado na lista pendente.

  ![Definições de autenticação](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Certifique-se de que as credenciais corretas são introduzidas.

## <a name="additional"></a>Passos adicionais

Se repita os passos anteriores e tudo está corretamente configurado, transfira [Wireshark](https://www.wireshark.org/#download) e efetue uma captura de pacotes.

1. Filtrar *iskmp* e observe o **IKE_SA** pacotes. Deve ser capaz de ver os detalhes da proposta de SA sob o **Payload: associação de segurança**. 
2. Certifique-se de que o cliente e o servidor tem um conjunto comum.

  ![Pacote](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg)

## <a name="next-steps"></a>Passos Seguintes
Para obter ajuda adicional, consulte [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
