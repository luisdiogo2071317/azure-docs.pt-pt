---
title: "Sincronização do Azure Active Directory Connect: configurar a localização de dados preferencial para Georreplicação várias capacidades no Office 365 | Microsoft Docs"
description: "Descreve como colocar os recursos de utilizador do Office 365 próximo do utilizador com a sincronização do Azure Active Directory Connect."
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
ms.date: 01/31/2018
ms.author: billmath
ms.openlocfilehash: a5ebd61539af7116b8f92cdf9404cd2b5cdea193
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Sincronização do Azure Active Directory Connect: configurar a localização de dados preferencial para os recursos do Office 365
É o objetivo deste tópico explica como configurar o atributo de localização de dados preferencial na sincronização Connect do Azure Active Directory (Azure AD). Quando alguém utiliza Georreplicação várias capacidades no Office 365, pode utilizar este atributo para designar a geolocalização os dados do utilizador do Office 365. (Os termos de licenciamento *região* e *georreplicação* -no alternadamente.)

> [!IMPORTANT]
> Várias Georreplicação está atualmente em pré-visualização. Se pretende aderir ao programa de pré-visualização, contacte o seu representante da Microsoft.
>
>

## <a name="enable-synchronization-of-preferred-data-location"></a>Ativar a sincronização de localização de dados preferencial
Por predefinição, os recursos do Office 365 para os seus utilizadores estão localizados na mesma geografia como inquilino do Azure AD. Por exemplo, se o seu inquilino está localizado na América do Norte, em seguida, caixas de correio dos utilizadores Exchange estão também localizadas na América do Norte. Para uma organização multinational, isto poderá não ser o ideal.

Ao definir o atributo **preferredDataLocation**, pode definir georreplicação de um utilizador. Pode ter do Office 365 recursos o utilizador, tais como a caixa de correio e o OneDrive, na mesma geografia que o utilizador e continuará a ter um inquilino para toda a organização.

> [!IMPORTANT]
> Para ser elegível para várias Georreplicação, tem de ter, pelo menos, 5 000 utilizadores individuais na sua subscrição do Office 365.
>
>

