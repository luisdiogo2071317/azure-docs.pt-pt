---
title: Azure AD v2 Windows ambiente de trabalho ao obter iniciada - Config | Microsoft Docs
description: "Como uma aplicação .NET de ambiente de trabalho do Windows (XAML) pode obter um token de acesso e chamar uma API protegida pelo ponto final do Azure Active Directory v2."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 922fd0e24334eb45995b66dfaaa49d3d4f835e9a
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação para a sua aplicação
Neste passo, terá de adicionar o Id da aplicação ao seu projeto.

1.  Abra `App.xaml.cs` e substituir a linha que contém o `ClientId` com:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>O que é o seguinte

[!INCLUDE  [Test and Validate](..\..\..\..\includes\active-directory-develop-guidedsetup-windesktop-test.md)]
