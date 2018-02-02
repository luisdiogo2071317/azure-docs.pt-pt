---
title: "Sincronização do Azure AD Connect: efetuar uma alteração na sincronização do Azure AD Connect de configuração | Microsoft Docs"
description: "Explica como efetuar uma alteração à configuração na sincronização do Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2018
ms.author: billmath
ms.openlocfilehash: e97d3e3e35ee87864c5d38e75e08e62088e25fdb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Sincronização do Azure AD Connect: efetuar uma alteração para a configuração predefinida
O objetivo deste artigo é para guiá-lo como efetuar alterações à configuração de predefinição na sincronização Connect do Azure Active Directory (Azure AD). Fornece os passos para alguns cenários comuns. Com este conhecimento, deverá conseguir efetuar alterações simples à sua própria configuração com base nas suas regras de negócio.

## <a name="synchronization-rules-editor"></a>Editor de regras de sincronização
O Editor de regras de sincronização é utilizado para ver e alterar a configuração predefinida. Pode encontrá-lo no **iniciar** menu sob o **do Azure AD Connect** grupo.  
![Menu Iniciar com o Editor de regras de sincronização](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Quando abrir o editor, verá as regras de out-of-box predefinidas.

![Editor de regras de sincronização](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navegar no editor
Utilizar listas pendentes na parte superior do editor, pode localizar rapidamente uma regra específica. Por exemplo, se pretender ver as regras em que está incluído o atributo proxyAddresses, pode alterar listas pendentes para o seguinte:  
![Filtragem de SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Para repor a filtragem e carregar uma configuração nova, prima F5 no teclado.

No canto superior direito a **Adicionar nova regra** botão. Utilize este botão para criar as suas próprias regras personalizadas.

Na parte inferior são botões para agir de uma regra de sincronização selecionada. **Editar** e **eliminar** fazer o que esperar-lhes. **Exportar** produz um script do PowerShell para voltar a criar a regra de sincronização. Com este procedimento, pode mover uma regra de sincronização de um servidor para outro.

## <a name="create-your-first-custom-rule"></a>Criar a primeira regra personalizada
As alterações mais comuns são para os fluxos de atributos. Os dados no seu diretório de origem não podem ser os mesmos que no Azure AD. O exemplo nesta secção, certifique-se o nome dado do utilizador é sempre em *maiúsculas e minúsculas adequada*.

### <a name="disable-the-scheduler"></a>Desativar o programador
O [programador](active-directory-aadconnectsync-feature-scheduler.md) executa cada 30 minutos por predefinição. Certifique-se de que não está a iniciar enquanto estiver a efetuar alterações e as novas regras de resolução de problemas. Para desativar temporariamente o programador, inicie o PowerShell e execute `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Desativar o programador](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Criar a regra
1. Clique em **Adicionar nova regra**.
2. No **Descrição** página, introduza o seguinte:  
   ![Regra de filtragem de entrada](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * **Nome**: atribua a regra de um nome descritivo.
   * **Descrição**: dê algum esclarecimentos para outra pessoa pode compreender o que está a regra para.
   * **Ligado sistema**: Este é o sistema em que é possível encontrar o objeto. Neste caso, selecione **conector do Active Directory**.
   * **Tipo de objeto de sistema/Metaverso ligado**: selecione **utilizador** e **pessoa**, respetivamente.
   * **Tipo de ligação**: alterar este valor para **associar**.
   * **Precedência**: forneça um valor que seja exclusivo no sistema. Um valor numérico inferior indica precedência superior.
   * **Etiqueta**: deixar isto em branco. Apenas as regras de out-of-box da Microsoft devem ter esta caixa preenchida com um valor.
3. No **Scoping filtro** página, introduza **givenName ISNOTNULL**.  
   ![Regra de filtro de âmbito de entrada](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Esta secção é utilizada para definir a quais os objetos que deve ser aplicada a regra. Se for deixado em branco, a regra seria aplicada a todos os objetos de utilizador. No entanto, que incluem gabinetes de conferências, contas de serviço e outros objetos de utilizador não pessoas.
4. No **associar regras** página, deixar o campo vazio.
5. No **transformações** página, altere **FlowType** para **expressão**. Para **atributo Target**, selecione **givenName**. E para **origem**, introduza **PCase([givenName])**.
   ![Transformações de regra de entrada](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   O motor de sincronização é maiúsculas e minúsculas para o nome de função e o nome do atributo. Se escrever um problema, ver um aviso quando adicionar a regra. Pode guardar e continuar, mas é necessário reabrir e corrija a regra.
6. Clique em **adicionar** para guardar a regra.

A nova regra personalizada deve estar visível com outras regras de sincronização no sistema.

### <a name="verify-the-change"></a>Certifique-se a alteração
Com esta nova alteração, deve certificar-se de que está a funcionar conforme esperado e não está a gerar erros. Dependendo do número de objetos que tiver, existem duas formas para efetuar este passo:

- Execute uma sincronização completa em todos os objetos.
- Execute uma pré-visualização e a sincronização completa num único objeto.

Abra o **serviço de sincronização** do **iniciar** menu. Os passos nesta secção estão todos nesta ferramenta.

**Sincronização completa no todos os objetos**  

   1. Selecione **conectores** na parte superior. Identificar o conector que alterados na secção anterior (neste caso, serviços de domínio do Active Directory) e selecione-o. 
   2. Para **ações**, selecione **executar**.
   3. Selecione **sincronização completa**e, em seguida, selecione **OK**.
   ![Sincronização completa](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Agora, os objetos são atualizados no metaverso. Certifique-se as suas alterações ao observar o objeto no metaverso.

**Pré-visualização e a sincronização completa num único objeto**  

   1. Selecione **conectores** na parte superior. Identificar o conector que alterados na secção anterior (neste caso, serviços de domínio do Active Directory) e selecione-o.
   2. Selecione **procurar espaço de conector**. 
   3. Utilizar **âmbito** para localizar um objeto que pretende utilizar para testar a alteração. Selecione o objeto e clique em **pré-visualização**. 
   4. No novo ecrã, selecione **consolidar pré-visualização**.  
   ![Consolidar pré-visualização](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   A alteração é consolidada agora para o metaverso.

**Ver o objeto no metaverso**  

1. Escolha alguns objetos de exemplo para se certificar de que o valor é esperado e que a regra é aplicada. 
2. Selecione **pesquisa de Metaverso** do nível superior. Adicione qualquer filtro que precisa para localizar os objetos relevantes. 
3. O resultado da pesquisa, abra um objeto. Observe os valores de atributo e também verificar no **regras de sincronização** coluna que a regra aplicada como esperado.  
![Pesquisa de metaversos](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Ativar o programador
Se tudo está conforme esperado, pode ativar o Programador de novo. A partir do PowerShell, execute `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Outras alterações de fluxo de atributos comuns
A secção anterior descrito como efetuar alterações a um fluxo de atributos. Nesta secção, são fornecidos alguns exemplos adicionais. Os passos sobre como criar a regra de sincronização é abreviada, mas pode encontrar os passos completos na secção anterior.

### <a name="use-an-attribute-other-than-the-default"></a>Utilize um atributo diferente da predefinição
Neste cenário Fabrikam, há uma floresta onde alfabeto local é utilizado para o nome especificado, o apelido e o nome a apresentar. A representação de carateres latinos destes atributos pode ser encontrada em atributos de extensão. Para criar um endereço global lista no Azure AD e do Office 365, a organização pretende utilizar em vez disso, estes atributos.

Com uma configuração predefinida, um objeto da floresta local este aspeto:  
![Fluxo de atributos 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Para criar uma regra com outros fluxos de atributos, efetue o seguinte:

1. Abra o **Editor de regras de sincronização** do **iniciar** menu.
2. Com **entrada** ainda selecionada para a esquerda, clique em de **Adicionar nova regra** botão.
3. Atribua a regra, um nome e descrição. Selecione a instância do Active Directory no local e os tipos de objetos relevantes. No **tipo de ligação**, selecione **associar**. Para **precedência**, escolha um número que não seja utilizado por outra regra. As regras de out-of-box começar com 100, pelo que o valor de 50 pode ser utilizado neste exemplo.
  ![Fluxo de atributos 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
4. Deixe **Scoping filtro** vazio. (Ou seja, se devem aplicar a todos os objetos de utilizador na floresta.)
5. Deixe **associar regras** vazio. (Ou seja, permitir que a regra de out-of-box processar todas as associações.)
6. No **transformações**, criar os fluxos do seguintes:  
  ![Fluxo de atributos 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
7. Clique em **adicionar** para guardar a regra.
8. Aceda a **Gestor do serviço de sincronização**. No **conectores**, seleccione o conector do qual adicionou a regra. Selecione **executar**e, em seguida, selecione **sincronização completa**. Uma sincronização completa recalcula todos os objetos utilizando as regras atuais.

Este é o resultado para o mesmo objeto com esta regra personalizada:  
![Fluxo de atributos 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Comprimento de atributos
Atributos de cadeia são indexable por predefinição e o comprimento máximo é de 448 caracteres. Se estiver a trabalhar com atributos de cadeia que podem conter mais, certifique-se incluir o seguinte no fluxo de atributos:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>Alterar o userPrincipalSuffix
O atributo userPrincipalName no Active Directory não é conhecido sempre pelos utilizadores e não pode ser adequado como o ID de início de sessão. Com o Assistente de instalação de sincronização do Azure AD Connect, pode escolher um atributo diferente – por exemplo, *correio*. Mas, em alguns casos, deve ser calculado o atributo.

Por exemplo, a empresa Contoso tem dois diretórios do Azure AD, uma para produção e outro para testes. Os utilizadores no seu inquilino de teste para utilizar outro sufixo no ID de início de sessão que pretendem:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

Nesta expressão demorar tudo esquerdo do primeiro @-sign (Word) e concatenate com uma cadeia de caracteres fixa.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Converter um atributo de valor múltiplos de valor único
Alguns atributos no Active Directory são com múltiplos valores no esquema, mesmo que procuram valor único no Active Directory de diretório de utilizadores e computadores. Um exemplo é o atributo de descrição:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Nesta expressão, se o atributo tem um valor, o primeiro item demorar (*Item*) no atributo, remover espaços à esquerda e (*compactar*) e, em seguida, mantenha os primeiro 448 caracteres (*à esquerda* ) na cadeia.

### <a name="do-not-flow-an-attribute"></a>Fluxo não de um atributo
Para em segundo plano no cenário nesta secção, consulte [controlar o processo de fluxo de atributos](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Existem duas formas de fluxo não de um atributo. O primeiro é utilizando o Assistente de instalação para [remover atributos selecionados](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Esta opção funciona se nunca tiver sincronizado o atributo antes. No entanto, se tiver iniciado para sincronizar este atributo e removê-lo mais tarde com esta funcionalidade, a deixa de motor de sincronização gerir o atributo e os valores existentes é mantida no Azure AD.

Se pretender remover o valor de um atributo e certifique-se de não ser fluxo no futuro, tem de criar uma regra personalizada.

Neste cenário Fabrikam, ter Observámos que alguns dos atributos que sincronizar com a nuvem não devem ser não existe. Queremos certificar-se de que estes atributos são removidos do Azure AD.  
![Atributos de extensão incorreta](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

1. Criar uma nova regra de sincronização de entrada e preencher a descrição.
  ![Descrições](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
2. Criar fluxos de atributos com **expressão** para **FlowType** e com **AuthoritativeNull** para **origem**. O literal **AuthoritativeNull** indica que o valor deve estar em branco no metaverso, mesmo se uma regra de sincronização de precedência inferior tenta povoar o valor.
  ![Transformação para atributos de extensão](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
3. Guarde a regra de sincronização. Iniciar o **serviço de sincronização**, localize o conector, selecione **executar**e, em seguida, selecione **sincronização completa**. Este passo recalcula todos os fluxos de atributos.
4. Certifique-se de que as alterações pretendidas estão prestes a ser exportado ao pesquisar o espaço de conector.
  ![Eliminar faseada](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Criar regras com o PowerShell
Utilizando o editor de regra de sincronização funciona bem quando só tem de fazer algumas alterações. Se precisar de efetuar alterações muitos, PowerShell pode ser uma opção melhor. Algumas funcionalidades avançadas só estão disponíveis com o PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Obter o script do PowerShell para uma regra de out-of-box
Para ver o script que criou uma regra de out-of-box, selecione a regra no editor de regras de sincronização e clique em do PowerShell **exportar**. Esta ação dá-lhe o script do PowerShell que criou a regra.

### <a name="advanced-precedence"></a>Precedência avançada
As regras de sincronização de out-of-box comecem com um valor de precedência de 100. Se tiver várias florestas, e terá de efetuar várias alterações personalizadas, em seguida, as regras de sincronização de 99 poderão não ser suficiente.

Pode instruir o motor de sincronização que pretende que sejam introduzidas antes das regras de out-of-box de regras adicionais. Para obter este comportamento, siga estes passos:

1. Marcar a primeira regra de sincronização de out-of-box (**da associação do utilizador do AD**) no editor de regras de sincronização e selecione **exportar**. Copie o valor do identificador de SR.  
![PowerShell antes de alteração](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Crie a nova regra de sincronização. Pode utilizar o editor de regras de sincronização para criá-la. Exporte a regra para um script do PowerShell.
3. Na propriedade **PrecedenceBefore**, inserir o valor do identificador da regra de out-of-box. Definir o **precedência** para **0**. Certifique-se de que o atributo de identificador é exclusivo e que não está a reutilizar um GUID de outra regra. Certifique-se também que o **ImmutableTag** propriedade não está definida. Esta propriedade deve ser definida apenas para uma regra de out-of-box.
4. Guarde o script do PowerShell e executá-la. O resultado é que a regra personalizada é atribuída o valor de precedência de 100 e todas as outras regras de out-of-box são incrementadas.  
![PowerShell após a alteração](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Pode ter várias regras de sincronização personalizados utilizando o mesmo **PrecedenceBefore** valor quando necessário.

## <a name="enable-synchronization-of-usertype"></a>Ativar a sincronização de UserType
Azure AD Connect suporta a sincronização do **UserType** atributo para **utilizador** objetos na versão 1.1.524.0 e mais tarde. Mais especificamente, as seguintes alterações foram introduzidas:

- O esquema do tipo de objeto **utilizador** no conector Azure AD é expandido para incluir o atributo UserType, que é do tipo cadeia e é o valor único.
- O esquema do tipo de objeto **pessoa** no metaverso é expandido para incluir o atributo UserType, que é do tipo cadeia e é o valor único.

Por predefinição, o atributo UserType não está ativado para a sincronização porque não existe nenhum atributo UserType correspondente no Active Directory no local. Tem de ativar manualmente sincronização. Antes de efetuar este procedimento, tem de tome nota do seguinte comportamento imposta pelo Azure AD:

- Azure AD só aceita dois valores para o atributo UserType: **membro** e **convidado**.
- Se o atributo UserType não está ativado para sincronização no Azure AD Connect, utilizadores do Azure AD criados através da sincronização de diretórios teria o atributo UserType definido como **membro**.
- Azure AD não permite que o atributo UserType no existentes do Azure AD que os utilizadores ser alterada pelo Azure AD Connect. Só pode ser definido durante a criação de utilizadores do Azure AD.

Antes de ativar a sincronização do atributo UserType, tem primeiro de decidir como o atributo é derivado do Active Directory no local. Seguem-se as abordagens mais comuns:

- Designar um não utilizados no local atributo de AD (por exemplo, extensionAttribute1) para ser utilizado como o atributo de origem. Designado no local de atributo de AD deve ser do tipo **cadeia**, valor único e conter o valor **membro** ou **convidado**. 

    Se escolher esta abordagem, tem de garantir que o atributo designado é preenchido com o valor correto para todos os utilizadores objetos existentes no Active Directory no local que são sincronizados com o Azure AD antes de ativar a sincronização do atributo UserType .

- Em alternativa, pode derivar de valor para o atributo UserType a outras propriedades. Por exemplo, pretende sincronizar todos os utilizadores como **convidado** se local atributo userPrincipalName de AD termina com a parte do domínio  *@partners.fabrikam123.org* . 

    Como mencionado anteriormente, o Azure AD Connect não permite que o atributo UserType no existentes do Azure AD que os utilizadores ser alterada pelo Azure AD Connect. Por conseguinte, tem de garantir que a lógica que escolheu é consistente com a forma como o atributo UserType já está configurado para todos os utilizadores do Azure AD existentes no seu inquilino.

Os passos para ativar a sincronização do atributo UserType podem ser resumidos como:

1.  Desativar o agendador de sincronização e certifique-se de que existe, está sem sincronização em curso.
2.  Adicione o atributo de origem para o local esquema conector AD.
3.  Adicione o UserType ao esquema do conector do Azure AD.
4.  Crie uma regra de sincronização de entrada para passar o valor do atributo do Active Directory no local.
5.  Crie uma regra de sincronização de saída para passar o valor do atributo com o Azure AD.
6.  Execute um ciclo de sincronização completa.
7.  Ative o agendador de sincronização.

>[!NOTE]
> O resto desta secção aborda estes passos. Estas são descritas no contexto de uma implementação do Azure AD com topologia de floresta única e sem as regras de sincronização personalizados. Se tiver uma topologia de várias floresta, as regras de sincronização personalizados configurados ou tem um servidor de teste, terá de ajustar os passos em conformidade.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Passo 1: Desative o agendador de sincronização e certifique-se de que existe, está sem sincronização em curso
Para evitar a exportação não intencional de alterações para o Azure AD, certifique-se de que nenhuma sincronização ocorre enquanto estiver a atualizar as regras de sincronização. Para desativar o agendador de sincronização incorporada:

 1. Inicie uma sessão do PowerShell no servidor do Azure AD Connect.
 2. Desativar a sincronização agendada, executando o cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Abra o Synchronization Service Manager, acedendo a **iniciar** > **serviço de sincronização**.
 4. Vá para o **operações** separador e confirme que não há nenhuma operação com um Estado de *em curso*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Passo 2: Adicionar o atributo de origem para o local esquema conector AD
Nem todos os atributos do Azure AD são importados para o local espaço de conector do AD. Para adicionar o atributo de origem para a lista dos atributos importados:

 1. Vá para o **conectores** separador o Synchronization Service Manager.
 2. Conector do AD de rato no local e selecione **propriedades**.
 3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
 4. Certifique-se de que o atributo de origem está selecionado na lista de atributos.
 5. Clique em **OK** para guardar.
![Adicione o atributo de origem para o local esquema conector AD](./media/active-directory-aadconnectsync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>Passo 3: Adicionar o UserType ao esquema do conector do Azure AD
Por predefinição, o atributo UserType não é importado para o Azure AD Connect espaço em. Para adicionar o atributo UserType à lista de atributos importados:

 1. Vá para o **conectores** separador o Synchronization Service Manager.
 2. Clique com botão direito do **conector do Azure AD** e selecione **propriedades**.
 3. Na caixa de diálogo de pop-up, vá para o **selecionar atributos** separador.
 4. Certifique-se de que o atributo PreferredDataLocation está selecionado na lista de atributos.
 5. Clique em **OK** para guardar.

![Adicione o atributo de origem para o esquema do conector do Azure AD](./media/active-directory-aadconnectsync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Passo 4: Criar uma regra de sincronização de entrada para passar o valor do atributo do Active Directory no local
A regra de sincronização de entrada permite que o valor de atributo para o fluxo a partir do atributo de origem do Active Directory no local para o metaverso:

1. Abra o Editor de regras de sincronização, acedendo a **iniciar** > **Editor de regras de sincronização**.
2. Definir o filtro de pesquisa **direção** ser **entrada**.
3. Clique em de **Adicionar nova regra** botão para criar uma regra de entrada novo.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | --- | --- | --- |
    | Nome | *Forneça um nome* | Por exemplo, *do AD – UserType do utilizador* |
    | Descrição | *Forneça uma descrição* |  |
    | Sistema ligado | *Escolha no local conector AD* |  |
    | Tipo de objeto de sistema ligado | **Utilizador** |  |
    | Tipo de objeto de Metaverso | **Person** |  |
    | Tipo de ligação | **Associar** |  |
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Vá para o **Scoping filtro** separador e adicione um **único grupo de filtro de âmbito** com a cláusula seguinte:

    | Atributo | Operador | Valor |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Utilizador\_ |

    O filtro de âmbito determina qual no local AD objetos que esta regra de sincronização de entrada é aplicada. Neste exemplo, utilizamos o filtro de âmbito mesmo utilizado o *do AD – utilizador comuns* regra de sincronização de out-of-box, o que impede que a regra de sincronização a ser aplicadas a objetos de utilizador criados através do utilizador do Azure AD funcionalidade de repetição de escrita. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.

6. Vá para o **transformação** separador e implementar a regra de transformação pretendido. Por exemplo, se tiver designado uma não utilizados no local atributo de AD (por exemplo, extensionAttribute1) como o atributo de origem para o UserType, pode implementar um fluxo de atributos direta:

    | Tipo de fluxo | Atributo de destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    | Direto | UserType | extensionAttribute1 | Desmarcada | Atualizar |

    Noutro exemplo, pretende o valor para o atributo UserType deriva de outras propriedades. Por exemplo, pretende sincronizar todos os utilizadores como convidado se local atributo userPrincipalName de AD termina com a parte do domínio  *@partners.fabrikam123.org* . Pode implementar uma expressão como esta:

    | Tipo de fluxo | Atributo de destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    | Direto | UserType | IIf(IsPresent([userPrincipalName]),IIF(CBool(Instr(LCase([userPrincipalName]),"@partners.fabrikam123.org")=0) "Membro", "Convidado"), erro ("UserPrincipalName não está presente para determinar UserType")) | Desmarcada | Atualizar |

7. Clique em **adicionar** para criar a regra de entrada.

![Criar regra de sincronização de entrada](./media/active-directory-aadconnectsync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Passo 5: Criar uma regra de sincronização de saída para passar o valor do atributo com o Azure AD
A regra de sincronização de saída permite que o valor de atributo para o fluxo do metaverso ao atributo PreferredDataLocation no Azure AD:

1. Aceda ao Editor de regras de sincronização.
2. Definir o filtro de pesquisa **direção** ser **saída**.
3. Clique em de **Adicionar nova regra** botão.
4. Sob o **Descrição** separador, forneça a seguinte configuração:

    | Atributo | Valor | Detalhes |
    | ----- | ------ | --- |
    | Nome | *Forneça um nome* | Por exemplo, *Out para o AAD – UserType do utilizador* |
    | Descrição | *Forneça uma descrição* ||
    | Sistema ligado | *Seleccione o conector AAD* ||
    | Tipo de objeto de sistema ligado | **Utilizador** ||
    | Tipo de objeto de Metaverso | **Person** ||
    | Tipo de ligação | **Associar** ||
    | Precedência | *Escolha um número entre 1 a 99* | 1 a 99 está reservado para regras de sincronização personalizados. Não escolha um valor que é utilizado por outra regra de sincronização. |

5. Vá para o **Scoping filtro** separador e adicione um **único grupo de filtro de âmbito** com dois cláusulas:

    | Atributo | Operador | Valor |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Utilizador |
    | cloudMastered | NOTEQUAL | Verdadeiro |

    O filtro de âmbito determina a que a do Azure AD objetos que esta regra de sincronização de saída é aplicada. Neste exemplo, utilizamos o filtro de âmbito mesmo do *horizontalmente para AD – a identidade do utilizador* regra de sincronização de out-of-box. Impede que a regra de sincronização que está a ser aplicado a objetos de utilizador que não estão sincronizados do Active Directory no local. Poderá ter de otimizar o filtro de âmbito, de acordo com a implementação do Azure AD Connect.

6. Vá para o **transformação** separador e implementar a seguinte regra de transformação:

    | Tipo de fluxo | Atributo de destino | Origem | Aplicar uma vez | Tipo de intercalação |
    | --- | --- | --- | --- | --- |
    | Direto | UserType | UserType | Desmarcada | Atualizar |

7. Clique em **adicionar** para criar a regra de saída.

![Criar regra de sincronização de saída](./media/active-directory-aadconnectsync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>Passo 6: Executar um ciclo de sincronização completa
Em geral, um ciclo de sincronização completa é necessário porque é ter adicionado novos atributos esquemas do Active Directory e o conector do Azure AD e introduzidas as regras de sincronização personalizados. Pretende verificar as alterações antes de exportá-los para o Azure AD. 

Pode utilizar os seguintes passos para verificar as alterações ao executar manualmente os passos que compõem um ciclo de sincronização completa.

1. Executar um **completa importação** no **conector AD no local**:

   1. Vá para o **operações** separador o Synchronization Service Manager.
   2. Clique com botão direito do **conector AD no local** e selecione **executar**.
   3. Na caixa de diálogo de pop-up, selecione **importação completa** e, em seguida, clique em **OK**.
   4. Aguarde a conclusão da operação.

    > [!NOTE]
    > Pode ignorar uma importação completa no no local conector AD Se o atributo de origem já está incluído na lista de importadas atributos. Por outras palavras, não tinha que efetuar quaisquer alterações durante [passo 2: adicionar o atributo de origem para o local esquema conector AD](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Executar um **completa importação** no **conector do Azure AD**:

   1. Clique com botão direito do **conector do Azure AD** e selecione **executar**.
   2. Na caixa de diálogo de pop-up, selecione **importação completa** e, em seguida, clique em **OK**.
   3. Aguarde a conclusão da operação.

3. Verificar as alterações de regra de sincronização de um objeto de utilizador existente:

    O atributo de origem no local do Active Directory e UserType do Azure AD foram importados para os respetivos espaços de conector. Antes de prosseguir com uma sincronização completa, efetuar uma **pré-visualização** num utilizador existente objeto no local espaço de conector do AD. O objeto que selecionou deve ter o atributo de origem preenchido.
    
    Um bem-sucedida **pré-visualização** com UserType preenchido no metaverso é um bom indicador de que configurou a sincronização regras corretamente. Para obter informações sobre como fazer uma **pré-visualização**, consulte a secção [verificar a alteração](#verify-the-change).

4. Executar um **completa sincronização** no **conector AD no local**:

   1. Clique com botão direito do **conector AD no local** e selecione **executar**.
   2. Na caixa de diálogo de pop-up, selecione **sincronização completa** e, em seguida, clique em **OK**.
   3. Aguarde a conclusão da operação.

5. Certifique-se **exportações pendentes** para o Azure AD:

   1. Clique com botão direito do **conector do Azure AD** e selecione **espaço de conector de pesquisa**.
   2. No **espaço de conector de pesquisa** caixa de diálogo de pop-up:

      - Definir **âmbito** para **pendentes exportação**.
      - Selecione as três caixas de verificação: **adicionar**, **modificar**, e **eliminar**.
      - Clique em de **pesquisa** botão para obter a lista de objetos com as alterações para ser exportada. Para examinar as alterações de determinado objecto, faça duplo clique o objeto.
      - Certifique-se de que as alterações esperadas.

6. Executar **exportar** no **conector Azure AD**:

   1. Clique com botão direito do **conector do Azure AD** e selecione **executar**.
   2. No **executar conector** caixa de diálogo de pop-up, selecione **exportar** e, em seguida, clique em **OK**.
   3. Aguarde que a exportação para o Azure AD para concluir.

> [!NOTE]
> Estes passos não incluem a sincronização completa e exportar os passos no conector Azure AD. Estes passos não são necessários porque os valores de atributo fluem do Active Directory no local ao Azure AD apenas.

### <a name="step-7-re-enable-the-sync-scheduler"></a>Passo 7: Volte a ativar o agendador de sincronização
Volte a ativar o agendador de sincronização incorporada:

1. Inicie uma sessão do PowerShell.
2. Volte a ativar a sincronização agendada, executando o cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre o modelo de configuração [compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Saiba mais sobre o idioma de expressão no [expressões compreender de aprovisionamento declarativo](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Tópicos de descrição geral**

* [Sincronização do Azure AD Connect: Noções e personalizar a sincronização](active-directory-aadconnectsync-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
