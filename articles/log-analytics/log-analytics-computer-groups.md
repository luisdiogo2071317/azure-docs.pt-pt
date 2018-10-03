---
title: Pesquisas de registos de grupos de computadores no Log Analytics do Azure | Documentos da Microsoft
description: Grupos de computadores no Log Analytics permitem pesquisas de registos de âmbito para um determinado conjunto de computadores.  Este artigo descreve os diferentes métodos que pode utilizar para criar grupos de computadores e como usá-las numa pesquisa de registos.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 7e4889148a752b552f8bd65702ea5dda450ded31
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044302"
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Pesquisas de registos de grupos de computadores no Log Analytics

Grupos de computador no Log Analytics permitem-lhe âmbito [pesquisas de registos](log-analytics-log-search-new.md) para um determinado conjunto de computadores.  Cada grupo é preenchido com computadores por meio de uma consulta por si ou ao importar grupos de diferentes origens.  Quando o grupo está incluído numa pesquisa de registos, os resultados estão limitados aos registos que correspondem a computadores no grupo.

## <a name="creating-a-computer-group"></a>Criar um grupo de computadores
Pode criar um grupo de computadores no Log Analytics com qualquer um dos métodos na tabela seguinte.  Obter detalhes sobre cada método são fornecidos nas secções abaixo. 

| Método | Descrição |
|:--- |:--- |
| Pesquisas de registos |Crie uma pesquisa de registos que devolva uma lista de computadores. |
| API de Pesquisas de Registos |Utilize a API de pesquisa de registo para criar programaticamente um grupo de computadores com base nos resultados da pesquisa de registos. |
| Active Directory |Analise automaticamente a associação do grupo de todos os computadores agente que são membros de um domínio do Active Directory e criar um grupo no Log Analytics para cada grupo de segurança. |
| Configuration Manager | Importar coleções a partir do System Center Configuration Manager e crie um grupo no Log Analytics para cada um. |
| Windows Server Update Services |Procurar o filtragem de grupos de servidores WSUS ou de clientes e criar um grupo no Log Analytics para cada automaticamente. |

### <a name="log-search"></a>Pesquisas de registos
Criado a partir de uma pesquisa de registos de grupos de computadores contém todos os computadores devolvidos por uma consulta que definir.  Esta consulta é executada sempre que o grupo de computadores é utilizado para que quaisquer alterações, uma vez que o grupo foi criado é refletida.  

Pode usar qualquer consulta para um grupo de computadores, mas tem de devolver um conjunto diferente de computadores utilizando `distinct Computer`.  Segue-se uma pesquisa de exemplo típico que poderia usar para como um grupo de computadores.

    Heartbeat | where Computer contains "srv" | distinct Computer

A tabela seguinte descreve as propriedades que definem um grupo de computadores.

| Propriedade | Descrição |
|:---|:---|
| Nome a Apresentar   | Nome da pesquisa para apresentar no portal. |
| Categoria       | Categoria para organizar as pesquisas no portal. |
| Consulta          | A consulta para o grupo de computadores. |
| Alias de função | Um alias de exclusivo utilizado para identificar o grupo de computadores numa consulta. |

Utilize o procedimento seguinte para criar um grupo de computadores a partir de uma pesquisa de registos no portal do Azure.

2. Open **pesquisa de registos** e, em seguida, clique em **pesquisas guardadas** na parte superior do ecrã.
3. Clique em **adicionar** e forneça valores para cada propriedade para o grupo de computadores.
4. Selecione **guardar esta consulta como grupo de computadores** e clique em **OK**.



### <a name="active-directory"></a>Active Directory
Quando configurar o Log Analytics para importar associações a grupos do Active Directory, analisa a associação de grupo de todos os computadores associados a um domínio com o agente do OMS.  Um grupo de computadores é criado no Log Analytics para cada grupo de segurança no Active Directory, e cada computador é adicionado para os grupos de computadores correspondentes aos grupos de segurança que são membros.  Esta associação é constantemente atualizada a cada 4 horas.  

Configurar o Log Analytics para importar grupos de segurança do Active Directory do Log Analytics **definições avançadas** no portal do Azure.  Selecione **grupos de computadores**, **do Active Directory**e, em seguida **associações a grupos de computadores do Active Directory de importação**.  Não há nenhuma configuração adicional.

