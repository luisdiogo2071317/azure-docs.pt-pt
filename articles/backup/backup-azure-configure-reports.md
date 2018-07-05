---
title: Configurar relatórios para o Azure Backup
description: Configure relatórios do Power BI para o Azure Backup com o Cofre dos serviços de recuperação.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81653f9125b9cc4411e5cfe358bd602f92c5bf89
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448371"
---
# <a name="configure-azure-backup-reports"></a>Configurar relatórios do Azure Backup
Este artigo fala sobre os passos para configurar os relatórios para o Azure Backup com o Cofre dos serviços de recuperação e para aceder a estes relatórios com o Power BI. Depois de efetuar estes passos, pode ir diretamente para o Power BI para ver todos os relatórios, personalizar e criar relatórios. 

## <a name="supported-scenarios"></a>Cenários suportados
1. Relatórios de cópia de segurança do Azure são suportados para cópia de segurança da máquina virtual do Azure e de cópia de segurança do ficheiro/pasta para a cloud com o agente de serviços de recuperação do Azure.
2. Relatórios do SQL do Azure, o DPM e o Azure Backup Server não são suportados neste momento.
3. Pode visualizar relatórios em cofres e entre subscrições, se a mesma conta de armazenamento está configurada para cada um dos cofres de. Conta de armazenamento selecionada deve estar na mesma região que o Cofre de serviços de recuperação.
4. A frequência de atualização agendada para os relatórios é 24 horas no Power BI. Também pode executar uma atualização do ad-hoc dos relatórios no Power BI, em que casos dados mais recentes na conta de armazenamento do cliente são utilizados para composição de relatórios. 
5. Relatórios de cópia de segurança do Azure não são atualmente suportados em clouds nacionais.

