---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3b8049515f753cbcf8ca068c1790f716f02d30b6
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
### <a name="noconnection"></a>Para modificar prefixos de endereço de IP do gateway de rede local - sem ligação de gateway

#### <a name="to-add-additional-address-prefixes"></a>Para adicionar prefixos de endereços adicionais:

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **configuração**.
2. Adicionar espaço de endereço IP no *adicionar intervalo de endereços adicionais* caixa.
3. Clique em **guardar** para guardar as definições.

#### <a name="to-remove-address-prefixes"></a>Para remover um prefixos de endereço:

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **configuração**.
2. Clique em de **'...'** na linha que contém o prefixo que pretende remover.
3. Clique em **remover**.
4. Clique em **guardar** para guardar as definições.

### <a name="withconnection"></a>Para modificar prefixos de endereço de IP do gateway de rede local - ligação de gateway existente

Se tiver uma ligação de gateway e pretender adicionar ou remover os prefixos de endereço IP contidos no gateway de rede local, tem de realizar os seguintes passos por ordem. Este procedimento resulta num período de indisponibilidade da ligação VPN. Ao modificar prefixos de endereços IP, não precisa de eliminar o gateway de VPN. Só tem de remover a ligação.

#### <a name="1-remove-the-connection"></a>1. Remova a ligação.

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **ligações**.
2. Clique em de **...**  na linha para cada ligação, em seguida, clique em **eliminar**.
3. Clique em **guardar** para guardar as definições.

#### <a name="2-modify-the-address-prefixes"></a>2. Modifique os prefixos de endereço.

Para adicionar prefixos de endereços adicionais:

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **configuração**.
2. Adicione o espaço de endereços IP.
3. Clique em **guardar** para guardar as definições.

Para remover um prefixos de endereço:

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **configuração**.
2. Clique em de **...**  na linha que contém o prefixo que pretende remover.
3. Clique em **remover**.
4. Clique em **guardar** para guardar as definições.

#### <a name="3-recreate-the-connection"></a>3. Recrie a ligação.

1. Navegue para o Gateway de rede Virtual para a sua VNet. (Não o Gateway de rede Local.)
2. No Gateway de rede Virtual, no **definições** secção, clique em **ligações**.
3. Clique em de **+ adicionar** para abrir o **adicionar ligação** painel.
4. Recrie a ligação.
5. Clique em **OK** para criar a ligação.