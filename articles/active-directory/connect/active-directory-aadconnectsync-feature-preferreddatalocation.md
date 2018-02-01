---
title: "Sincronização do Azure AD Connect: configurar a localização de dados preferencial para utilizadores do Office 365 | Microsoft Docs"
description: "Descreve como colocar os recursos de utilizador do Office 365 próximo do utilizador com a sincronização do Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2018
ms.author: billmath
ms.openlocfilehash: 73b9b8d208b5eac2e62f62ab786efafa056e3cb4
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Sincronização do Azure AD Connect: configurar a localização de dados preferencial para os recursos do Office 365
O objetivo se este tópico explica como configurar preferredDataLocation na sincronização do Azure AD Connect. Este atributo é utilizado para indicar ao Office 365 em que o utilizador está localizado para que os recursos podem ser colocados próximo do utilizador. Esta funcionalidade destina-se a clientes maior.

> [!IMPORTANT]
> Esta funcionalidade está atualmente em pré-visualização e desativado por predefinição na nuvem. Se pretende aderir ao programa de pré-visualização, contacte o seu representante da Microsoft.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>Ativar a sincronização de PreferredDataLocation
Por predefinição, os recursos do Office 365 para os seus utilizadores estão localizados na mesma região que o inquilino do Azure AD. Por exemplo, se o seu inquilino está localizado na América do Norte, em seguida, as caixas de correio do Exchange de utilizadores são também localizadas na América do Norte. Para uma organização com vários national Isto poderá não ser o ideal. Ao definir o atributo preferredDataLocation pode ser definida a região do utilizador.

Pelas definições este atributo, pode ter recursos do Office 365 do utilizador, tais como a caixa de correio e o OneDrive, na mesma região que o utilizador e continuará a ter um inquilino para toda a organização.

> [!IMPORTANT]
> Para ser elegível para esta funcionalidade tem de ter menos de 5000 utilizadores individuais na sua subscrição do Office 365.
>
>

As regiões no Office 365 são:

| Região | Descrição |
| --- | --- |
| NAM | América do Norte |
| EUR | Europa |
| APC | Ásia-Pacífico |
| JPN | Japão |
| AUS | Austrália |
| CAN | Canadá |
| GBR | Grã-Bretanha |
| LAM | América Latina |

Nem todas as cargas de trabalho do Office 365 suporta a utilização de definição de região de um utilizador.

Azure AD Connect suporta a sincronização do **PreferredDataLocation** atributo para **utilizador** objetos na versão 1.1.524.0 e depois. Mais especificamente, as seguintes alterações foram introduzidas:

* O esquema do tipo de objeto **utilizador** no conector Azure AD é expandido para incluir o atributo PreferredDataLocation, que é do tipo cadeia de valor único.

* O esquema do tipo de objeto **pessoa** no Metaverso é expandido para incluir o atributo de PreferredDataLocation, o que é do tipo cadeia e é o valor único.

Por predefinição, o atributo PreferredDataLocation não está ativado para sincronização. Esta funcionalidade foi concebida para organizações de maior e não todos seriam beneficiar do mesmo. Tem também de identificar um atributo para conter a região do Office 365 para os seus utilizadores, uma vez que não existe nenhum atributo PreferredDataLocation no Active Directory no local. Isto vai ser diferentes para cada organização.

> [!IMPORTANT]
> Atualmente, Azure AD permite que o atributo PreferredDataLocation no sincronizados objetos de utilizador e nuvem utilizador objetos diretamente a ser configurado com o Azure AD PowerShell. Assim que tiver ativado a sincronização do atributo PreferredDataLocation, tem de parar a utilizar o Azure AD PowerShell para configurar o atributo em **sincronizados objetos de utilizador** como o Azure AD Connect irá substitui-los com base nos valores de atributo de origem no Active Directory no local.

> [!IMPORTANT]
> Desde 1 de Setembro de 2017, Azure AD já não permite que o atributo PreferredDataLocation no **sincronizados objetos de utilizador** diretamente seja configurado com o Azure AD PowerShell. Para configurar o atributo PreferredLocation em objetos de utilizador sincronizados, tem de utilizar o Azure AD Connect.

