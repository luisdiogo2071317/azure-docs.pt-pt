---
title: Utilize a validação como um serviço para o portal do Azure Stack para agendar o seu primeiro teste | Documentos da Microsoft
description: Utilize a validação como um serviço para o portal do Azure Stack para agendar o seu primeiro teste.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 0f681070df4b4b3384171c05edb3851abec2ab5c
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235514"
---
# <a name="quickstart-use-the-validation-as-a-service-portal-to-schedule-your-first-test"></a>Início rápido: Utilizar a validação como um portal de serviço para agendar o seu primeiro teste

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Saiba como programar o seu primeiro teste com a validação como um portal de serviço (VaaS) para verificar se o seu hardware. O agente local tem de ser implementado na solução do Azure Stack que está a ser validada antes de executar testes de validação.

Neste início rápido adicione sua solução e executar testes.

## <a name="prerequisites"></a>Pré-requisitos

Antes de seguir este início rápido, terá de ter:
 - Uma validação como uma conta de serviço. Para obter instruções, consulte [configurar a sua validação como uma conta de serviço](azure-stack-vaas-set-up-account.md).  
- O agente local instalado no seu sistema. Para obter instruções, consulte [implementar as máquinas de virtuais locais do agente e o teste](azure-stack-vaas-test-vm.md).

## <a name="add-a-new-solution"></a>Adicionar uma nova solução

1. Inicie sessão para o [portal de validação](https://azurestackvalidation.com).

    ![Inicie sessão no portal a validação](media/vaas_portalsignin.png)  

2. Selecione **nova solução**.
3. Introduza um nome para a solução e selecione **guardar**.

## <a name="create-a-solution-validation-workflow"></a>Criar um fluxo de trabalho de validação de solução

1. Selecione o nome da solução.
2. Selecione **gerir** sobre o **validações de solução** mosaico.

    ![Solução validações](media/image2.png)

## <a name="create-a-solution-workflow"></a>Criar um fluxo de trabalho da solução

1. Selecione **validação de solução nova**.
2. Escreva o nome da validação.
3. Selecione **mínimo** ou **máximo**.  
    - **Mínimo**  
    A solução está configurada com o número mínimo suportado de nós.  
    - **Máximo**  
    A solução está configurada com o número máximo suportado de nós.
4. Adicione os parâmetros ambientais. Para obter mais informações, consulte [adicionar parâmetros ambientais](#add-environmental-parameters).
5. Adicione os parâmetros comuns de teste. Para obter mais informações, consulte [adicionar parâmetros comuns de teste](#add-common-test-parameters).

    Consoante a definição de teste, o teste poderá exigir que introduza um valor independentemente dos parâmetros comuns, ou poderá permitir-lhe substituir o valor do parâmetro comum.
6. Clique em **submeter** para agendar o teste.

## <a name="add-environmental-parameters"></a>Adicionar parâmetros ambientais

Adicione os seguintes parâmetros ambientais:

| Informações de aprovação de teste | Necessário | Descrição |
| --- | --- | --- | --- |
| Compilação de pilha do Azure | Necessário | Compilação de pilha do Azure que valor numérico (por exemplo 20170501.1) tem de ser um Azure Stack válido criar número ou uma versão, por exemplo, 1.0.170330.9 |
| ID do inquilino | Necessário | ID do inquilino do Active Directory. Tem de ser um GUID (por exemplo ECA23256-6BA0-4F27-8E4D-AFB02F088363) |
| Região | Necessário | Região de implementação do Azure Stack |
| Ponto final do Gestor de recursos de inquilino | Necessário | Ponto final para operações de inquilino do Azure Resource Manager (por exemplo https://management.loc-ext.domain.com) |
| Ponto final do Gestor de recursos de administração | Não necessário | Ponto final para operações de inquilino do Azure Resource Manager (por exemplo https://management.loc-ext.domain.com) |
| FQDN externo | Não necessário | Externo utilizado como o sufixo para pontos finais de nome de domínio completamente qualificado. (por exemplo local.azurestack.external ou redmond.contoso.com) |
| Número de nós | Necessário | O número de nós na sua solução. |

## <a name="add-common-test-parameters"></a>Adicionar parâmetros comuns de teste

Adicione os seguintes parâmetros comuns de teste:

| Informações de aprovação de teste | Necessário | Descrição |
| --- | --- | --- |
| Nome de utilizador do inquilino | Necessário | Nome de utilizador do inquilino (por exemplo tenant@contoso.onmicrosoft.com) |
| Palavra-passe do inquilino | Necessário | A palavra-passe para o inquilino. |
| Nome de utilizador do administrador de serviços | Não necessário | Nome de utilizador do inquilino (por exemplo tenant@contoso.onmicrosoft.com) |
| Palavra-passe de administrador de serviços | Não necessário | Nome de utilizador do administrador de serviços (por exemplo serviceadmin@contoso.onmicrosoft.com) |
| Nome de utilizador do administrador da nuvem | Não necessário | Conta de administrador do Azure domínio de pilha (por exemplo, contoso\cloudadmin) |
| Palavra-passe de administrador da nuvem | Não necessário | |
|  Cadeia de ligação de diagnóstico | Não necessário | URI de SAS para uma conta de armazenamento do Azure para que o diagnóstico registos serão copiados durante a execução de teste. Ver [criar um blob de armazenamento do Azure para armazenar registos](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs). <br><br>O valor do **cadeia de ligação de diagnóstico** parâmetro comum de será armazenado pelo serviço e indicado no momento de agendamento para todos os testes no fluxo de trabalho que utilize este parâmetro. Quando o URL de SAS é dentro de 30 dias de expiração, será solicitado para um URL de SAS novo na página de parâmetros comuns. |
| Etiqueta - nome | Não necessário |  Marcas descritivas podem ser introduzidas para etiquetar o fluxo de trabalho. Este é o nome da etiqueta. |
| Etiqueta - valor | Não necessário | Marcas descritivas podem ser introduzidas para etiquetar o fluxo de trabalho. Este é o valor da etiqueta. |

## <a name="next-steps"></a>Passos Seguintes

- [Validar uma nova solução do Azure Stack](azure-stack-vaas-validate-solution-new.md)  
- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