![Grupos de computadores do Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Quando os grupos foram importados, o menu de lista o número de computadores com a associação a grupos detetados e o número de grupos importados.  Pode clicar em qualquer uma destas ligações para retornar a **grupo de computador** registos com essas informações.

### <a name="windows-server-update-service"></a>Serviço de Atualização do Windows Server
Quando configurar o Log Analytics para importar associações a grupos WSUS, analisa a associação ao grupo de direcionamento de todos os computadores com o agente do OMS.  Se estiver a utilizar o lado do cliente como o objetivo, qualquer computador que está ligada ao Log Analytics e faz parte de qualquer WSUS filtragem de grupos tem respetiva associação a grupos importada para o Log Analytics. Se estiver a utilizar o servidor como o objetivo, o OMS agente deve ser instalado no servidor WSUS para que as informações de associação de grupo ser importado para o Log Analytics.  Esta associação é constantemente atualizada a cada 4 horas. 

Configurar o Log Analytics para importar grupos WSUS a partir do Log Analytics **definições avançadas** no portal do Azure.  Selecione **grupos de computadores**, **WSUS**e, em seguida **associações de grupo do WSUS de importação**.  Não há nenhuma configuração adicional.

![Grupos de computadores do WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Quando os grupos foram importados, o menu de lista o número de computadores com a associação a grupos detetados e o número de grupos importados.  Pode clicar em qualquer uma destas ligações para retornar a **grupo de computador** registos com essas informações.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Quando configurar o Log Analytics para importar subscrições da coleção do Configuration Manager, ele cria um grupo de computadores para cada coleção.  As informações de associação de coleção são recuperadas em três horas a manter os grupos de computadores atualizados. 

Antes de pode importar coleções do Configuration Manager, tem [ligar o Configuration Manager ao Log Analytics](log-analytics-sccm.md).  Em seguida, pode configurar a importação do Log Analytics **definições avançadas** no portal do Azure.  Selecione **grupos de computadores**, **SCCM**e, em seguida **associações de coleção do Gestor de configuração da importação**.  Não há nenhuma configuração adicional.

![Grupos de computadores do SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Quando tem sido importadas coleções, o menu de lista o número de computadores com a associação a grupos detetados e o número de grupos importados.  Pode clicar em qualquer uma destas ligações para retornar a **grupo de computador** registos com essas informações.

## <a name="managing-computer-groups"></a>Gerir grupos de computadores
Pode ver grupos de computadores que foram criados a partir de uma pesquisa de registos ou a API de pesquisa de registos do Log Analytics **definições avançadas** no portal do Azure.  Selecione **grupos de computadores** e, em seguida **guardado grupos**.  

Clique nas **x** no **remover** coluna para eliminar o grupo de computadores.  Clique nas **ver membros** ícone para um grupo para executar a pesquisa de registos do grupo, que retorna seus membros.  Não é possível modificar um grupo de computadores, mas em vez disso, deve eliminar e, em seguida, recrie-o com as definições modificadas.

![Grupos de computadores guardados](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Utilizar um grupo de computadores numa pesquisa de registos
Utilize um grupo de computadores criado a partir de uma pesquisa de registos numa consulta ao tratar o respetivo alias como uma função, normalmente, com a seguinte sintaxe:

  `Table | where Computer in (ComputerGroup)`

Por exemplo, poderia usar o seguinte para devolver registos UpdateSummary para apenas os computadores num grupo de computadores chamado mycomputergroup.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


Grupos de computadores importados e os respetivos computadores incluídos são armazenadas no **grupo de computador** tabela.  Por exemplo, a seguinte consulta retornaria uma lista de computadores no grupo de computadores de domínio do Active Directory. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

A seguinte consulta retornaria registos UpdateSummary para apenas os computadores em computadores de domínio.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Registos do grupo de computadores
Na área de trabalho do Log Analytics para cada associação de grupo do computador criada a partir do Active Directory ou WSUS, é criado um registo.  Estes registos têm um tipo de **grupo de computador** e ter as propriedades na tabela seguinte.  Registos não são criados para grupos de computadores com base em pesquisas de registos.

| Propriedade | Descrição |
|:--- |:--- |
| Tipo |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computador |Nome do computador membro. |
| Grupo |Nome do grupo. |
| GroupFullName |Caminho completo para o grupo, incluindo a origem e o nome de origem. |
| GroupSource |Esse grupo de origem foi recolhido a partir de. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Nome da origem de que o grupo foi recolhido a partir de.  Para o Active Directory, este é o nome de domínio. |
| ManagementGroupName |O nome do grupo de gestão para agentes do SCOM.  Para outros agentes, é AOI -\<ID da área de trabalho\> |
| TimeGenerated |Data e hora, o grupo de computadores foi criado ou atualizado. |

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registos](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções.  

