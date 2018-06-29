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
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: cca9befb65abed3b6d07a52aa286c43725577121
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051751"
---
# <a name="azure-data-factory---naming-rules"></a>O Azure Data Factory - as regras de nomenclatura
A tabela seguinte fornece regras de nomenclatura artefactos do Data Factory de dados.

| Nome | Exclusividade de nome | Verificações de validação |
|:--- |:--- |:--- |
| Data Factory |Exclusivo em todo o Microsoft Azure. Os nomes são sensível, ou seja, `MyDF` e `mydf` Consulte para a mesma fábrica de dados. |<ul><li>Cada fábrica de dados está associada ao exatamente uma subscrição do Azure.</li><li>Nomes de objeto tem de começar com uma letra ou um número e só podem conter letras, números e o caráter de travessão (-).</li><li>Cada caráter de travessão (-) tem de ser imediatamente precedido e seguido por uma letra ou um número. Traços consecutivos não são permitidos em nomes de contentor.</li><li>Nome pode ter entre 3 e 63 carateres.</li></ul> |
| Os serviços ligados/tabelas/Pipelines |Exclusivo com uma fábrica de dados. Os nomes são sensível. |<ul><li>Número máximo de carateres num nome de tabela: a 260.</li><li>Nomes de objeto tem de começar com uma letra, o número ou um caráter de sublinhado (_).</li><li>Os seguintes carateres não são permitidos: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li><li>Traços ("-") não são permitidos em nomes de serviços ligados e de conjuntos de dados apenas.</li></ul>  |
| Grupo de Recursos |Exclusivo em todo o Microsoft Azure. Os nomes são sensível. | Para obter mais informações, consulte [as regras de nomenclatura do Azure e restrições](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). |

## <a name="next-steps"></a>Passos Seguintes
Saiba como criar fábricas de dados ao seguir as instruções passo a passo em [início rápido: criar uma fábrica de dados](quickstart-create-data-factory-powershell.md) artigo. 
