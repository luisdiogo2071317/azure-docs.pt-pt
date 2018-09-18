---
title: Com o portal de administrador no Azure Stack | Documentos da Microsoft
description: Como um operador do Azure Stack, saiba como utilizar o portal de administrador.
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
ms.date: 09/12/2018
ms.author: mabrigg
ms.openlocfilehash: 058e1f9abbf094fb49a21191f60195df53615451
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985299"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Com o portal de administrador no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Existem dois portais no Azure Stack; o portal de administrador e o portal de utilizador (por vezes referido como o *inquilino* portal.) Como um operador do Azure Stack, pode utilizar o portal de administrador para operações do Azure Stack e o gerenciamento diários.

## <a name="access-the-administrator-portal"></a>Aceder ao portal de administrador

Para um ambiente do kit de desenvolvimento, precisa primeiro certifique-se de que o utilizador pode [ligar para o anfitrião do kit de desenvolvimento](azure-stack-connect-azure-stack.md) por meio de conexão de área de trabalho remoto ou uma rede privada virtual (VPN).

Para aceder ao portal de administrador, navegue para o URL do portal e a de início de sessão com as credenciais de um operador do Azure Stack. Para um sistema integrado, o portal que URL varia com base no nome da região e externo nome de domínio completamente qualificado (FQDN) da sua implementação do Azure Stack.

| Ambiente | URL do Portal de administrador |   
| -- | -- | 
| Kit de desenvolvimento| https://adminportal.local.azurestack.external  |
| Sistemas integrados | https://adminportal.&lt; *região*&gt;.&lt; *FQDN*&gt; | 
| | |

 ![O portal de administrador](media/azure-stack-manage-portals/admin-portal.png)

No portal do administrador, pode fazer coisas como:

* Gerir a infraestrutura (incluindo o estado de funcionamento do sistema, atualizações, capacidade, etc.)
* Povoar o marketplace
* Criar subscrições para os utilizadores
* Criar planos e ofertas

O **tutorial de início rápido** mosaico fornece ligações para documentação online para as tarefas mais comuns.

Embora tenha um operador pode criar recursos, como máquinas virtuais, redes virtuais e contas de armazenamento no portal do administrador, deve [inicie sessão no portal de utilizador](user/azure-stack-use-portal.md) para criar e testar recursos.

>[!NOTE]
>O **criar uma máquina virtual** link no mosaico do tutorial de início rápido faz que crie uma máquina virtual no portal do administrador, mas isso apenas se destina a validar o Azure Stack depois de ser implantado pela primeira vez.

## <a name="understand-subscription-behavior"></a>Compreender o comportamento de subscrição

Há apenas uma subscrição disponível para utilização a partir do portal do administrador. Esta subscrição está a *subscrição do fornecedor predefinido*. Não é possível adicionar quaisquer outras subscrições e utilizá-los no portal do administrador.

Como um operador do Azure Stack, pode adicionar subscrições para os seus utilizadores (incluindo a próprio) do portal do administrador. Os utilizadores (incluindo a próprio) podem acessar e usar estas subscrições do **utilizador** portal. No entanto, o portal de utilizador não fornece acesso a qualquer um dos recursos administrativos e operacionais do portal do administrador.

Os portais de administrador e usuário são apoiados por instâncias separadas do Gestor de recursos do Azure. Devido a essa separação de Gestor de recursos, as subscrições não ultrapassam portais. Por exemplo, se, enquanto um operador do Azure Stack, iniciar sessão no portal de utilizador, não é possível aceder a *subscrição do fornecedor predefinido*. Embora não tiver acesso a quaisquer funções administrativas, pode criar subscrições para mesmo de ofertas de públicas disponíveis. Enquanto tem sessão iniciada portal de utilizador são considerados um utilizador do inquilino.

  >[!NOTE]
  >No ambiente do kit de desenvolvimento, se um utilizador pertencer ao mesmo diretório do inquilino operador do Azure Stack, eles não são impedidos de iniciar sessão no portal do administrador. No entanto, eles não é possível acessar qualquer uma das funções administrativas. Além disso, no portal do administrador, eles não podem adicionar subscrições ou de acesso oferece que estejam disponíveis no portal de utilizador.

## <a name="administrator-portal-tips"></a>Dicas de portais de administrador

### <a name="customize-the-dashboard"></a>Personalizar o dashboard

O dashboard contém um conjunto de mosaicos predefinidos. Pode selecionar **editar dashboard** para modificar o dashboard predefinido ou selecione **novo dashboard** para adicionar um dashboard personalizado. Pode adicionar facilmente mosaicos num dashboard. Por exemplo, pode selecionar **+ criar um recurso**, clique com botão direito **ofertas + planos**e, em seguida, selecione **afixar ao dashboard**.

Às vezes, poderá ver um dashboard em branco no portal. Para recuperar o dashboard, clique em **editar Dashboard**e, em seguida, clique com o botão direito do rato e selecione **repor para o estado predefinido**.

### <a name="quick-access-to-online-documentation"></a>Acesso rápido a documentação online

Para acessar a documentação de operador do Azure Stack, utilize a ajuda e suporte de ícone (ponto de interrogação) no canto superior direito do portal do administrador. Mover o cursor para o ícone e, em seguida, selecione **ajuda + suporte**.

### <a name="quick-access-to-help-and-support"></a>Acesso rápido a ajuda e suporte

Se selecionar o ícone de ajuda e suporte (ponto de interrogação) no canto superior direito do portal do administrador e, em seguida, selecione **novo pedido de suporte**, um dos seguintes resultados acontecer:

- Se estiver a utilizar um sistema integrado, esta ação abre um site onde é possível diretamente abrir um pedido de suporte com o Microsoft ao cliente suportar (CSS). Consulte a [onde pode obter suporte](azure-stack-manage-basics.md#where-to-get-support) compreender quando deve passar por meio de suporte da Microsoft ou o suporte de fornecedor de hardware do fabricante de equipamento original (OEM).
- Se estiver usando o kit de desenvolvimento, esta ação abre o site dos Fóruns do Azure Stack diretamente. Esses fóruns são monitorados regularmente. Como o kit de desenvolvimento é um ambiente de avaliação, não há nenhum oficial de suporte oferecido por meio da CSS do Microsoft.

## <a name="next-steps"></a>Passos Seguintes

- [Gestão da região no Azure Stack](azure-stack-region-management.md)
