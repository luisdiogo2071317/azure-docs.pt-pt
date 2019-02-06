---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 72b7bd722bc7634a707e10cbcd45768140ed527a
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55757611"
---
| Recurso | Partilhas de ficheiros padrão | Partilhas de ficheiros de Premium (pré-visualização) |
|----------|---------------|------------------------------------------|
| Tamanho mínimo de uma partilha de ficheiros | (nenhum valor mínimo; opção pay as you go) | 100 GiB |
| Tamanho máx. de uma partilha de ficheiros | 5 TiB | 5 TiB |
| Tamanho máx. de um ficheiro numa partilha de ficheiros | 1 TiB | 1 TiB |
| Número máx. de ficheiros numa partilha de ficheiros | Sem limite | Sem limite |
| IOPS máx por partilha | 1000 IOPS | Linha de base IOPS de 5120<br />15.360 IOPS com rajada |
| Número máx. de políticas de acesso armazenadas por partilha de ficheiros | 5 | 5 |
| Taxa de transferência de destino para partilha de ficheiros único | Até 60 MiB/segundo | Até 612 MiB/seg (aprovisionado) |
| Máximos identificadores abertos por ficheiro | 2.000 identificadores abertos | 2.000 identificadores abertos |
| Número máximo de instantâneos de partilha | instantâneos de partilha de 200 | instantâneos de partilha de 200 |
| Comprimento do nome do objeto máximo (diretórios e arquivos) | 2048 carateres | 2048 carateres |
| Componente de nome do caminho máximo (no caminho \A\B\C\D, cada letra é um componente) | 255 carateres | 255 carateres |