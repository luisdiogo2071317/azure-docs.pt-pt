---
title: Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure
description: Este artigo explica os passos para criar uma definição de política para identificar recursos incompatíveis.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 25e8e7837328fa8691066631514414e4455e4851
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935906"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure

O primeiro passo para compreender a conformidade no Azure consiste em identificar o estado dos seus recursos. Este guia de introdução acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, vai identificar com êxito as máquinas virtuais que não estão a utilizar discos geridos. Estão em *não conformidade* com a atribuição de política.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução, vai criar uma atribuição de política e atribuir as *Máquinas Virtuais de Auditoria sem uma definição de política do Managed Disks*.

1. Inicie o serviço Azure Policy no portal do Azure ao clicar em **Todos os serviços** e, em seguida, ao pesquisar e selecionar **Policy**.

   ![Pesquisar política](media/assign-policy-definition/search-policy.png)

2. Selecione **Atribuições** no lado esquerdo da página Azure Policy. Uma atribuição é uma política que foi atribuída para ter lugar num âmbito específico.
3. Selecione **Atribuir Política** na parte superior da página **Política - Atribuições**.

   ![Atribuir uma definição de política](media/assign-policy-definition/select-assign-policy.png)

4. Na página **Atribuir Política**, selecione o **Âmbito** ao clicar nas reticências e selecionando uma subscrição (obrigatório) e um grupo de recursos (opcional). Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta.  Em seguida, clique em **Selecionar** na parte inferior da página **Âmbito**.

   Este exemplo utiliza a **Subscrição Contoso**. A sua subscrição vai ser diferente.

5. Se quisesse excluir um ou mais grupos de recursos (se definisse apenas o âmbito como uma subscrição) ou recursos específicos dentro de um grupo de recursos (ambos os casos de definição de âmbito), podia configurar **Exclusões** a partir da atribuição de política. Por agora, deixe em branco.

6. Selecione as reticências de **Definição de política** para abrir a lista de definições disponíveis. O Azure Policy já inclui as definições de política incorporada que pode utilizar. Muitas definições de política incorporada estão disponíveis, tais como:

   - Impor etiqueta e o respetivo valor
   - Aplicar etiqueta e o respetivo valor
   - Requer a Versão do SQL Server 12.0

    Para obter uma lista completa de todas as políticas incorporadas disponíveis, veja os [Modelos de política](json-samples.md).

7. Pesquise nas definições de política para encontrar a definição *Auditar VMs que não utilizam discos geridos*. Clique nessa política e clique em **Selecionar**.

   ![Encontrar a definição de política correta](media/assign-policy-definition/select-available-definition.png)

8. O **Nome da atribuição** é automaticamente preenchido com o nome da política que selecionou, mas pode alterá-lo. Neste exemplo, vamos deixar *Auditar VMs que não utilizam discos geridos*. Também pode adicionar uma **Descrição** opcional. A descrição fornece detalhes sobre esta atribuição de política.

9. Clique em **Atribuir**.

Agora, está pronto para identificar recursos incompatíveis para compreender o estado de compatibilidade do seu ambiente.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Selecione **Conformidade** no lado esquerdo da página e localize a atribuição de política **Auditar VMs que não utilizam discos geridos** que criou.

![Conformidade com a política](media/assign-policy-definition/policy-compliance.png)

Se existirem quaisquer recursos existentes que não sejam compatíveis com esta nova atribuição, aparecem em **Recursos em não conformidade**.

Quando uma condição é avaliada em relação aos seus recursos existentes e resulta como verdadeira, esses recursos são então marcados como em não conformidade com a política. A tabela seguinte mostra como funcionam os diferentes efeitos de política com a avaliação de condição para o estado de conformidade resultante. Apesar de não ver a lógica de avaliação no portal do Azure, são mostrados os resultados do estado de conformidade. O resultado do estado de conformidade pode ser em conformidade ou em não conformidade.

| **Estado do Recurso** | **Efeito** | **Avaliação da Política** | **Estado de Compatibilidade** |
| --- | --- | --- | --- |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Falso | Compatível |
| Novo | Audit, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Novo | Audit, AuditIfNotExist\* | Falso | Compatível |

\* Os efeitos de Append, DeployIfNotExist e AuditIfNotExist requerem que a declaração IF seja TRUE. Os efeitos também necessitam que a condição de existência seja FALSE para estarem em não conformidade. Quando for TRUE, a condição IF aciona a avaliação da condição de existência dos recursos relacionados.

## <a name="clean-up-resources"></a>Limpar recursos

Outros guias desta coleção têm por base este guia de introdução. Se quiser continuar a trabalhar com os tutoriais subsequentes, não limpe os recursos criados neste guia de introdução. Se não pretender continuar, utilize os passos seguintes para eliminar todos os recursos criados por este guia de introdução no portal do Azure.

1. Selecione **Conformidade** (ou **Atribuições**) no lado esquerdo da página Azure Policy e localize a atribuição de política **Auditar VMs que não utilizam discos geridos** que criou.

2. Clique com o botão direito do rato na atribuição de política **Auditar VMs que não utilizam discos geridos** e selecione **Eliminar atribuição**

   ![Eliminar uma atribuição](media/assign-policy-definition/delete-assignment.png)

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, atribuiu uma definição de política a um âmbito e avaliou o respetivo relatório de conformidade. A definição de política garante que todos os recursos no âmbito estão em conformidade e identifica quais não estão.

Para saber mais sobre a atribuição de políticas, de forma a certificar-se de que os **futuros** recursos que são criados estão em conformidade, avance para o tutorial:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](create-manage-policy.md)