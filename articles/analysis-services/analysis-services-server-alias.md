---
title: Nomes de servidor de alias Analysis Services do Azure | Microsoft Docs
description: Descreve como criar e utilizar aliases de nome de servidor.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c563eb97e093924076684dcf40ac3a8fe5f96880
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596758"
---
# <a name="alias-server-names"></a>Nomes de alias de servidor

Ao utilizar um alias de nome de servidor, os utilizadores podem ligar ao seu servidor de Analysis Services do Azure com um mais curto *alias* em vez do nome de servidor. Ao ligar a partir de uma aplicação de cliente, o alias é especificado como um ponto final utilizando a **ligação: / /** formato de protocolo. O ponto final, em seguida, devolve o nome do servidor real para se ligar.

Nomes de alias de servidor são ideais para:

- Modelos de migrar entre servidores sem afetar os utilizadores. 
- Os nomes dos servidores amigável são mais fácil para os utilizadores memorizem a. 
- Utilizadores diretos em servidores diferentes em alturas diferentes do dia. 
- Utilizadores diretos em regiões diferentes para as instâncias que sejam geograficamente próximo, como quando utilizar o Gestor de tráfego do Azure. 

Qualquer ponto final de HTTPS que devolve um nome de servidor válido do Azure Analysis Services pode servir como um alias. O ponto final tem de suportar HTTPS através da porta 443 e a porta não pode ser especificada no URI.

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

## <a name="see-also"></a>Consulte também

[Bibliotecas de cliente](analysis-services-data-providers.md)   
[Ligar a partir do ambiente de trabalho do Power BI](analysis-services-connect-pbi.md)
