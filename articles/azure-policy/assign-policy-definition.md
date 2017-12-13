---
title: "Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs"
description: "Este artigo explica os passos para criar uma definição de política para identificar recursos incompatíveis."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 85136ff2783b21472ef02aee15f8ec5844a00c12
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure
O primeiro passo para compreender a conformidade no Azure é saber onde está posicionado com os seus próprios recursos atuais. Este guia de introdução acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, terá identificado com êxito as máquinas virtuais que não estão a utilizar discos geridos e são, por conseguinte, *incompatíveis*.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="opt-in-to-azure-policy"></a>Participar no Azure Policy

O Azure Policy está agora disponível em Pré-visualização Pública e tem de se registar para pedir acesso.

1. Aceda ao Azure Policy em https://aka.ms/getpolicy e selecione **Inscrever** no painel esquerdo.

   ![Pesquisar política](media/assign-policy-definition/sign-up.png)

2. Participe no Azure Policy ao selecionar as subscrições na lista de **Subscrições** com que gostaria de trabalhar. Em seguida, selecione **Registar**.

   ![Participar no Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   O seu pedido é aprovado automaticamente para a Pré-visualização. Aguarde até 30 minutos para o sistema processar o registo.

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução iremos criar uma atribuição de política e atribuir as *Máquinas Virtuais de Auditoria sem uma definição de política de Discos Geridos*.

1. Selecione **Atribuições** no painel esquerdo da página de Política do Azure.
2. Selecione **Atribuir Política** a partir da parte superior do painel **Atribuições**.

   ![Atribuir uma definição de política](media/assign-policy-definition/select-assign-policy.png)

3. Na página **Atribuir Política**, clique em ![botão de Definição de política](media/assign-policy-definition/definitions-button.png), junto ao campo **Política**, para abrir a lista de definições disponíveis.

   ![Abrir definições de política disponíveis](media/assign-policy-definition/open-policy-definitions.png)

   O Azure Policy já inclui as definições de política incorporada que pode utilizar. Verá as definições de política incorporada, como:

   - Impor etiqueta e o respetivo valor
   - Aplicar etiqueta e o respetivo valor
   - Requer a Versão do SQL Server 12.0

4. Pesquisa nas definições de política para encontrar a definição *VMs de auditoria que não utilizam discos geridos*. Clique nessa política e clique em **Atribuir**.

   ![Encontrar a definição de política correta](media/assign-policy-definition/select-available-definition.png)

5. Indique um **Nome** de apresentação para a atribuição de política. Neste caso, vamos utilizar *VMs de auditoria que não utilizam discos geridos*. Também pode adicionar uma **Descrição** opcional. A descrição apresenta detalhes sobre a forma como esta atribuição de política identifica todas as máquinas virtuais criadas neste ambiente que não utilizam discos geridos.
6. Altere o escalão de preço para **Standard** para se certificar de que a política é aplicada aos recursos existentes.

   Existem dois escalões de preços no Azure Policy – *Gratuito* e *Standard*. No Escalão gratuito, apenas pode impor políticas em recursos futuros, enquanto no Standard, também pode impô-los em recursos existentes para compreender melhor o seu estado de compatibilidade. Uma vez que estamos na Pré-visualização Limitada, ainda não lançámos um modelo de preços, pelo que não irá receber uma fatura para selecionar *Standard*. Para obter mais informações sobre preços, veja: [Azure Policy pricing (Preços do Azure Policy)](https://azure.microsoft.com/pricing/details/azure-policy/).

7. Selecione o **Âmbito** em que pretende que a política seja aplicada.  Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos.
8. Selecione a subscrição (ou grupo de recursos) que registou anteriormente quando optou pelo Azure Policy. Neste exemplo, estamos a utilizar esta subscrição - **Desenvolvimento de Capacidade de Análise do Azure**, mas as opções irão variar.

   ![Encontrar a definição de política correta](media/assign-policy-definition/assign-policy.png)

9. Selecione **Atribuir**.

Agora, está pronto para identificar recursos incompatíveis para compreender o estado de compatibilidade do seu ambiente.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Selecione **Conformidade** no painel esquerdo e procure a atribuição de política que criou.

![Conformidade com a política](media/assign-policy-definition/policy-compliance.png)

Se existirem quaisquer recursos existentes que não sejam compatíveis com esta nova atribuição, estes serão apresentados no separador **Recursos não compatíveis**.

Se uma condição for avaliada nos recursos existentes e for verdadeira para alguns deles, esses recursos são marcados como incompatíveis com a política. Eis uma tabela de como as diferentes ações que temos disponíveis atualmente funcionam com o resultado da avaliação de condição e o estado de compatibilidade dos seus recursos.

|Recurso  |Se a Condição na Política Avaliar como  |Ação na Política   |Estado de Compatibilidade  |
|-----------|---------|---------|---------|
|Existe     |Verdadeiro     |Negar     |Incompatível |
|Existe     |Falso    |Negar     |Compatível     |
|Existe     |Verdadeiro     |Acrescentar   |Incompatível |
|Existe     |Falso    |Acrescentar   |Compatível     |
|Existe     |Verdadeiro     |Auditoria    |Incompatível |
|Existe     |Falso    |Auditoria    |Incompatível |

## <a name="clean-up-resources"></a>Limpar recursos

Outros guias desta coleção têm por base este guia de introdução. Se quiser continuar a trabalhar com os tutoriais subsequentes, não limpe os recursos criados neste guia de introdução. Se não pretender continuar, utilize os passos seguintes para eliminar todos os recursos criados por este guia de introdução no portal do Azure.
1. Selecione **Atribuições** no painel esquerdo.
2. Procure a atribuição que acabou de criar.

   ![Eliminar uma atribuição](media/assign-policy-definition/delete-assignment.png)

3.  Selecione **Eliminar Atribuição**.

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, atribuiu uma definição de política a um âmbito para se certificar de que todos os recursos desse âmbito são compatíveis e identificar aqueles que não são.

Para saber mais sobre a atribuição de políticas, de forma a certificar-se de que os **futuros** recursos que são criados estão em conformidade, avance para o tutorial:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./create-manage-policy.md)