É possível encontrar uma lista de todos os geos para o Office 365 no [é onde os dados localizados?](https://aka.ms/datamaps).

Geos no Office 365 disponíveis para várias Georreplicação são:

| Geo | valor de preferredDataLocation |
| --- | --- |
| Ásia-Pacífico | APC |
| Austrália | AUS |
| Canadá | CAN |
| União Europeia | EUR |
| Índia | IND |
| Japão | JPN |
| Coreia do Sul | KOR |
| Reino Unido | GBR |
| Estados Unidos | NAM |

* Se um georreplicação não está listada nesta tabela (por exemplo, América do Sul), em seguida,-não pode ser utilizado para várias Georreplicação.
* Índia e Sul Coreia geos só estão disponíveis para os clientes com endereços de faturação e de licenças adquiridas nesses geos.
* Nem todas as cargas de trabalho do Office 365 suportam a utilização de definição georreplicação de um utilizador.

### <a name="azure-ad-connect-support-for-synchronization"></a>Suporte do Azure AD Connect para sincronização

Azure AD Connect suporta a sincronização do **preferredDataLocation** atributo para **utilizador** objetos na versão 1.1.524.0 e mais tarde. Especificamente:

* O esquema do tipo de objeto **utilizador** no conector Azure AD é expandido para incluir o **preferredDataLocation** atributo. O atributo é do tipo de cadeia de valor único.
* O esquema do tipo de objeto **pessoa** no metaverso é expandido para incluir o **preferredDataLocation** atributo. O atributo é do tipo de cadeia de valor único.

Por predefinição, **preferredDataLocation** não está ativado para sincronização. Esta funcionalidade foi concebida para organizações de maior. Também tem de identificar um atributo para conter a georreplicação do Office 365 para os seus utilizadores, porque não há nenhum **preferredDataLocation** atributo no Active Directory no local. Isto vai ser diferentes para cada organização.

> [!IMPORTANT]
> Azure AD permite que o **preferredDataLocation** atributo no **objetos de utilizador de nuvem** diretamente ser configurada utilizando o Azure AD PowerShell. Azure AD permite que já não é o **preferredDataLocation** atributo no **sincronizados objetos de utilizador** diretamente ser configurada utilizando o Azure AD PowerShell. Para configurar este atributo na **sincronizados objetos de utilizador**, tem de utilizar o Azure AD Connect.

Antes de ativar a sincronização:

* Decida que atributo do Active Directory no local para ser utilizado como o atributo de origem. Deve ser do tipo, **cadeia de valor único**. Nos passos que se seguem, uma do **extensionAttributes** é utilizado.
* Se tiver configurado anteriormente o **preferredDataLocation** atributo em existente **sincronizados objetos de utilizador** no Azure AD com o Azure AD PowerShell, tem de backport valores de atributo para o correspondente **utilizador** objetos no Active Directory no local.

    > [!IMPORTANT]
    > Se o fizer não backport estes valores, o Azure AD Connect remove os valores de atributo existentes no Azure AD quando a sincronização para o **preferredDataLocation** atributo estiver activado.

* Configure agora o atributo de origem em, pelo menos, alguns objetos de utilizador do Active Directory no local. Pode utilizá-lo para a verificar mais tarde.

As secções seguintes fornecem os passos para ativar a sincronização do **preferredDataLocation** atributo.

> [!NOTE]
> Os passos são descritos no contexto de uma implementação do Azure AD com a topologia de floresta única e sem as regras de sincronização personalizados. Se tiver uma topologia de várias floresta, as regras de sincronização personalizados configurados ou tem um servidor de teste, deverá ajustar os passos em conformidade.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Passo 1: Desativar do agendador de sincronização e certifique-se de que existe, está sem sincronização em curso
Para evitar alterações indesejadas que está a ser exportadas para o Azure AD, certifique-se de que nenhuma sincronização ocorre enquanto estiver a atualizar as regras de sincronização. Para desativar o agendador de sincronização incorporada:

1. Inicie uma sessão do PowerShell no servidor do Azure AD Connect.
2. Desativar a sincronização agendada, executando este cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Iniciar o **Synchronization Service Manager** acedendo a **iniciar** > **serviço de sincronização**.
4. Selecione o **operações** separador e confirme que não há nenhuma operação com o estado *em curso*.

![Captura de ecrã de sincronização do Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>Passo 2: Adicionar o atributo de origem para o esquema de conector do Active Directory no local
Nem todos os atributos do Azure AD são importados para o espaço de conector do Active Directory no local. Se tiver optado por utilizar um atributo que não está sincronizado por predefinição, em seguida, terá de importá-la. Para adicionar o atributo de origem para a lista dos atributos importados:

1. Selecione o **conectores** separador o Synchronization Service Manager.
2. O conector do Active Directory no local com o botão direito e selecione **propriedades**.
3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
4. Certifique-se de que o atributo de origem que tiver selecionado utilizar está selecionado na lista de atributos. Se não vir o atributo, selecione o **Mostrar tudo** caixa de verificação.
5. Para guardar, selecione **OK**.

![Caixa de diálogo de propriedades e captura de ecrã de sincronização do Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Passo 3: Adicionar **preferredDataLocation** o esquema do conector do Azure AD
Por predefinição, o **preferredDataLocation** atributo não é importado para o espaço de conector do Azure AD. Para o adicionar à lista de atributos importados:

1. Selecione o **conectores** separador o Synchronization Service Manager.
2. O conector do Azure AD com o botão direito e selecione **propriedades**.
3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
4. Selecione o **preferredDataLocation** atributo na lista.
5. Para guardar, selecione **OK**.

![Caixa de diálogo de propriedades e captura de ecrã de sincronização do Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule"></a>Passo 4: Criar uma regra de sincronização de entrada
A regra de sincronização de entrada permite que o valor do atributo a fluir do atributo de origem no Active Directory no local para o metaverso.

1. Iniciar o **Editor de regras de sincronização** acedendo a **iniciar** > **Editor de regras de sincronização**.
2. Definir o filtro de pesquisa **direção** ser **entrada**.
3. Para criar uma nova regra de entrada, selecione **Adicionar nova regra**.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | --- | --- | --- |
    | Nome | *Forneça um nome* | Por exemplo, "do AD – utilizador preferredDataLocation" |
    | Descrição | *Forneça uma descrição personalizada* |  |
    | Sistema ligado | *Escolha o conector do Active Directory no local* |  |
    | Tipo de objeto de sistema ligado | **utilizador** |  |
    | Tipo de objeto de Metaverso | **Person** |  |
    | Tipo de ligação | **Associar** |  |
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Manter o **Scoping filtro** vazio, inclua todos os objetos. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.
6. Vá para o **separador de transformação**e implementar a seguinte regra de transformação:

    | Tipo de fluxo | Atributo de destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    |Direto | preferredDataLocation | Escolha o atributo de origem | Desmarcada | Atualizar |

7. Para criar regra de entrada, selecione **adicionar**.

![Captura de ecrã de criar a regra de sincronização de entrada](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule"></a>Passo 5: Criar uma regra de sincronização de saída
A regra de sincronização de saída permite que o valor de atributo para o fluxo do metaverso para o **preferredDataLocation** atributo no Azure AD:

1. Vá para o **Editor de regras de sincronização**.
2. Definir o filtro de pesquisa **direção** ser **saída**.
3. Selecione **Adicionar nova regra**.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | ----- | ------ | --- |
    | Nome | *Forneça um nome* | Por exemplo, "Out para o Azure AD – utilizador preferredDataLocation" |
    | Descrição | *Forneça uma descrição* ||
    | Sistema ligado | *Selecione o conector Azure AD* ||
    | Tipo de objeto de sistema ligado | **utilizador** ||
    | Tipo de objeto de Metaverso | **Person** ||
    | Tipo de ligação | **Associar** ||
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Vá para o **Scoping filtro** separador e adicionar um único grupo de filtro de âmbito com dois cláusulas:

    | Atributo | Operador | Valor |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Utilizador |
    | cloudMastered | NOTEQUAL | Verdadeiro |

    Filtro de âmbito determina qual é aplicada esta regra de sincronização de saída para os objetos do Azure AD. Neste exemplo, utilizamos o mesmo filtro de âmbito de "Out AD – a identidade do utilizador" regra de sincronização de OOB (out-of-box). Impede que a regra de sincronização ser aplicada a **utilizador** objetos que não estão sincronizados do Active Directory no local. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.

6. Vá para o **transformação** separador e implementar a seguinte regra de transformação:

    | Tipo de fluxo | Atributo de destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    | Direto | preferredDataLocation | preferredDataLocation | Desmarcada | Atualizar |

7. Fechar **adicionar** para criar a regra de saída.

![Captura de ecrã de criar a regra de sincronização de saída](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Passo 6: Executar o ciclo de sincronização completa
Em geral, é necessário o ciclo de sincronização completa. Isto acontece porque tiver adicionado novos atributos para o Active Directory e o esquema do conector do Azure AD e introduzidas as regras de sincronização personalizados. Certifique-se de que as alterações antes de exportá-los para o Azure AD. Pode utilizar os seguintes passos para verificar as alterações, ao executar manualmente os passos que compõem um ciclo de sincronização completa.

1. Executar **completa importação** no conector do Active Directory no local:

   1. Vá para o **operações** separador o Synchronization Service Manager.
   2. Clique com botão direito do **conector do Active Directory no local**e selecione **executar**.
   3. Na caixa de diálogo, selecione **importação completa**e selecione **OK**.
   4. Aguarde a conclusão da operação.

    > [!NOTE]
    > Pode ignorar a importação completa no conector do Active Directory no local se o atributo de origem já está incluído na lista de atributos importados. Por outras palavras, precisa de fazer qualquer alteração durante o passo 2, anteriormente neste artigo.

2. Executar **completa importação** no conector Azure AD:

   1. Clique com botão direito do **conector do Azure AD**e selecione **executar**.
   2. Na caixa de diálogo, selecione **importação completa**e selecione **OK**.
   3. Aguarde a conclusão da operação.

3. Verificar as alterações de regra de sincronização no existente **utilizador** objeto.

   O atributo de origem a partir do Active Directory no local, e **preferredDataLocation** do Azure AD, tiverem sido importados para cada espaço de conector correspondentes. Antes de prosseguir com o passo de sincronização completa, efetuar uma pré-visualização em existente **utilizador** objeto no espaço de conector do Active Directory no local. O objeto selecionado que deve ter o atributo de origem preenchido. Uma versão de pré-visualização com êxito com **preferredDataLocation** preenchido no metaverso é um bom indicador de que configurou a sincronização regras corretamente. Para obter informações sobre como fazer uma versão de pré-visualização, consulte [verificar a alteração](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Executar **completa sincronização** no conector do Active Directory no local:

   1. Clique com botão direito do **conector do Active Directory no local**e selecione **executar**.
   2. Na caixa de diálogo, selecione **sincronização completa**e selecione **OK**.
   3. Aguarde a conclusão da operação.

5. Certifique-se **exportações pendentes** para o Azure AD:

   1. Clique com botão direito do **conector do Azure AD**e selecione **espaço de conector de pesquisa**.
   2. No **espaço de conector de pesquisa** caixa de diálogo:

        a. Definir **âmbito** para **pendentes exportação**.<br>
        b. Selecione todas as três caixas de verificação, incluindo **adicionar, modificar e eliminar**.<br>
        c. Para ver a lista de objetos com alterações seja exportado, selecione **pesquisa**. Para examinar as alterações de determinado objecto, faça duplo clique o objeto.<br>
        d. Certifique-se de que as alterações esperadas.

6. Executar **exportar** no **conector Azure AD**

   1. Clique com botão direito do **conector do Azure AD**e selecione **executar**.
   2. No **executar conector** caixa de diálogo, selecione **exportar**e selecione **OK**.
   3. Aguarde a conclusão da operação.

> [!NOTE]
> É possível que repare que os passos não incluem o passo de sincronização completa no conector Azure AD ou o passo de exportação no conector do Active Directory. Os passos não são necessários, porque os valores de atributo fluem do Active Directory no local ao Azure AD apenas.

## <a name="step-7-re-enable-sync-scheduler"></a>Passo 7: Volte a ativar do agendador de sincronização
Volte a ativar o agendador de sincronização incorporada:

1. Inicie uma sessão do PowerShell.
2. Volte a ativar a sincronização agendada, executando este cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Passo 8: Verificar o resultado
Agora está na altura de verificar a configuração e ativá-la para os seus utilizadores.

1. Adicione a georreplicação para o atributo selecionado num utilizador. A lista de geos disponível pode ser encontrada na [esta tabela](#enable-synchronization-of-preferreddatalocation).  
![Captura de ecrã do atributo de AD adicionado a um utilizador](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Aguarde que o atributo sejam sincronizados com o Azure AD.
3. Através do Exchange Online PowerShell, certifique-se de que a região de caixa de correio foi definida corretamente.  
![Captura de ecrã do Exchange Online PowerShell](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Pressupondo que o inquilino foi marcado para poder utilizar esta funcionalidade, a caixa de correio é movida para a georreplicação correta. Isto pode ser verificado ao observar o nome do servidor onde está localizada a caixa de correio.
4. Para verificar que esta definição foi eficaz através de várias caixas de correio, utilize o script no [Galeria de TechNet](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Este script também tem uma lista dos prefixos de servidor de todos os centros de dados do Office 365 e que georreplicação está localizado no. Pode ser utilizado como referência no passo anterior para verificar a localização da caixa de correio.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre várias geográfica no Office 365:

* [Georreplicação de várias sessões no Ignite](https://aka.ms/MultiGeoIgnite)
* [Várias geográfica no OneDrive](https://aka.ms/OneDriveMultiGeo)
* [Várias geográfica no SharePoint Online](https://aka.ms/SharePointMultiGeo)

Saiba mais sobre o modelo de configuração no motor de sincronização:

* Saiba mais sobre o modelo de configuração [compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Saiba mais sobre o idioma de expressão no [expressões compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

Tópicos de descrição geral:

* [Sincronização do Azure AD Connect: Noções e personalizar a sincronização](active-directory-aadconnectsync-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
