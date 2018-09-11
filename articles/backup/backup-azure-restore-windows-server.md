---
title: Restaurar dados no Azure a um Windows Server ou o computador do Windows
description: Saiba como restaurar dados armazenados no Azure a um Windows Server ou computador Windows.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 9/7/2018
ms.author: saurse
ms.openlocfilehash: 20d2f289f4d40d773fde9f6b770dc49b87c34804
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297252"
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Restaurar ficheiros para um servidor Windows ou para um computador cliente Windows com o modelo de implementação Resource Manager

Este artigo explica como restaurar dados a partir de um cofre de cópia de segurança. Para restaurar os dados, utilize o assistente recuperar dados no agente dos serviços de recuperação do Azure (MARS) da Microsoft. Ao restaurar dados, é possível:

* Restaure os dados na mesma máquina a partir do qual as cópias de segurança foram executadas.
* Restaure dados numa máquina alternativa.

Em Janeiro de 2017, a Microsoft lançou uma atualização de pré-visualização para o agente MARS. Juntamente com correções de erros, esta atualização permite restaurar instantânea, que lhe permite montar um instantâneo de ponto de recuperação gravável como um volume de recuperação. Em seguida, pode explorar os ficheiros de volume e cópia de recuperação para um computador local, assim, de forma seletiva restaurar os ficheiros.