Antes de ativar a sincronização do atributo PreferredDataLocation, tem de:

 * Primeiro, tem de decidir que atributo do Active Directory no local para ser utilizado como o atributo de origem. Deve ser do tipo **cadeia de valor único**. Os passos abaixo de um do extensionAttributes são utilizados.

 * Se configurou anteriormente o atributo PreferredDataLocation no existente sincronizados objetos de utilizador no Azure AD com o Azure AD PowerShell, tem **backport** os valores de atributo para os objetos de utilizador correspondentes no Active Directory no local.

    > [!IMPORTANT]
    > Se o fizer não backport os valores de atributo para os objetos de utilizador correspondentes no Active Directory no local, o Azure AD Connect irá remover os valores de atributo existentes no Azure AD quando a sincronização para o atributo PreferredDataLocation está ativada.

 * É recomendável que configure o atributo de origem em, pelo menos, duas no local utilizador AD objetos agora, que pode ser utilizado para a verificar mais tarde.

Os passos para ativar a sincronização do atributo PreferredDataLocation podem ser resumidos como:

1. Desativar o agendador de sincronização e certifique-se de que existe, está sem sincronização em curso
2. Adicione o atributo de origem para o esquema de conector do ADDS no local
3. Adicionar PreferredDataLocation o esquema de conector do Azure AD
4. Criar uma regra de sincronização de entrada para passar o valor do atributo do Active Directory no local
5. Criar uma regra de sincronização de saída para passar o valor do atributo com o Azure AD
6. Execute o ciclo de sincronização completa
7. Ativar o agendador de sincronização
8. Verificar o resultado

> [!NOTE]
> O resto desta secção aborda estes passos nos detalhes. Estas são descritas no contexto de uma implementação do Azure AD com topologia de floresta única e sem as regras de sincronização personalizados. Se tiver uma topologia de várias floresta, as regras de sincronização personalizados configurados ou tem um servidor de teste, terá de ajustar os passos em conformidade.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Passo 1: Desativar do agendador de sincronização e certifique-se de que existe, está sem sincronização em curso
Certifique-se de que não sincronização ocorre enquanto estiver a atualizar as regras de sincronização para evitar alterações indesejadas que está a ser exportadas para o Azure AD. Para desativar o agendador de sincronização incorporada:

1. Inicie uma sessão do PowerShell no servidor do Azure AD Connect.
2. Desativar a sincronização agendada, executando o cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Iniciar o **Synchronization Service Manager** acedendo a **iniciar** > **serviço de sincronização**.
4. Vá para o **operações** separador e confirme que não há nenhuma operação com o estado *em curso*.

