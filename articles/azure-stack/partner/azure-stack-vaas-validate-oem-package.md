---
title: Validar pacotes de fabricante de equipamento original (OEM) na validação do Azure Stack como serviço | Documentos da Microsoft
description: Saiba como verificar os pacotes de fabricante de equipamento original (OEM) com a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a08f439780e0080d8da2cde1531e1580dbdad14f
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235479"
---
# <a name="validate-oem-packages"></a>Validar pacotes do OEM

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Quando tiver ocorrido uma alteração para o firmware ou os controladores de uma validação de solução concluída, pode testar um novo pacote de OEM. Quando o pacote tiver passado o teste, ele é assinado pela Microsoft. O teste tem de conter o pacote de extensão de OEM atualizado com os controladores e firmware que tenham passado o logótipo do Windows Server e os testes PCS.

Todos os testes de ser concluído dentro de 24 horas com o resultado da **foi concluída com êxito**. Se tiver qualquer um dos testes de um resultado de **falha**, Registre um bug no [Microsoft Collaborate](https://aka.ms/collaborate) e notificar a Microsoft enviando um e-mail para [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

## <a name="get-your-oem-package-signed"></a>Crie o pacote de OEM com sessão iniciado

1. Certifique-se de que a atualização mensal atual foi aplicada. Para a versão mais recente, consulte a versão mais recente na [documentação de operador do Azure Stack > Descrição geral > notas de versão](https://docs.microsoft.com/azure/azure-stack/) .

    As atualizações de software do Microsoft Azure Stack são designadas usando uma convenção de nomenclatura, por exemplo, 1803 que indica que a atualização é de Março de 2018. Para obter informações sobre a política de atualização do Azure Stack, cadência e notas de versão estão disponíveis, consulte [política de manutenção do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

1. Verifique o estado de funcionamento do sistema, executando **AzureStack teste** como descrito em [executar um teste de validação para o Azure Stack. Corrija todos os avisos e erros antes de iniciar qualquer teste.

2. Na [portal de validação](https://azurestackvalidation.com), selecione uma solução existente. Se não tiver adicionado sua solução, consulte [adicionar uma nova solução](azure-stack-vaas-validate-solution-new.md#add-a-new-solution).

3. Selecione **iniciar** sobre o **validações de pacote** mosaico para iniciar um novo fluxo de trabalho.

    ![Validações do pacote](media/image3.png)

4.  Forneça um diagnóstico de cadeia de ligação. Para obter instruções, consulte [configurar uma conta de armazenamento](azure-stack-vaas-set-up-account.md).

    É necessário especificar um pacote de extensão do OEM para cada execução de validação do pacote. Especifique o pacote de OEM que foi instalado na solução no momento da implementação do Azure Stack. Para obter instruções, consulte [criar um blob de armazenamento do Azure para armazenar registos](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    Um ficheiro JSON com as variáveis de ambiente deve ser usado para concluir a entrada para campos obrigatórios para a execução evitar erros na entrada de dados. Para obter instruções, consulte [obter um ficheiro de configuração numa implementação do Azure Stack](azure-stack-vaas-parameters.md).

5. Execute os testes.

6. Quando todos os testes foram concluídas com êxito, enviar o nome da sua solução e o pacote de validação para [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) para solicitar a assinatura de pacote.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
