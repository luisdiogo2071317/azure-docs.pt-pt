---
title: Relatório de validação para o Azure Stack | Documentos da Microsoft
description: Utilize o relatório do Verificador de preparação do Azure Stack para rever os resultados da validação.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 156b84e4941363716721b5cee6c19333ffe7594c
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079451"
---
# <a name="azure-stack-validation-report"></a>Relatório de validação do Azure Stack
Utilize a ferramenta de verificação de preparação do Azure Stack para executar validações que suportam a implementação e manutenção de um ambiente do Azure Stack. A ferramenta grava os resultados para um ficheiro de relatório. JSON. O relatório apresenta dados detalhados e resumidos sobre o estado dos pré-requisitos de implementação do Azure Stack. O relatório também apresenta informações sobre a rotação de segredos para Implantações de pilha do Azure existente.  

 ## <a name="where-to-find-the-report"></a>Onde encontrar o relatório
Quando a ferramenta é executada, regista os resultados para **AzsReadinessCheckerReport.json**. A ferramenta também cria um registo com o nome **AzsReadinessChecker.log**. A localização dos ficheiros é apresentada com os resultados de validação no PowerShell.

![validação de execução](./media/azure-stack-validation-report/validation.png)

Ambos os arquivos de mantêm os resultados de verificações de validação subsequentes quando são executados no mesmo computador.  Por exemplo, a ferramenta pode ser executada para validar certificados, execute novamente para validar a identidade do Azure e, em seguida, uma terceira vez para validar o registo. Os resultados de todas as três validações estão disponíveis no relatório resultante. JSON.  

Por predefinição, ambos os ficheiros são escritos *C:\Users\<nome de utilizador > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Utilize o **- OutputPath** ***&lt;caminho&gt;*** parâmetro no final da linha de comandos de execução para especificar uma localização diferente do relatório.   
- Utilize o **- CleanReport** parâmetro no final do comando de execução para limpar as informações de *AzsReadinessCheckerReport.json*. sobre execuções anteriores da ferramenta.

## <a name="view-the-report"></a>Ver o relatório
Para ver o relatório no PowerShell, fornecer o caminho para o relatório como um valor para **- ReportPath**. Este comando apresenta o conteúdo do relatório e identifica as validações que ainda não tem resultados.

Por exemplo, para ver o relatório a partir da linha de comandos do PowerShell que está aberta para a localização onde o relatório se encontra, execute: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

O resultado é semelhante à imagem seguinte:

![Ver relatório](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Ver o relatório de resumo
Para ver um resumo do relatório, pode adicionar os **-resumo** mudar para o final da linha de comando do PowerShell. Por exemplo: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

O resumo mostra as validações que não têm os resultados e indica a aprovação ou reprovação para validações forem concluídas. O resultado é semelhante à imagem seguinte:

![Resumo do relatório](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Ver um relatório filtrado
Para ver um relatório que é filtrado num único tipo de validação, utilize o **- ReportSections** parâmetro com um dos seguintes valores:
- Certificado
- AzureRegistration
- AzureIdentity
- Tarefas   
- Todos  

Por exemplo, para ver o relatório Resumo de certificados só, utilizar a seguinte linha de comandos do PowerShell: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Consulte também
[Referência do cmdlet Start-AzsReadinessChecker](azure-stack-azsreadiness-cmdlet.md)
