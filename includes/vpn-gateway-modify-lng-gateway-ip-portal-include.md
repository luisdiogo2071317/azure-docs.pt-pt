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
ms.openlocfilehash: a929149f115d716bf7f9d850abe5ba97bd5a8189
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30198099"
---
### <a name="gwipnoconnection"></a> Para modificar o endereço IP de gateway de rede local - nenhuma ligação de gateway

Utilize o exemplo para modificar um gateway de rede local que não tem uma ligação de gateway. Ao modificar este valor, também pode modificar os prefixos de endereços em simultâneo.

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **configuração**.
2. No **endereço IP** caixa, modifique o endereço IP.
3. Clique em **Guardar** para guardar as definições.

### <a name="gwipwithconnection"></a>Para modificar o endereço IP de gateway da gateway de rede local - existente a ligação de gateway

Para modificar um gateway de rede local que tenha uma ligação, terá de remover primeiro a ligação. Depois de a ligação ser removida, pode modificar o endereço IP do gateway e recriar uma nova ligação. Também pode modificar os prefixos de endereços em simultâneo. Este procedimento resulta num período de indisponibilidade da ligação VPN. Ao modificar os endereços IP de gateway, não precisa de eliminar o gateway de VPN. Só tem de remover a ligação.
 
#### <a name="1-remove-the-connection"></a>1. Remova a ligação.

1. No recurso de Gateway de rede Local, no **definições** secção, clique em **ligações**.
2. Clique em de **...**  na linha para a ligação, em seguida, clique em **eliminar**.
3. Clique em **guardar** para guardar as definições.

#### <a name="2-modify-the-ip-address"></a>2. Modificar o endereço IP.

Também pode modificar os prefixos de endereços em simultâneo.

1. No **endereço IP** caixa, modifique o endereço IP.
2. Clique em **Guardar** para guardar as definições.

#### <a name="3-recreate-the-connection"></a>3. Recrie a ligação.

1. Navegue para o Gateway de rede Virtual para a sua VNet. (Não o Gateway de rede Local.)
2. No Gateway de rede Virtual, no **definições** secção, clique em **ligações**.
3. Clique em de **+ adicionar** para abrir o **adicionar ligação** painel.
4. Recrie a ligação.
5. Clique em **OK** para criar a ligação.