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
ms.openlocfilehash: 0363b719e2587226282257e19e58e3c4e55c2219
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47454472"
---
## <a name="overview-of-ssh-and-keys"></a>Descrição geral de SSH e chaves

O SSH é um protocolo de conexão criptografada que permite inícios de sessão seguros através de ligações não seguras. O SSH é o protocolo de ligação predefinido para VMs do Linux alojados no Azure. Embora o SSH em si fornece uma ligação encriptada, utilizar palavras-passe com ligações SSH ainda deixa a VM vulnerável a força bruta ataques ou a adivinhação de palavras-passe. Um método mais seguro e preferencial de ligação a uma VM com SSH é utilizar um par de chaves públicas-privadas, também conhecido como *chaves SSH*. 

* O *chave pública* é colocada na sua VM do Linux, ou qualquer outro serviço que deseja usar com a criptografia de chave pública.

* O *chave privada* é o que apresenta a sua VM do Linux quando efetuar uma ligação de SSH, para verificar a sua identidade. Proteja esta chave privada. Não a partilhe.

Dependendo das políticas de segurança da sua organização, pode reutilizar um único par de chaves públicas-privadas para aceder a várias VMs do Azure e serviços. Não é necessário um separado par de chaves para cada VM ou serviço que pretende aceder. 

A chave pública pode ser partilhada com ninguém, mas apenas (ou a infraestrutura de segurança local) deve possuir a sua chave privada.