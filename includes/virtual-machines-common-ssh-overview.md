---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/08/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: cff3d7bfb89d5b03f986da32edc148efcfb7e7bd
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51506384"
---
## <a name="overview-of-ssh-and-keys"></a>Descrição geral de SSH e chaves

O SSH é um protocolo de conexão criptografada que permite inícios de sessão seguros através de ligações não seguras. O SSH é o protocolo de ligação predefinido para VMs do Linux alojados no Azure. Embora o SSH em si fornece uma ligação encriptada, utilizar palavras-passe com ligações SSH ainda deixa a VM vulnerável a força bruta ataques ou a adivinhação de palavras-passe. Um método mais seguro e preferencial de ligação a uma VM com SSH é utilizar um par de chaves públicas-privadas, também conhecido como *chaves SSH*. 

* O *chave pública* é colocada na sua VM do Linux, ou qualquer outro serviço que deseja usar com a criptografia de chave pública.

* O *chave privada* o sistema local é utilizado por um cliente SSH para verificar a sua identidade quando se liga a sua VM do Linux. Proteja esta chave privada. Não a partilhe.

Dependendo das políticas de segurança da sua organização, pode reutilizar um único par de chaves públicas-privadas para aceder a várias VMs do Azure e serviços. Não é necessário um separado par de chaves para cada VM ou serviço que pretende aceder. 

A chave pública pode ser partilhada com ninguém, mas apenas (ou a infraestrutura de segurança local) deve possuir a sua chave privada.