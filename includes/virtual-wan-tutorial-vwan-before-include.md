---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 09f642a4c96781276d225cba37d71386d429d0c9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004185"
---
Antes de iniciar a configuração, verifique se cumpre os seguintes critérios:

* Se já tem uma rede virtual e quer ligar-se à mesma, confirme que nenhuma das sub-redes da sua rede no local se sobrepõe às redes virtuais às quais se quer ligar. A rede virtual não precisa de uma sub-rede de gateway e não pode ter nenhum gateway de rede virtual. Se não tiver uma rede virtual, pode utilizar os passos neste artigo para criar uma.
* Obtenha um intervalo de endereços IP para a região do seu hub. O hub é uma rede virtual e o intervalo de endereços que especificar para a região do mesmo não se pode sobrepor a nenhuma das redes virtuais já existentes a que se queira ligar. Também não se pode sobrepor aos intervalos de endereços a que se ligue no local. Se não estiver familiarizado com os intervalos de endereços IP localizados na configuração de rede no local, tem de se coordenar com alguém que consiga fornecer esses detalhes.
* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.