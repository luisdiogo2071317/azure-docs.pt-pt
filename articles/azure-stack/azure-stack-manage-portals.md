---
title: Utilizar o portal de administrador na pilha do Azure | Microsoft Docs
description: Como um operador de pilha do Azure, saiba como utilizar o portal de administrador.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: mabrigg
ms.openlocfilehash: 673b1144fe927e0619f5f8638d7e8ce9a181f48c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248525"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Utilizar o portal de administrador na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Existem dois portais na pilha do Azure; o portal de administrador e o portal de utilizador (por vezes referido como o *inquilino* portal.) Como um operador de pilha do Azure, pode utilizar o portal de administrador para gestão diária e as operações da pilha do Azure.

## <a name="access-the-administrator-portal"></a>Aceder ao portal de administrador

Para um ambiente do kit de desenvolvimento, tem de primeiro certifique-se de que pode [ligar para o anfitrião do kit de desenvolvimento](azure-stack-connect-azure-stack.md) através de ligações de ambiente de trabalho remoto ou através de uma rede privada virtual (VPN).

Para aceder ao portal de administrador, navegue para o URL e o início de sessão utilizando as credenciais de um operador de pilha do Azure portal. Para um sistema integrado, portal que URL varia com base no nome de região e externo nome de domínio completamente qualificado (FQDN) da sua implementação de pilha do Azure.

| Ambiente | URL do Portal de administrador |   
| -- | -- | 
| Kit de desenvolvimento| https://adminportal.local.azurestack.external  |
| Sistemas integrados | https://adminportal.&lt; *região*&gt;.&lt; *FQDN*&gt; | 
| | |

 ![O portal de administrador](media/azure-stack-manage-portals/image1.png)

No portal do administrador, pode fazer coisas como:

* Gerir a infraestrutura (incluindo o estado de funcionamento do sistema, atualizações, capacidade, etc.)
* Povoar o marketplace
* criar subscrições para os utilizadores
* criar planos e ofertas

O **tutorial de início rápido** mosaico fornece ligações para documentação online para as tarefas mais comuns.

Embora tenha de um operador podem criar recursos, tais como máquinas virtuais, redes virtuais e contas de armazenamento no portal do administrador, deve [inicie sessão no portal de utilizador](user/azure-stack-use-portal.md) para criar e testar recursos.

>[!NOTE]
>O **criar uma máquina virtual** ligação no mosaico do tutorial de início rápido tem a criar uma máquina virtual no portal do administrador, mas isto só se destina a validar a pilha do Azure após é implementada pela primeira vez.

## <a name="understand-subscription-behavior"></a>Compreender o comportamento de subscrição

Não há apenas uma subscrição disponível para utilização do portal do administrador. Esta subscrição está a *subscrição do fornecedor predefinido*. Não é possível adicionar quaisquer outras subscrições e utilizá-los no portal do administrador.

Como um operador de pilha do Azure, pode adicionar subscrições para os seus utilizadores (incluindo por si) a partir do portal do administrador. Utilizadores (incluindo por si) podem aceder e utilizar estas subscrições do **utilizador** portal. No entanto, o portal de utilizador não fornece acesso a qualquer uma das funcionalidades administrativas ou operacionais do portal do administrador.

Os portais de administrador e utilizador são apoiados por instâncias separadas do Gestor de recursos do Azure. Devido a esta separação de Gestor de recursos, subscrições não cruzada portais. Por exemplo, se, como um operador de pilha do Azure, iniciar sessão portal de utilizador, não é possível aceder a *subscrição do fornecedor predefinido*. Apesar de não tiver acesso a quaisquer funções administrativas, pode criar subscrições para si próprio de ofertas públicas disponíveis. Enquanto tem sessão iniciada portal de utilizador são consideradas um utilizador de inquilino.

  >[!NOTE]
  >No ambiente do kit de desenvolvimento, se um utilizador pertencer ao mesmo diretório do inquilino o operador de pilha do Azure, estes não estão bloqueadas de iniciar sessão portal do administrador. No entanto, não vão conseguir aceder qualquer uma das funções administrativas. Além disso, no portal de administrador, eles não é possível adicionar subscrições ou acesso oferece que estão disponíveis no portal de utilizador.

## <a name="administrator-portal-tips"></a>Sugestões de portais de administrador

### <a name="customize-the-dashboard"></a>Personalizar o dashboard

O dashboard contém um conjunto de mosaicos predefinidos. Pode selecionar **editar dashboard** para modificar o dashboard predefinido ou selecione **novo dashboard** para adicionar um dashboard personalizado. Pode facilmente adicionar mosaicos a um dashboard. Por exemplo, pode selecionar **novo**, faça duplo clique **oferece + planos**e, em seguida, selecione **afixar ao dashboard**.

### <a name="quick-access-to-online-documentation"></a>Acesso rápido a documentação online

Para aceder à documentação do operador de pilha do Azure, a ajuda e suporte (ponto de interrogação) do ícone no canto superior direito do portal do administrador. Mova o cursor para o ícone e, em seguida, selecione **ajuda + suporte**.

### <a name="quick-access-to-help-and-support"></a>Acesso rápido a ajuda e suporte

Se selecionar o ícone de ajuda e suporte (ponto de interrogação) no canto superior direito do portal de administrador e, em seguida, selecione **novo pedido de suporte**, um dos resultados seguintes ocorrem:

- Se estiver a utilizar um sistema integrado, esta ação abre-se um site, onde pode diretamente abrir um pedido de suporte com o Microsoft ao cliente suportar (CSS). Consulte [onde obter suporte](azure-stack-manage-basics.md#where-to-get-support) compreender quando deve passar através de suporte da Microsoft ou através do suporte de fornecedor de hardware de fabricante de equipamento original (OEM).
- Se estiver a utilizar o kit de desenvolvimento, esta ação abre-se o site de fóruns do Azure pilha diretamente. Estes fóruns regularmente são monitorizados. Porque o kit de desenvolvimento é um ambiente de avaliação, não há nenhum oficial de suporte oferecida através do Microsoft CSS.

## <a name="next-steps"></a>Passos Seguintes

- [Gestão de região na pilha do Azure](azure-stack-region-management.md)