![Verificação de Gestor do serviço de sincronização - não existem operações em curso](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>Passo 2: Adicionar o atributo de origem para o esquema de conector do ADDS no local
Nem todos os atributos de AD são importados para o local espaço de conector do AD. Se tiver optado por utilizar um atributo não sincronizado por predefinição, em seguida, terá de importá-la. Para adicionar o atributo de origem para a lista dos atributos importados:

1. Vá para o **conectores** separador o Synchronization Service Manager.
2. Clique com botão direito do **conector AD no local** e selecione **propriedades**.
3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
4. Certifique-se de que o atributo de origem que tiver selecionado utilizar está selecionado na lista de atributos. Se não vir o atributo, em seguida, clique na caixa de verificação "Mostrar tudo".
5. Clique em **OK** para guardar.

![Adicione o atributo de origem para o local esquema conector AD](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Passo 3: Adicionar PreferredDataLocation o esquema de conector do Azure AD
Por predefinição, o atributo PreferredDataLocation não é importado para o espaço de conector do Azure AD. Para adicionar o atributo PreferredDataLocation à lista de atributos importados:

1. Vá para o **conectores** separador o Synchronization Service Manager.
2. Clique com botão direito do **conector do Azure AD** e selecione **propriedades**.
3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
4. Selecione o atributo de preferredDataLocation na lista de atributos.
5. Clique em **OK** para guardar.

![Adicione o atributo de origem para o esquema do conector do Azure AD](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Passo 4: Criar uma regra de sincronização de entrada para passar o valor do atributo do Active Directory no local
A regra de sincronização de entrada permite que o valor de atributo para o fluxo a partir do atributo de origem do Active Directory no local para o Metaverso:

1. Iniciar o **Editor de regras de sincronização** acedendo a **iniciar** > **Editor de regras de sincronização**.
2. Definir o filtro de pesquisa **direção** ser **entrada**.
3. Clique em **Adicionar nova regra** botão para criar uma regra de entrada novo.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | --- | --- | --- |
    | Nome | *Forneça um nome* | Por exemplo, *"do AD – utilizador PreferredDataLocation"* |
    | Descrição | *Forneça uma descrição personalizada* |  |
    | Sistema ligado | *Escolha no local conector AD* |  |
    | Tipo de objeto de sistema ligado | **Utilizador** |  |
    | Tipo de objeto de Metaverso | **Person** |  |
    | Tipo de ligação | **Associar** |  |
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Manter o **Scoping filtro** vazio para incluir todos os objetos. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.
6. Vá para o **separador de transformação** e implementar a seguinte regra de transformação:

    | Tipo de fluxo | Atributo de Destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    |Direto | PreferredDataLocation | Escolha o atributo de origem | Desmarcada | Atualizar |

7. Clique em **adicionar** para criar a regra de entrada.

![Criar regra de sincronização de entrada](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Passo 5: Criar uma regra de sincronização de saída para passar o valor do atributo com o Azure AD
A regra de sincronização de saída permite que o valor de atributo para o fluxo do Metaverso ao atributo PreferredDataLocation no Azure AD:

1. Vá para o **as regras de sincronização** Editor.
2. Definir o filtro de pesquisa **direção** ser **saída**.
3. Clique em **Adicionar nova regra** botão.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | ----- | ------ | --- |
    | Nome | *Forneça um nome* | Por exemplo, "enviados ao AAD – utilizador PreferredDataLocation" |
    | Descrição | *Forneça uma descrição* ||
    | Sistema ligado | *Seleccione o conector AAD* ||
    | Tipo de objeto de sistema ligado | Utilizador ||
    | Tipo de objeto de Metaverso | **Person** ||
    | Tipo de ligação | **Associar** ||
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Vá para o **Scoping filtro** separador e adicione um **único grupo de filtro de âmbito com dois cláusulas**:

    | Atributo | Operador | Valor |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Utilizador |
    | cloudMastered | NOTEQUAL | Verdadeiro |

    Filtro de âmbito determina qual é aplicada esta regra de sincronização de saída para os objetos do Azure AD. Neste exemplo, utilizamos o mesmo filtro de âmbito de "Out AD – a identidade do utilizador" regra de sincronização de OOB. Impede que a regra de sincronização que está a ser aplicadas a objetos de utilizador que não estão sincronizados do Active Directory no local. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.

6. Vá para o **transformação** separador e implementar a seguinte regra de transformação:

    | Tipo de fluxo | Atributo de Destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    | Direto | PreferredDataLocation | PreferredDataLocation | Desmarcada | Atualizar |

7. Fechar **adicionar** para criar a regra de saída.

![Criar regra de sincronização de saída](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Passo 6: Ciclo de sincronização completa de execução
Em geral, o ciclo de sincronização completa é necessário uma vez que, foram adicionados novos atributos para ambos os AD e esquema de conector do Azure AD e as regras de sincronização personalizados introduzidos. Recomenda-se que verificar as alterações antes de exportá-los para o Azure AD. Pode utilizar os seguintes passos para verificar as alterações ao executar manualmente os passos que compõem um ciclo de sincronização completa.

1. Executar **completa importação** passo no **conector AD no local**:

   1. Vá para o **operações** separador o Synchronization Service Manager.
   2. Clique com botão direito do **conector AD no local** e selecione **executar...** .
   3. Na caixa de diálogo de pop-up, selecione **importação completa** e clique em **OK**.
   4. Aguarde pela conclusão da operação.

    > [!NOTE]
    > Pode ignorar a importação completa no no local conector AD Se o atributo de origem já está incluído na lista de importadas atributos. Por outras palavras, não tinha que fazer qualquer alteração durante [passo 2: adicionar o atributo de origem para o local esquema conector AD](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema).

2. Executar **completa importação** passo no **conector do Azure AD**:

   1. Clique com botão direito do **conector do Azure AD** e selecione **executar...**
   2. Na caixa de diálogo de pop-up, selecione **importação completa** e clique em **OK**.
   3. Aguarde pela conclusão da operação.

3. Verificar as alterações de regra de sincronização de um objeto de utilizador existente:

O atributo de origem no local do Active Directory e PreferredDataLocation do Azure AD foram importados para o espaço de conector correspondentes. Antes de continuar com o passo de sincronização completa, recomenda-se que efetue uma **pré-visualização** num utilizador existente objeto no local espaço de conector do AD. O objeto selecionado que deve ter o atributo de origem preenchido. Um bem-sucedida **pré-visualização** com PreferredDataLocation preenchido no Metaverso é um bom indicador de que configurou a sincronização regras corretamente. Para obter informações sobre como fazer uma **pré-visualização**, consulte a secção [verificar a alteração](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Executar **sincronização completa** passo no **conector AD no local**:

   1. Clique com botão direito do **conector AD no local** e selecione **executar...** .
   2. Na caixa de diálogo de pop-up, selecione **sincronização completa** e clique em **OK**.
   3. Aguarde pela conclusão da operação.

5. Certifique-se **exportações pendentes** para o Azure AD:

   1. Clique com botão direito do **conector do Azure AD** e selecione **espaço de conector de pesquisa**.
   2. Na caixa de diálogo de pop-up da espaço de conector de pesquisa:

      1. Definir **âmbito** para **pendentes exportação**.
      2. Verifique todas as três caixas de verificação, incluindo **adicionar, modificar e eliminar**.
      3. Clique em de **pesquisa** botão para obter a lista de objetos com as alterações para ser exportada. Para examinar as alterações de determinado objecto, faça duplo clique o objeto.
      4. Certifique-se de que as alterações são esperadas.

6. Executar **exportar** passo no **conector do Azure AD**

   1. Clique com botão direito do **conector do Azure AD** e selecione **executar...** .
   2. Na caixa de diálogo de pop-up a executar o conector, selecione **exportar** e clique em **OK**.
   3. Aguarde que a exportação para o Azure AD para concluir.

> [!NOTE]
> Poderá reparar que os passos não incluem o passo de sincronização completa no conector do Azure AD e exportar no conector AD. Os passos não são necessários, uma vez que os valores de atributo fluem do Active Directory no local ao Azure AD apenas.

## <a name="step-7-re-enable-sync-scheduler"></a>Passo 7: Volte a ativar do agendador de sincronização
Volte a ativar o agendador de sincronização incorporada:

1. Inicie a sessão do PowerShell.
2. Volte a ativar a sincronização agendada, executando o cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Passo 8: Verificar o resultado
Agora está na altura de verificar a configuração e ativá-la para os seus utilizadores.

1. Adicione a região para o atributo selecionado num utilizador. A lista de regiões disponíveis pode ser encontrada no [esta tabela](#enable-synchronization-of-preferreddatalocation).  
![Atributo de AD adicionado a um utilizador](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Aguarde que o atributo sejam sincronizados com o Azure AD.
3. Através do Exchange Online PowerShell, certifique-se de que a região de caixa de correio foi definida corretamente.  
![A região de caixa de correio definir um utilizador no Exchange Online](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Pressupondo que o inquilino foi marcado para poder utilizar esta funcionalidade, a caixa de correio é movida para a região correta. Isto pode ser verificado ao observar o nome do servidor onde está localizada a caixa de correio.
4. Para verificar que esta definição foi eficaz através de várias caixas de correio, utilize o script no [Galeria de Technet](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Este script também tem uma lista de todos os prefixos de servidor de centros de dados do Office 365 e que região, este ficará localizado no. Pode ser utilizado como referência no passo anterior para verificar a localização da caixa de correio.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre o modelo de configuração [compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Saiba mais sobre o idioma de expressão no [expressões compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Tópicos de descrição geral**

* [Sincronização do Azure AD Connect: Noções e personalizar a sincronização](active-directory-aadconnectsync-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
