---
title: Gerir contas de armazenamento do Azure pilha | Microsoft Docs
description: Saiba como localizar, gerir, recuperar e recuperar as contas de armazenamento de pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 2ae2b628b2e61893a5289151c3b405e7412e7d13
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076937"
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Gerir contas de armazenamento na pilha do Azure
Saiba como gerir contas de armazenamento na pilha do Azure para localizar, recuperar e recuperar a capacidade de armazenamento com base nas necessidades de negócio.

## <a name="find"></a>Localizar uma conta de armazenamento
A lista de contas do storage na região pode ser visualizada na pilha do Azure por:

1. Num browser da Internet, navegue para https://adminportal.local.azurestack.external.
2. Inicie sessão no portal de administração do Azure pilha como um operador da nuvem (utilizando as credenciais fornecidas durante a implementação)
3. Encontrar no dashboard predefinido – o **gestão região** lista e selecione a região que pretende explorar, por exemplo **(local**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Selecione **armazenamento** do **fornecedores de recursos** lista.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Agora, no painel de administrador do fornecedor de recursos de armazenamento – desloque para baixo até o **contas do Storage** separador e selecione-o.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   A página resultante é a lista de contas do storage nessa região.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Por predefinição, são apresentadas as primeiras 10 contas. Pode optar por obter mais informações, clicando a **carregar mais** ligação na parte inferior da lista.

OU

Se estiver interessado numa conta do storage específico – pode **filtrar e obter as contas relevantes** apenas.


**Para filtrar para contas:**

1. Selecione **filtro** na parte superior do painel.
2. No painel de filtro, permite-lhe especificar **nome da conta**, * * ID de subscrição, ou **estado** para otimizar a lista de contas do storage a apresentar. Utilize-os conforme apropriado.
3. Selecione **atualização**. A lista deverá atualizar em conformidade.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Para repor o filtro: selecione **filtro**, desmarque terminar as seleções e atualizar.

Caixa de texto de pesquisa (no topo do painel de lista de contas de armazenamento) permite-lhe realce o texto seleccionado na lista de contas. Pode utilizar este quando o ID de nome completo ou não está facilmente disponível.

Pode utilizar aqui o texto livre para ajudar a localizar a conta que está interessado.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Ver detalhes da conta
Depois de localizar as contas que está interessado visualização, pode selecionar a conta específica para ver certos detalhes. Um novo painel abre-se com os detalhes da conta, tais como: o tipo da conta, a hora de criação, localização, etc.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Recuperar uma conta eliminada
Pode ser uma situação em que precisar de recuperar de uma conta eliminada.

Na pilha de Azure há uma forma simple para o fazer:

1. Navegue para a lista de contas de armazenamento. Consulte [localizar uma conta de armazenamento](#find) neste tópico para obter mais informações.
2. Localize essa conta específica na lista. Se pretender filtrar.
3. Verifique o *estado* da conta. Deverá indicar **eliminado**.
4. Selecione a conta, que abre o painel de detalhes de conta.
5. Por cima neste painel, localize o **recuperar** botão e selecioná-lo.
6. Selecione **Sim** para confirmar.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. A recuperação está agora em *... processar espera* para uma indicação de que foi efetuada com êxito.
   Também pode selecionar o ícone de "sino" na parte superior do portal para ver indicações de progresso.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Assim que a conta recuperada está sincronizada com êxito, podem ser utilizada novamente.

### <a name="some-gotchas"></a>Alguns Gotchas
* A conta eliminada mostra o estado como **fora de retenção**.
  
  Fora de retenção significa que a conta eliminada excedeu o período de retenção e não pode ser recuperável.
* A conta eliminada não for apresentada na lista de contas.
  
  A conta não pode mostrar na lista de conta quando a conta eliminada já tiver sido recolhidos. Neste caso, este não pode ser recuperado. Consulte [recuperar a capacidade](#reclaim) neste tópico.

## <a name="set-the-retention-period"></a>Definir o período de retenção
A definição de período de retenção permite um operador da nuvem especificar um período de tempo em dias (entre 0 e 9999 dias) durante o qual qualquer conta eliminada, potencialmente, pode ser recuperada. O período de retenção predefinido está definido para 0 dias. Definir o valor para "0" significa que qualquer conta eliminada imediatamente está fora de retenção e marcado para recolha de lixo periódica.

**Para alterar o período de retenção:**

1. Num browser da internet, navegue para https://adminportal.local.azurestack.external.
2. Inicie sessão no portal de administração do Azure pilha como um operador da nuvem (utilizando as credenciais fornecidas durante a implementação)
3. Encontrar no dashboard predefinido – o **gestão região** lista e selecione a região que pretende explorar – por exemplo **(local**).
4. Selecione **armazenamento** do **fornecedores de recursos** lista.
5. Selecione **definições** na parte superior para abrir o painel definição.
6. Selecione **configuração** , em seguida, edite o valor de período de retenção.

   Definir o número de dias e guarde-o.
   
   Este valor é imediatamente em vigor e está definido para a região de toda.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Recuperar a capacidade
Uma dos efeitos de lado de ter um período de retenção é que uma conta eliminada continua a capacidade de consumir até que se trata de fora do período de retenção. Como um operador da nuvem poderá necessitar de uma forma de recuperar o espaço de conta eliminado apesar do período de retenção ainda não expirou.

Pode recuperar a capacidade de utilizar o portal ou PowerShell.

**Para recuperar a capacidade através do portal:**
1. Navegue para o painel de contas de armazenamento. Consulte [localizar uma conta de armazenamento](#find).
2. Selecione **recuperar espaço** na parte superior do painel.
3. Ler a mensagem e, em seguida, selecione **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Aguarde que a notificação de sucesso Consulte no ícone de campainha no portal.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Atualize a página de contas de armazenamento. As contas eliminadas já não são apresentadas na lista porque a serem foram removidas.

Pode também utilizar o PowerShell para substituir explicitamente o período de retenção e recuperar imediatamente a capacidade.

**Para recuperar a capacidade através do PowerShell:**   

1. Certifique-se de que tem o Azure PowerShell instalada e configurada. Caso contrário, utilize as instruções seguintes: 
   * Para instalar a versão mais recente do Azure PowerShell e associá-lo à sua subscrição do Azure, consulte [como instalar e configurar o Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Para obter mais informações sobre cmdlets do Azure Resource Manager, consulte [utilizar o Azure PowerShell com o Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Execute os seguintes cmdlets:

> [!NOTE]
> Se executar estes cmdlets, pode eliminar permanentemente a conta e o respetivo conteúdo. Não é recuperável. Utilize esta opção com cuidado.

```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
````

Para obter mais informações, consulte [documentação do PowerShell de pilha do Azure.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="next-steps"></a>Passos Seguintes

 - Para obter informações sobre a gestão de permissões, consulte [controlo de acesso de Manage Role-Based](azure-stack-manage-permissions.md).
 - Para obter informações sobre a capacidade de pilha do Azure gerir armazenamento, consulte [gerir a capacidade de pilha do Azure armazenamento](azure-stack-manage-storage-shares.md).