## <a name="prerequisites"></a>Pré-requisitos
1. Criar uma [conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) para configurá-lo para relatórios. Esta conta de armazenamento é utilizada para armazenar dados relacionados de relatórios.
2. [Criar uma conta do Power BI](https://powerbi.microsoft.com/landing/signin/) para visualizar, personalizar e criar seus próprios relatórios através do portal do Power BI.
3. Registar o fornecedor de recursos **Microsoft. insights** se não registado já, com a subscrição da conta de armazenamento e também com a subscrição do Cofre de serviços de recuperação para ativar a comunicar dados a ser enviados para o armazenamento conta. Para fazer o mesmo, tem de aceder ao portal do Azure > subscrição > fornecedores de recursos e de verificação para este fornecedor para o registar. 

## <a name="configure-storage-account-for-reports"></a>Configurar a conta de armazenamento para relatórios
Utilize os seguintes passos para configurar a conta de armazenamento para o Cofre dos recovery services através do portal do Azure. Esta é uma configuração única e assim que a conta de armazenamento estiver configurada, pode aceder ao Power BI diretamente para ver o pacote de conteúdos e tirar partido dos relatórios.
1. Se já tiver um cofre de serviços de recuperação aberto, avance para o passo seguinte. Se não tiver um cofre de serviços de recuperação aberto, mas está no portal do Azure, clique em **todos os serviços**.

   * Na lista de recursos, escreva **Serviços de Recuperação**.
   * À medida que começa a escrever, a lista filtra com base na sua entrada. Quando vir **Cofres dos Serviços de Recuperação**, clique no mesmo.

      ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     É apresentada a lista dos cofres dos Serviços de Recuperação. Na lista dos cofres dos Serviços de Recuperação, selecione um cofre.

     O dashboard do cofre selecionado é aberto.
2. Na lista de itens que é apresentado no cofre, clique em **relatórios de cópia de segurança** na secção monitorização e relatórios para configurar a conta de armazenamento para relatórios.

      ![Selecione os relatórios de cópia de segurança menu item passo 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. No painel de relatórios de cópia de segurança, clique em **as definições de diagnóstico** ligação. Esta ação abre as definições de diagnóstico da interface do Usuário que é utilizado para enviar dados para a conta de armazenamento do cliente.

      ![Ative o diagnóstico passo 3](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Clique no link **ativar os diagnósticos**. Esta ação abre o da interface do Usuário para a configuração de conta de armazenamento. 

      ![Ativar o passo de diagnóstico 4](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. Introduza o nome da definição no campo **Name** e selecione **arquivo para uma conta de armazenamento** caixa de verificação para que os relatórios de dados podem começar a fluir em à conta de armazenamento.

      ![Ative o diagnóstico passo 5](./media/backup-azure-configure-reports/select-setting-name.png)
6. Clique em Seletor de conta de armazenamento e selecione a conta de armazenamento e a subscrição relevante na lista para armazenar dados de relatórios e clique em **OK**.

      ![Selecione o passo de conta de armazenamento 6](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Selecione **AzureBackupReport** caixa na secção de registo de verificação e mover o controlo de deslize para o período de retenção Selecione para isso, dados de relatórios. Relatórios de dados na conta de armazenamento é mantido durante o período selecionado utilizar este controlo de deslize.

      ![Guardar o passo de conta de armazenamento 7](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Reveja todas as alterações e clique em **guardar** botão na parte superior, conforme mostrado na figura acima. Esta ação garante que todas as alterações são guardadas e conta de armazenamento está agora configurada para armazenar dados de relatórios.

9. A tabela de definições de diagnóstico deve agora mostrar a nova definição ativada para o cofre. Se ele não aparecer, atualize a tabela para ver a definição atualizada.

      ![Passo 9 da definição de diagnóstico do Vista](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Depois de configurar os relatórios ao guardar a conta de armazenamento, deve **Aguarde 24 horas** para push de dados inicial concluir. Deve importar o pacote de conteúdos de cópia de segurança do Azure no Power BI apenas após esse tempo. Consultar [secção de FAQ](#frequently-asked-questions) para obter mais detalhes. 
>
>

## <a name="view-reports-in-power-bi"></a>Ver relatórios no Power BI 
Após configurar a conta de armazenamento para comunica com o Cofre de serviços de recuperação, demora cerca de 24 horas para dados de relatórios para começarem a fluir. Após 24 horas de configuração de conta de armazenamento, utilize os seguintes passos para ver relatórios no Power BI:
1. [Inicie sessão no](https://powerbi.microsoft.com/landing/signin/) para o Power BI.
2. Clique em **obter dados** e clique em **obter** sob **serviços** na biblioteca de pacote de conteúdos. Utilize os passos mencionados [documentação do Power BI para aceder ao pacote de conteúdos](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![Importar o pacote de conteúdos](./media/backup-azure-configure-reports/content-pack-import.png)
3. Tipo **Azure Backup** na barra de pesquisa e clique em **obter agora**.

      ![Obter pacote de conteúdos](./media/backup-azure-configure-reports/content-pack-get.png)
4. Introduza o nome da conta de armazenamento configurada no passo 5 acima e clique em **seguinte** botão.

    ![Introduza o nome da conta de armazenamento](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Introduza a chave de conta de armazenamento para esta conta de armazenamento. Pode [ver e copiar chaves de acesso de armazenamento](../storage/common/storage-create-storage-account.md#manage-your-storage-account) ao navegar para a sua conta de armazenamento no portal do Azure. 

     ![Introduza a conta de armazenamento](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Clique em **iniciar sessão** botão. Após o início de sessão for bem-sucedida, obtém **importação de dados** notificação.

    ![Importar o pacote de conteúdos](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Depois de algum tempo, obterá **êxito** notificação após a importação estiver concluída. Poderá demorar pouco mais para importar o pacote de conteúdos, se houver muitos dados na conta de armazenamento.
    
    ![Importar o pacote de conteúdos de sucesso](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Assim que os dados são importados com êxito, **Azure Backup** pacote de conteúdos é visível na **aplicações** no painel de navegação. A lista mostra agora o dashboard de cópia de segurança do Azure, relatórios e conjunto de dados com uma estrela amarela indicando relatórios recentemente importados. 

     ![Pacote de conteúdos de cópia de segurança do Azure](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Clique em **Azure Backup** em Dashboards, que mostra um conjunto de relatórios afixados de chave.

      ![Dashboard de cópia de segurança do Azure](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Para ver o conjunto completo de relatórios, clique em qualquer relatório no dashboard.

      ![Integridade de tarefa de cópia de segurança do Azure](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Clique em cada separador nos relatórios para ver os relatórios nessa área.

      ![Separadores de relatórios de cópia de segurança do Azure](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes
1. **Como posso ver se os dados de relatórios foi iniciada a fluir na conta de armazenamento?**
    
    Pode aceder à conta de armazenamento configurada e selecionar contentores. Se o contentor tem uma entrada para insights-logs-azurebackupreport, ele indica que dados de relatórios foi iniciada a fluir.

2. **O que é a frequência de push de dados para a conta de armazenamento e o pacote de conteúdos de cópia de segurança do Azure no Power BI?**

   Para os utilizadores de dia 0, demoraria cerca de 24 horas para enviar dados para a conta de armazenamento. Quando este push inicial estiver concluída, os dados são atualizados com a frequência seguinte mostrada a figura abaixo. 
      * Dados relacionados com **trabalhos, alertas, itens de cópia de segurança, cofres, servidores protegidos e políticas** são emitidos via push para a conta de armazenamento do cliente como e quando é registado.
      * Dados relacionados com **armazenamento** são emitidos via push para a conta de armazenamento do cliente a cada 24 horas.
   
    ![Frequência de push de dados de relatórios de cópia de segurança do Azure](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  O Power BI tem um [atualização agendada uma vez por dia](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Pode efetuar uma atualização manual dos dados no Power BI para o pacote de conteúdos.

3. **Quanto posso manter os relatórios?** 

   Ao configurar a conta de armazenamento, pode selecionar o período de retenção de dados de relatórios na conta de armazenamento (utilizando o passo 6 na configuração da conta de armazenamento para a secção de relatórios acima). Além disso, pode [relatórios de análise no excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) e salvá-los por um período de retenção mais longo, de acordo com suas necessidades. 

4. **Irá ver todos os meus dados em relatórios depois de configurar a conta de armazenamento?**

   Todos os dados gerados após **"configurando a conta de armazenamento"** será enviada para a conta de armazenamento e estarão disponíveis em relatórios. No entanto, **não são enviadas em tarefas de curso** para relatórios. Assim que a tarefa é concluída ou falha, ele é enviado para relatórios.

5. **Se eu já tiver configurado a conta de armazenamento para ver relatórios, posso alterar a configuração para utilizar outra conta de armazenamento?** 

   Sim, pode alterar a configuração para apontar para uma conta de armazenamento diferente. Deve utilizar a conta de armazenamento recentemente configurado ao ligar ao pacote de conteúdos do Azure Backup. Além disso, quando uma conta de armazenamento diferente estiver configurada, novos dados seriam fluir nesta conta de armazenamento. Mas os dados mais antigos (antes de alterar a configuração) seriam ainda permanecem na conta de armazenamento mais antiga.

6. **Posso ver os relatórios em cofres e entre subscrições?** 

   Sim, pode configurar a mesma conta de armazenamento em vários cofres para ver relatórios de cross-cofre. Além disso, pode configurar a mesma conta de armazenamento para cofres entre subscrições. Em seguida, pode utilizar esta conta de armazenamento ao ligar ao pacote de conteúdos de cópia de segurança do Azure no Power BI para visualizar os relatórios. No entanto, a conta de armazenamento selecionada deve ser na mesma região que o Cofre de serviços de recuperação.
   
## <a name="troubleshooting-errors"></a>Resolução de erros
| Detalhes do erro | Resolução |
| --- | --- |
| Depois de configurar a conta de armazenamento para relatórios de cópia de segurança, **conta de armazenamento** continua a mostrar **não configurada**. | Se tiver configurado a conta de armazenamento com êxito, os dados de relatórios irão fluir no apesar deste problema. Para resolver este problema, aceda ao portal do Azure > todos os serviços > definições de diagnóstico > RS cofre > Editar definição. Eliminar a definição configurada anteriormente e criar uma nova definição a partir do painel do mesmo. Desta vez, defina o campo **Name** ao **serviço**. Isto deve mostrar a conta de armazenamento configurada. |
|Depois de importar a cópia de segurança do Azure de pacote no Power BI, o erro de conteúdos **404-container não está** é exibido. | Como sugerido neste documento, têm de aguardar durante 24 horas depois de configurar relatórios no cofre dos serviços de recuperação para vê-los corretamente no Power BI. Se tentar aceder aos relatórios antes de 24 horas, irá obter este erro, uma vez que completa de dados ainda não está presente para mostrar relatórios válidos. |

## <a name="next-steps"></a>Passos Seguintes
Agora que configurou a conta de armazenamento e o pacote de conteúdos do Azure Backup importado, a próxima etapa é personalizar estes relatórios e utilizar relatórios de dados de modelo para criar relatórios. Consulte os artigos seguintes para obter mais detalhes.

* [Utilizando o modelo de dados de relatórios do Azure Backup](backup-azure-reports-data-model.md)
* [Filtragem de relatórios no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Criar relatórios no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

