---
title: O Azure Service Fabric service model XML simples tipos de esquema | Documentos da Microsoft
description: Descreve os tipos simples no esquema XML do modelo de serviço do Service Fabric.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/10/2018
ms.author: ryanwi
ms.openlocfilehash: 8ebdeb1e25f190fc89b88a9b049c14932130b957
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258802"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>Tipos de esquema XML modelo do serviço simples

## <a name="fabricuri-simpletype"></a>FabricUri simpleType
Um URI que é utilizado como um identificador estável dos serviços pelo sistema de nomenclatura do Microsoft Azure Service Fabric. 

### <a name="xml-source"></a>Origem XML
```xml
<xs:simpleType xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
