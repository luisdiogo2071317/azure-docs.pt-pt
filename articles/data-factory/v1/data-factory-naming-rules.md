---
title: Regras para atribuição de nomes de entidades do Azure Data Factory | Microsoft Docs
description: Descreve as regras de nomenclatura para entidades do Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: eb4579c2d94ae610d21b7221c0b410b07268d419
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051805"
---
# <a name="azure-data-factory---naming-rules"></a>O Azure Data Factory - as regras de nomenclatura
> [!NOTE]
> Este artigo aplica-se a versão 1 do Data Factory. Se estiver a utilizar a versão atual do serviço Data Factory, consulte o artigo [nomenclatura regras no Data Factory](../naming-rules.md).

A tabela seguinte fornece regras de nomenclatura artefactos do Data Factory de dados.

| Nome | Exclusividade de nome | Verificações de validação |
|:--- |:--- |:--- |
| Data Factory |Exclusivo em todo o Microsoft Azure. Os nomes são sensível, ou seja, `MyDF` e `mydf` Consulte para a mesma fábrica de dados. |<ul><li>Cada fábrica de dados está associada ao exatamente uma subscrição do Azure.</li><li>Nomes de objeto tem de começar com uma letra ou um número e só podem conter letras, números e o caráter de travessão (-).</li><li>Cada caráter de travessão (-) tem de ser imediatamente precedido e seguido por uma letra ou um número. Traços consecutivos não são permitidos em nomes de contentor.</li><li>Nome pode ter entre 3 e 63 carateres.</li></ul> |
| Os serviços ligados/tabelas/Pipelines |Exclusivo com uma fábrica de dados. Os nomes são sensível. |<ul><li>Número máximo de carateres num nome de tabela: a 260.</li><li>Nomes de objeto tem de começar com uma letra, o número ou um caráter de sublinhado (_).</li><li>Os seguintes carateres não são permitidos: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Grupo de Recursos |Exclusivo em todo o Microsoft Azure. Os nomes são sensível. |<ul><li>Número máximo de carateres: 1000.</li><li>O nome pode conter letras, dígitos e os seguintes carateres: "-", "_",","e"."</li></ul> |

