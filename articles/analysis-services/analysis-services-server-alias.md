---
title: Nomes de servidor de alias Analysis Services do Azure | Microsoft Docs
description: Descreve como criar e utilizar aliases de nome de servidor.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a947dde1551c653faa54f088c1712c41c7657aa0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="alias-server-names"></a>Nomes de alias de servidor

Ao utilizar um alias de nome de servidor, os utilizadores podem ligar ao seu servidor de Analysis Services do Azure com um mais curto *alias* em vez do nome de servidor. Ao ligar a partir de uma aplicação de cliente, o alias é especificado como um ponto final utilizando a **ligação: / /** formato de protocolo. O ponto final, em seguida, devolve o nome do servidor real para se ligar.

Nomes de alias de servidor são ideais para:

- Modelos de migrar entre servidores sem afetar os utilizadores. 
- Os nomes dos servidores amigável são mais fácil para os utilizadores memorizem a. 
- Utilizadores diretos em servidores diferentes em alturas diferentes do dia. 
- Utilizadores diretos em regiões diferentes para as instâncias que sejam geograficamente próximo, como quando utilizar o Gestor de tráfego do Azure. 

Qualquer ponto final de HTTP que devolve um nome de servidor válido do Azure Analysis Services pode servir como um alias.

![Alias utilizando o formato de ligação](media/analysis-services-alias/aas-alias-browser.png)

Ao ligar-se de um cliente, o nome do servidor de alias é introduzido utilizando **ligação: / /** formato de protocolo. Por exemplo, no ambiente de trabalho do Power BI:

![Ligação do Power BI Desktop](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Criar um alias

Para criar um ponto final de alias, pode utilizar qualquer método que devolva um nome de servidor de Analysis Services do Azure válido. Por exemplo, uma referência a um ficheiro no armazenamento de Blobs do Azure que contém o servidor real nome, ou para criar e publicar uma aplicação de formulários Web ASP.NET.

Neste exemplo, é criada uma aplicação de formulários Web ASP.NET no Visual Studio. O controlo de utilizador e de referência de página mestra são removidas da página Default.aspx. O conteúdo do Default.aspx é simplesmente a diretiva de página seguinte:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

O evento de Page_Load no default.aspx utiliza o método Response.Write() para devolver o nome do servidor de Analysis Services do Azure.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Consultar também

[Bibliotecas de cliente](analysis-services-data-providers.md)   
[Ligar a partir do ambiente de trabalho do Power BI](analysis-services-connect-pbi.md)
