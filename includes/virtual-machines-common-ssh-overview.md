---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 5e199771c39ba2fbdeabbd04ed4081a9cd3ea117
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36964523"
---
## <a name="overview-of-ssh-and-keys"></a>Descrição geral de SSH e chaves

SSH é um protocolo de ligação encriptada que permite que os inícios de sessão seguros através de ligações não segura. É o protocolo de ligação predefinido para VMs com Linux alojados no Azure. Apesar de SSH próprio fornece uma ligação encriptada, utilizar as palavras-passe com ligações SSH ainda deixa a VM vulneráveis a força bruta ataques ou deteção de palavras-passe. Um método mais seguro e preferencial de ligação a uma VM com o SSH é através da utilização de um par de chaves públicas-privadas, também conhecido como as chaves SSH. 

* O *chave pública* é colocada na VM com Linux ou qualquer outro serviço que pretende utilizar com a criptografia de chave pública.

* O *chave privada* é o que lhe está presente na VM com Linux quando efetuar uma ligação de SSH, para verificar a sua identidade. Proteja esta chave privada. Não a partilhe.

Consoante as políticas de segurança da sua organização, pode reutilizar um par de chaves públicas-privadas único para aceder aos vários serviços e as VMs do Azure. Não é necessário um par de chaves separado para cada VM ou serviço que pretende aceder. 

A sua chave pública pode ser partilhada com qualquer pessoa; mas apenas o utilizador (ou a infraestrutura de segurança local) possui a chave privada.