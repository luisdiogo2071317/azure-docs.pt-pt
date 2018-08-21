---
title: Validar uma nova solução do Azure Stack | Documentos da Microsoft
description: Saiba como validar uma nova solução do Azure Stack com validação como um serviço.
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
ms.openlocfilehash: 90c18af0f846061c102f1daf1b84a332aaec1dc6
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235527"
---
# <a name="validate-a-new-azure-stack-solution"></a>Validar uma nova solução do Azure Stack

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Saiba como pode utilizar o fluxo de trabalho de validação de solução de certificação novas soluções do Azure Stack.

Uma solução do Azure Stack é uma lista de hardware de materiais (LM) que tem sido em conjunto, faz um acordo com a Microsoft e tiver passado o logótipo do Windows Server com os requisitos de certificação. Também pode utilizar o fluxo de trabalho de validação de solução quando tiver ocorrido uma alteração para o BoM que faria com que uma solução para ser classificado como de hardware *novo*. Se existirem questões sobre o que dispararia uma **novos** ou **recertification** de uma solução em contato pelo [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Para certificar a sua solução, execute o fluxo de trabalho duas vezes. Executá-lo uma vez para o *minimamente* suportado de configuração. Executar numa segunda vez para o *máximo* configuração. Microsoft certifica-se que a solução se ambas as configurações de passar em todos os testes.

Este início rápido permite-lhe acontecendo o processo de adicionar a solução e a execução de testes.

## <a name="add-a-new-solution"></a>Adicionar uma nova solução

1. Inicie sessão para o [portal de validação](https://azurestackvalidation.com).
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
4. Selecione **carregar** e, em seguida, adicione o ficheiro de configuração de implementação. Este é um passo opcional. Também pode adicionar os parâmetros de teste ao seguir os passos na secção seguinte.

    > [!note]  
    > Pode criar o ficheiro de configuração adicionando os parâmetros nos parâmetros do ambientais um secções de parâmetros comuns do teste na interface. Pode recuperar o arquivo gerado pelo serviço da implementação do Azure Stack que está a ser validado. Para obter instruções, consulte [parâmetros comuns do fluxo de trabalho para a validação do Azure Stack como um serviço](azure-stack-vaas-parameters.md).

5. Adicione os parâmetros ambientais. Para obter mais informações, consulte [adicionar parâmetros ambientais](#add-environmental-parameters).
6. Adicione os parâmetros comuns de teste. Para obter mais informações, consulte [adicionar parâmetros comuns de teste](#add-common-test-parameters).

    Consoante a definição de teste, o teste poderá exigir que introduza um valor independentemente dos parâmetros comuns, ou poderá permitir-lhe substituir o valor do parâmetro comum.

7. Clique em **submeter** para agendar o teste.

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

- [Reagendar ou cancelar um teste](azure-stack-vaas-monitor-test.md#reschedule-a-test)
- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).