> [!NOTE]
> O [Janeiro de 2017 atualização da cópia de segurança do Azure](https://support.microsoft.com/en-us/help/3216528?preview) é necessário se quiser usar a restauração instantânea para restaurar os dados. Também os dados de cópia de segurança devem ser protegidos em cofres em localidades listadas no artigo de suporte. Consulte a [Janeiro de 2017 atualização da cópia de segurança do Azure](https://support.microsoft.com/en-us/help/3216528?preview) para a lista mais recente das regiões que suportam a restaurar instantânea. É o restauro imediato **não** atualmente disponível em todas as localidades.
>

Use a restauração do instantâneo com os cofres dos serviços de recuperação no portal do Azure. Se tiver armazenado dados nos cofres de cópia de segurança, eles foram convertidos para cofres dos serviços de recuperação. Se quiser usar a restauração do instantâneo, transferir a atualização de MARS e siga os procedimentos que mencionam restaurar instantânea.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Usar a restauração instantânea para recuperar os dados na mesma máquina

Se um ficheiro eliminado acidentalmente e deseja restaurá-lo na mesma máquina (do qual a cópia de segurança está a ser utilizada), os passos seguintes irão ajudá-lo a recuperar os dados.

1. Abra o **Microsoft Azure Backup** ajuste no. Se não souber qual o snap de em foi instalado, procure o computador ou servidor para **Microsoft Azure Backup**.

    A aplicação de ambiente de trabalho deve aparecer nos resultados da pesquisa.

2. Clique em **recuperar dados** para iniciar o assistente.

    ![Recuperar dados](./media/backup-azure-restore-windows-server/recover.png)

3. Sobre o **introdução** painel, para restaurar os dados no mesmo servidor ou computador, selecione **neste servidor (`<server name>`)** e clique em **seguinte**.

    ![Escolha esta opção de servidor para restaurar os dados na mesma máquina](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Sobre o **selecionar modo de recuperação** painel, escolha **ficheiros e pastas individuais** e, em seguida, clique em **seguinte**.

    ![Procurar ficheiros](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
> [!IMPORTANT]
> A opção de restaurar *ficheiros e pastas individuais* requer o .NET Framework 4.5.2 ou superior. Se não vir a *ficheiros e pastas individuais* opção, tem de atualizar .NET Framework versão 4.5.2 ou superior e tente novamente.

> [!TIP]
> O *ficheiros e pastas individuais* permite a opção para um acesso rápido aos dados de ponto de recuperação. É adequado para a recuperação de arquivos individuais, com tamanhos totalling não mais de 80 GB de tamanho e ofertas de transferência/Copiar acelera MBps até 6 durante a recuperação. O *Volume* opção recupera todas as cópias de segurança de dados num determinado Volume. Esta opção fornece velocidades de transferência (até 60 MBps), que é ideal para a recuperação de grande tamanho de dados ou volumes inteiros.

5. Sobre o **selecionar Volume e data** painel, selecione o volume que contém os ficheiros de e/ou pastas que pretende restaurar.

    No calendário, selecione um ponto de recuperação. Pode restaurar a partir de qualquer ponto de recuperação no tempo. As datas em **negrito** indicam a disponibilidade de, pelo menos, um ponto de recuperação. Depois de selecionar uma data, se vários pontos de recuperação disponíveis, selecione o ponto de recuperação específica a partir da **tempo** menu pendente.

    ![Data e de volume](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Depois de escolher o ponto de recuperação para restaurar, clique em **montar**.

    O Azure Backup monta o ponto de recuperação local e o usa como um volume de recuperação.

7. Sobre o **procurar e recuperar arquivos** painel, clique em **procurar** para abrir o Explorador do Windows e encontrar os ficheiros e pastas que pretende.

    ![Opções de recuperação](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. No Explorador do Windows, copie os ficheiros de e/ou pastas que pretende restaurar e cole-os em qualquer localização local para o servidor ou computador. Pode abrir ou transmitir ficheiros diretamente a partir do volume de recuperação e certifique-se de que está a recuperar as versões corretas.

    ![Copie e cole os ficheiros e pastas a partir do volume montado para localização local](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)


9. Quando tiver terminado de restaurar os ficheiros de e/ou pastas, sobre o **procurar e ficheiros de recuperação** painel, clique em **Unmount**. Em seguida, clique em **Sim** para confirmar que pretende desmontar o volume.

    ![Desmontar o volume e confirmar](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Se não clicar Unmount, o Volume de recuperação permanecerão montado durante 6 horas desde a hora quando estiver montada. No entanto, o tempo de montagem é expandido até um máximo de 24 horas em caso de uma cópia de ficheiros em curso. Não existem operações de cópia de segurança serão executados enquanto o volume está montado. Qualquer operação de cópia de segurança agendada para ser executada durante o período de tempo quando o volume está montado, será executado depois do volume de recuperação é desmontado.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Usar a restauração instantânea para restaurar dados para uma máquina alternativa
Se o servidor completo é perdido, pode continuar a recuperar dados de cópia de segurança do Azure para uma máquina diferente. Os passos seguintes mostram o fluxo de trabalho.


Inclui a terminologia usada nestes passos:

* *Máquina de origem* – a máquina original a partir da cópia de segurança e que está atualmente indisponível.
* *Máquina de destino* – a máquina para que os dados são recuperados.
* *Cofre de exemplo* – o Cofre dos serviços de recuperação a para o qual o *máquina de origem* e *máquina de destino* estão registados. <br/>

> [!NOTE]
> As cópias de segurança não não possível restaurar um computador de destino que executa uma versão anterior do sistema operativo. Por exemplo, uma cópia de segurança obtida a partir de um 7 do Windows computador pode ser restaurada no Windows 8 ou posterior, computador. Não é possível restaurar uma cópia de segurança obtida a partir de um computador com Windows 8 para um computador com Windows 7.
>
>

1. Abra o **Microsoft Azure Backup** ajustar no *máquina de destino*.

2. Certifique-se a *máquina de destino* e o *máquina de origem* estão registados no mesmo cofre dos serviços de recuperação.

3. Clique em **recuperar dados** para abrir o **assistente recuperar dados**.

    ![Recuperar dados](./media/backup-azure-restore-windows-server/recover.png)

4. Sobre o **introdução** painel, selecione **outro servidor**

    ![Outro servidor](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Disponibilize o ficheiro de credenciais de cofre que corresponde da *Cofre de exemplo*e clique em **próxima**.

    Se o ficheiro de credenciais do Cofre é inválido (ou expirou), transfira um novo ficheiro de credenciais do cofre do *cofre exemplo* no portal do Azure. Depois de fornecer uma credencial de cofre válido, o nome do Cofre de cópia de segurança correspondente é exibido.


6. Na **selecionar servidor de cópia de segurança** painel, selecione a *máquina de origem* na lista de máquinas apresentadas e forneça a frase de acesso. Clique depois em **Seguinte**.

    ![Lista de máquinas](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Sobre o **selecionar modo de recuperação** painel, selecione **ficheiros e pastas individuais** e clique em **seguinte**.

    ![Pesquisa](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Sobre o **selecionar Volume e data** painel, selecione o volume que contém os ficheiros de e/ou pastas que pretende restaurar.

    No calendário, selecione um ponto de recuperação. Pode restaurar a partir de qualquer ponto de recuperação no tempo. As datas em **negrito** indicam a disponibilidade de, pelo menos, um ponto de recuperação. Depois de selecionar uma data, se vários pontos de recuperação disponíveis, selecione o ponto de recuperação específica a partir da **tempo** menu pendente.

    ![Procurar itens](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Clique em **montar** localmente montar o ponto de recuperação como um volume de recuperação no seu *máquina de destino*.

10. Sobre o **procurar e recuperar arquivos** painel, clique em **procurar** para abrir o Explorador do Windows e encontrar os ficheiros e pastas que pretende.

    ![Encriptação](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. No Explorador do Windows, copie os ficheiros de e/ou pastas do volume de recuperação e colá-los para sua *máquina de destino* localização. Pode abrir ou transmitir ficheiros diretamente a partir do volume de recuperação e certifique-se de que as versões corretas são recuperadas.

    ![Encriptação](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Quando tiver terminado de restaurar os ficheiros de e/ou pastas, sobre o **procurar e ficheiros de recuperação** painel, clique em **Unmount**. Em seguida, clique em **Sim** para confirmar que pretende desmontar o volume.

    ![Encriptação](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Se não clicar Unmount, o Volume de recuperação permanecerão montado durante 6 horas desde a hora quando estiver montada. No entanto, o tempo de montagem é expandido até um máximo de 24 horas em caso de uma cópia de ficheiros em curso. Não existem operações de cópia de segurança serão executados enquanto o volume está montado. Qualquer operação de cópia de segurança agendada para ser executada durante o período de tempo quando o volume está montado, será executado depois do volume de recuperação é desmontado.
    >

## <a name="next-steps"></a>Passos Seguintes
* Agora que recuperar ficheiros e pastas, pode [gerir as cópias de segurança](backup-azure-manage-windows-server.md).
