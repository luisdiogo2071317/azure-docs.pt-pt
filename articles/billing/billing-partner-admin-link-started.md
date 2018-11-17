---
title: Ligar a conta do Azure para o ID de parceiros | Documentos da Microsoft
description: ID de parceiro de ligação para a conta de utilizador que utiliza para gerir os recursos do cliente para controlar compromissos com os clientes do Azure.
services: billing
author: dhirajgandhi
ms.author: cwatson
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 342e182466d2fe704c5ee188e35dece0abaeea02
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823100"
---
# <a name="link-partner-id-to-your-azure-accounts"></a>ID de parceiro de ligação para suas contas do Azure

Como parceiro, pode controlar o impacto em seu envolvimento com o cliente ao ligar o seu ID de parceiro às contas utilizadas para gerir recursos do cliente.

Esta funcionalidade está disponível em pré-visualização pública.

## <a name="get-access-from-your-customer"></a>Obtenha acesso a partir do seu cliente

Antes de ligar o seu ID de parceiro, seu cliente tem de dar acesso aos seus recursos do Azure ao utilizar uma das seguintes opções:

- **Utilizador convidado:** seu cliente pode adicioná-lo como um utilizador convidado e atribuir a todas as funções RBAC. Para obter mais informações, consulte [adicionar utilizadores convidados a partir de outro diretório](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Conta de diretório:** seu cliente pode criar um novo utilizador da sua organização em seu diretório e atribuir qualquer função RBAC.

- **Principal de serviço:** seu cliente pode adicionar uma aplicação ou script da sua organização em seu diretório e atribuir qualquer função RBAC. A identidade da aplicação ou script é conhecida como principal de serviço.

## <a name="link-partner-id"></a>ID do parceiro de ligação

Quando tem acesso aos recursos do cliente, utilize o portal do Azure, PowerShell ou CLI para ligar o seu ID do Microsoft Partner Network (MPN ID) para o seu ID de utilizador ou principal de serviço. Tem de associar o ID de parceiro em cada inquilino do cliente.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Utilizar o portal do Azure para ligar o novo ID de parceiro

1. Aceda a [link para um ID de parceiro](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) no portal do Azure.

2. Inicie sessão no Portal do Azure.

3. Introduza o ID de parceiro Microsoft. O parceiro ID é o [Microsoft Partner Network (MPN)](https://partner.microsoft.com/) ID da sua organização.

  ![Captura de ecrã que mostra o ID de parceiro de ligação](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Para ligar o ID de parceiro de outro cliente, utilize o alternador de diretório. Sob o diretório de comutador, escolha o seu diretório.

  ![Captura de ecrã que mostra o ID de parceiro de ligação](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>Utilize o PowerShell para ligar o novo ID de parceiro

1. Instalar o [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) módulo do PowerShell.

2. Inicie sessão no inquilino do cliente com a conta de utilizador ou principal de serviço, para obter mais informações, consulte [início de sessão com o Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Ligar o novo ID de parceiro. O parceiro ID é o [Network(MPN) de parceiro Microsoft](https://partner.microsoft.com/) ID da sua organização.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>Obter o ID de parceiro ligadas
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Atualizar o ID de parceiro ligadas
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Eliminar o ID de parceiro ligadas
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Utilize a CLI para ligar o novo ID de parceiro
1.  Instale a extensão da CLI.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Inicie sessão no inquilino do cliente com a conta de utilizador ou principal de serviço. Para obter mais informações, consulte [iniciar sessão com a CLI do Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Ligar o novo ID de parceiro. O parceiro ID é o [Network(MPN) de parceiro Microsoft](https://partner.microsoft.com/) ID da sua organização.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Obter o ID de parceiro ligadas
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Atualizar o ID de parceiro ligadas
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Eliminar o ID de parceiro ligadas
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>Passos Seguintes

Junte-se a discussão no [Comunidade de parceiros da Microsoft](https://aka.ms/PALdiscussion) para receber atualizações ou enviar comentários.

## <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

**Quem pode associar o ID de parceiro?**

Qualquer utilizador da organização parceira que está a gerir recursos do Azure do cliente pode ligar o ID de parceiro na conta.

**Uma vez que está ligado um ID de parceiro pode ela ser alterada?**

Sim, ID de parceiro ligado pode ser alterado, adicionada ou removida.

**E se um utilizador tiver uma conta em vários inquilinos do cliente?**

A ligação entre o ID de parceiro e a conta é feita para cada inquilino do cliente.  Tem de associar o ID de parceiro em cada inquilino do cliente.

**Podem outros parceiros ou clientes, editar ou remover a ligação para o ID de parceiro?**

A ligação está associada ao nível da conta de utilizador. Apenas pode editar ou remover a ligação para o ID de parceiro. O cliente e outro parceiro não é possível alterar a ligação para o ID de parceiro. 
