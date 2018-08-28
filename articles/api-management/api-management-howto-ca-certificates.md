---
title: Adicionar um certificado de AC personalizado - gestão de API do Azure | Documentos da Microsoft
description: Saiba como adicionar um certificado de AC personalizado na gestão de API do Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: 9d3399ba6ee724d91117486744ad1431f53edbce
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053448"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Como adicionar um certificado de AC personalizado na gestão de API do Azure

Gestão de API do Azure permite a instalação de certificados de AC no computador dentro de raiz fidedigna e arquivos de certificados intermédios. Esta funcionalidade deve ser utilizada se os seus serviços necessitam de um certificado de AC personalizado.

O artigo mostra como gerir certificados de AC de uma instância de serviço de gestão de API do Azure no portal do Azure.

## <a name="step1"> </a>Carregar um certificado de AC

![Adicionar certificados de AC](media/api-management-howto-ca-certificates/00.png)

Siga os passos abaixo para carregar um novo certificado de AC. Se ainda não criou uma instância de serviço de gestão de API, veja o tutorial [criar uma instância de serviço de gestão de API](get-started-create-service-instance.md).

1. Navegue até à sua instância do serviço de gestão de API do Azure no portal do Azure.

2. Selecione **certificados de AC** no menu.

3. Clique nas **+ adicionar** botão.  

    ![Adicionar certificados de AC](media/api-management-howto-ca-certificates/01.png)  

4. Navegue para o certificado e decidir qual o arquivo de certificados. Apenas a chave pública é necessária, pelo que a palavra-passe não é necessária.

    ![Adicionar certificados de AC](media/api-management-howto-ca-certificates/02.png)  

5. Clique em **Guardar**. Esta operação poderá demorar alguns minutos.

    ![Adicionar certificados de AC](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Pode carregar um certificado de AC com o `New-AzureRmApiManagementSystemCertificate` comando do Powershell.

## <a name="step1a"> </a>Eliminar um certificado de cliente

Para eliminar um certificado, clique no menu de contexto **...**  e selecione **eliminar** junto ao certificado.

![Eliminar certificados de AC](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a