---
title: Relatório de validação de pilha do Azure | Microsoft Docs
description: Utilize o relatório do Verificador de preparação de pilha do Azure para rever os resultados da validação.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a0ca0ae3ed615f6bc2774364f7a443023b911b5d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="azure-stack-validation-report"></a>Relatório de validação de pilha do Azure
A ferramenta do Verificador de preparação de pilha do Azure é executado validações que suportam a implementação e manutenção de um ambiente de pilha do Azure. A ferramenta escreve resultados de validação para um ficheiro de relatório. JSON. O relatório apresenta dados resumidos e detalhados sobre o estado dos pré-requisitos de implementação da pilha do Azure e no rotação de segredos para implementações de pilha do Azure existente.  

 ## <a name="where-to-find-the-report"></a>Onde encontrar o relatório
Quando a ferramenta é executada, regista os resultados para **AzsReadinessCheckerReport.json**. A ferramenta também cria um registo com o nome **AzsReadinessChecker.log**. Apresenta a localização destes ficheiros com os resultados de validação no PowerShell.

![validação de execução](./media/azure-stack-validation-report/validation.png)

Ambos os ficheiros de mantêm os resultados das verificações de validação subsequentes quando executados no mesmo computador.  Por exemplo, a ferramenta pode ser executada para Validar certificado, execute novamente para validar a identidade do Azure e, em seguida, uma terceira tempo para validar o registo. Os resultados de todas as três validações estão disponíveis no relatório. JSON resultante.  

Por predefinição, ambos os ficheiros são escritos *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Utilize o **- OutputPath** ***&lt;caminho&gt;*** parâmetro no final da linha de comandos execute para especificar uma localização de relatórios diferente.   
- Utilize o **- CleanReport** parâmetro no final do comando para limpar as informações de execução *AzsReadinessCheckerReport.json*. sobre execuções anteriores da ferramenta.

## <a name="view-the-report"></a>Ver o relatório
Para ver o relatório no PowerShell, forneça o caminho para o relatório como um valor para **- ReportPath**. Este comando apresenta o conteúdo do relatório e também identifica validações que ainda não existirem resultados.

Por exemplo, para ver o relatório a partir da linha de comandos do PowerShell que está aberta para a localização onde está localizado o relatório, execute: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

O resultado é semelhante a imagem seguinte:

![Ver relatório](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Ver o relatório de resumo
Para ver um resumo do relatório, pode adicionar o **-resumo** mudar para o fim da linha de comandos do PowerShell. Por exemplo: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

O resumo mostra validações que não tem resultados e indica passar ou falhar para validações estiverem concluídas. O resultado é semelhante a imagem seguinte:

![relatório de resumo](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Ver um relatório filtrado
Para ver um relatório filtrado num único tipo de validação, utilize o **- ReportSections** parâmetro e especificar um dos seguintes valores que correspondem ao tipo de validação que pretende visualizar:
- Certificado
- AzureRegistration
- AzureIdentity
- Tarefas   
- Todos  

Por exemplo, para ver o relatório de resumo para certificados só, utilizar a seguinte linha de comandos do PowerShell: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Consulte também
[Referência de cmdlet Start-AzsReadinessChecker](azure-stack-azsreadiness-cmdlet.md)
