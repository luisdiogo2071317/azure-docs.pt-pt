---
title: Resolver problemas de um objeto que não está a sincronizar com o Azure AD | Documentos da Microsoft
description: Resolver problemas relacionados com a razão pela qual um objeto não está a sincronizar com o Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 4336cabd256e492981e1bbff8d1b3b9e4ef07df1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820526"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad"></a>Resolver problemas de um objeto que não está a sincronizar com o Azure AD

Se um objeto não está a sincronizar conforme esperado para o Azure AD, em seguida, pode ser devido a vários motivos. Se receber um e-mail de erro do Azure AD ou vir o erro no Azure AD Connect Health, em seguida, leia [resolver problemas de erros de exportação](tshoot-connect-sync-errors.md) em vez disso. Mas se estiver a resolver um problema em que o objeto não está no Azure AD, em seguida, este tópico é para si. Ele descreve como localizar erros na sincronização de Azure AD Connect de componentes no local.

>[!IMPORTANT]
>Para o Azure Active Directory (AAD) Connect implementação com a versão 1.1.749.0 ou superior, utilize o [tarefas de resolução de problemas](tshoot-connect-objectsync.md) no Assistente para resolver problemas de sincronização do objeto. 

## <a name="synchronization-process"></a>Processo de sincronização

Antes de investigar problemas de sincronização, vamos compreender os **do Azure AD Connect** o processo de sincronização:

  ![O processo de sincronização do Azure AD Connect](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminologia**

* **CS:** Espaço conector, uma tabela na base de dados.
* **MV:** Metaverso, uma tabela na base de dados.
* **AD:** Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Passos de sincronização**
O processo de sincronização envolve os seguintes passos:

1. **Importar do AD:** **Do Active Directory** trazidos para objetos **AD CS**.

2. **Importar do AAD:** **O Azure Active Directory** trazidos para objetos **AAD CS**.

3. **Sincronização:** **Regras de sincronização de entrada** e **regras de sincronização de saída** são executados na ordem de número de precedência de inferior para superior. Para ver as regras de sincronização, pode aceder à **Editor de regras de sincronização** das aplicações de ambiente de trabalho. O **regras de sincronização de entrada** traz dados a partir do CS para MV. O **regras de sincronização de saída** move dados de MV para CS.

4. **Exportar para o AD:** Depois de executar a sincronização, objetos são exportados a partir do AD CS **do Active Directory**.

5. **Exportar para o AAD:** Depois de executar a sincronização, objetos são exportados a partir do AAD CS **do Azure Active Directory**.

## <a name="troubleshooting"></a>Resolução de problemas

Para localizar os erros, vai examinar alguns locais diferentes na seguinte ordem:

1. O [registos de operações](#operations) para localizar erros identificados pelo motor de sincronização durante a importação e sincronização.
2. O [espaço conector](#connector-space-object-properties) para localizar objetos em falta e erros de sincronização.
3. O [metaverso](#metaverse-object-properties) para localizar problemas relacionados com dados.

Inicie [Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md) antes de iniciar estes passos.

## <a name="operations"></a>Operações
Guia de operações no Synchronization Service Manager é onde deve começar a sua solução de problemas. A guia de operações mostra os resultados das operações mais recentes.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/operations.png)  

A metade superior mostra todas as execuções por ordem cronológica. Por predefinição, as operações de registo mantém informações sobre os últimos sete dias, mas esta definição pode ser alterada com o [agendador](how-to-connect-sync-feature-scheduler.md). Deve procurar qualquer execução que não mostra um Estado de êxito. Pode alterar a ordenação ao clicar nos cabeçalhos.

O **estado** coluna são as informações mais importantes e mostra o problema mais grave para uma execução. Aqui está um resumo rápido dos Estados mais comuns na ordem de prioridade para investigar (onde * indicar várias cadeias de caracteres de erro possíveis).

| Estado | Comentário |
| --- | --- |
| stopped-* |Não foi possível concluir a execução. Por exemplo, se o sistema remoto está inativo e não pode ser contactado. |
| stopped-error-limit |Há mais de 5.000 erros. A execução automaticamente foi parada devido ao grande número de erros. |
| concluída -\*-erros |A execução foi concluída, mas existem erros (menos de 5.000), que devem ser investigados. |
| concluída -\*-avisos |A execução foi concluída, mas alguns dados não está no estado esperado. Se tiver erros, em seguida, esta mensagem é, normalmente, apenas um sintoma. Até que a corrigir esses erros, avisos não deve investigar. |
| exito |Não existem problemas. |

Quando seleciona uma linha, atualiza a parte inferior para mostrar os detalhes de que são executados. Mais à esquerda da parte inferior, terá de indicar uma lista **passo n. º**. Esta lista aparece apenas se tiver vários domínios na sua floresta em que cada domínio é representado por um passo. O nome de domínio pode ser encontrado no cabeçalho **partição**. Sob **estatísticas de sincronização**, pode encontrar mais informações sobre o número de alterações que foram processados. Pode clicar nas hiperligações para obter uma lista dos objetos alterados. Se tiver objetos com erros, esses erros apresentados no **erros de sincronização**.

### <a name="troubleshoot-errors-in-operations-tab"></a>Resolver erros no guia de operações
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Quando tiver erros, tanto o objeto erro e o próprio erro são ligações fornecem mais informações.

Comece clicando a cadeia de erro (**sincronização regra-erro-função-acionada** na imagem). É apresentada pela primeira vez com uma visão geral do objeto. Para ver o erro real, clique no botão **rastreio de pilha**. Este rastreio fornece informações de nível de depuração para o erro.

Pode com o botão direito no **informações da pilha de chamadas** caixa, escolha **Selecionar tudo**, e **cópia**. Em seguida, pode copiar a pilha e examinar o erro no seu editor favorito, como o bloco de notas.

* Se o erro é partir **SyncRulesEngine**, em seguida, as informações da pilha de chamada tem primeiro uma lista de todos os atributos no objeto. Desloque para baixo até ver o rumo **InnerException = >**.  
  ![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)  
  A linha após mostra o erro. Na imagem acima, o erro é de um Fabrikam de regra de sincronização personalizado criado.

Se o erro em si não lhe dá informações suficientes, em seguida, é hora de examinar os dados em si. Pode clicar na ligação com o identificador de objeto e continuar a resolução de problemas a [objeto importado do espaço conector](#cs-import).

## <a name="connector-space-object-properties"></a>Propriedades de objeto de espaço conector
Se não tiver quaisquer erros encontrados no [operações](#operations) separador, em seguida, a próxima etapa é acompanhar o objeto de espaço conector do Active Directory, para o metaverso e para o Azure AD. Esse caminho, deve encontrar onde está o problema.

### <a name="search-for-an-object-in-the-cs"></a>Pesquisar um objeto no CS

Na **Synchronization Service Manager**, clique em **conectores**, selecione o conector do Active Directory, e **procurar espaço conector**.

Na **âmbito**, selecione **RDN** (quando pretende procurar no atributo CN) ou **DN ou âncora** (quando pretender uma procura o atributo distinguishedName). Introduza um valor e clique em **pesquisa**.  
![Pesquisa de espaço conector](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Se não encontrar o objeto que está procurando, poderá ter sido filtrado com [filtragem baseada em domínio](how-to-connect-sync-configure-filtering.md#domain-based-filtering) ou [filtragem baseada em UO](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Leitura a [configurar a filtragem](how-to-connect-sync-configure-filtering.md) tópico para verificar se a filtragem é configurada como esperado.

Outra pesquisa útil consiste em Selecionar o conector do Azure AD, no **âmbito** selecionar **importação**e selecione o **Add** caixa de verificação. Esta pesquisa dá-lhe todos os objetos sincronizados no Azure AD que não pode ser associado um objeto no local.  
![Orphan de pesquisa de espaço conector](./media/tshoot-connect-object-not-syncing/cssearchorphan.png)  
Esses objetos foram criados por outro motor de sincronização ou de um motor de sincronização com uma configuração de filtragem diferente. Esta vista é uma lista de **orphan** objetos já não é geridos. Deve rever esta lista e considere remover esses objetos usando o [do Azure AD PowerShell](https://aka.ms/aadposh) cmdlets.

### <a name="cs-import"></a>Importação de CS
Quando abrir um objeto de cs, existem várias guias na parte superior. O **importar** separador mostra os dados preparados após uma importação.  
![Objecto CS](./media/tshoot-connect-object-not-syncing/csobject.png)    
O **valor antigo** mostra o que atualmente é armazenado no Connect e o **novo valor** o que foi recebido do sistema de origem e ainda não foi aplicado. Se existir um erro no objeto, as alterações não são processadas.

**Error**  
![Objecto CS](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  
O **erro de sincronização** separador só está visível se existe um problema com o objeto. Para obter mais informações, consulte [resolver problemas de erros de sincronização](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS linhagem
O separador de linhagem mostra como o objeto de espaço conector está relacionado ao objeto de metaverso. Pode ver o conector importados pela última vez uma alteração do sistema ligado e quais regras aplicadas para preencher dados no metaverso.  
![Linhagem CS](./media/tshoot-connect-object-not-syncing/cslineage.png)  
Na **ação** coluna, pode ver outro **entrada** regra de sincronização com a ação **aprovisionar**. Que indica que, desde que este objeto de espaço conector estiver presente, o objeto de metaverso permanece. Se a lista de regras de sincronização mostra em vez disso, uma regra de sincronização com a direção **saída** e **aprovisionar**, ele indica que este objeto é eliminado quando o objeto de metaverso é eliminado.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/cslineageout.png)  
Também pode ver na **PasswordSync** coluna que o espaço conector de entrada pode contribuir com alterações para a palavra-passe, uma vez que uma regra de sincronização tem o valor **verdadeiro**. Esta palavra-passe é enviada para o Azure AD através da regra de saída.

Na guia linhagem, pode obter para o metaverso ao clicar em [propriedades do objeto de Metaverso](#mv-attributes).

Na parte inferior de todas as guias são dois botões: **Pré-visualização** e **Log**.

### <a name="preview"></a>Pré-visualização
A página de pré-visualização é usada para sincronizar um único objeto. É útil se estiver a resolver algumas regras de sincronização personalizados e quiser ver o efeito de uma alteração num único objeto. Pode selecionar entre **Full sync** e **sincronização Delta**. Também pode selecionar entre **gerar pré-visualização**, que apenas mantém a alteração na memória, e **confirmar pré-visualização**, que atualizado metaverso e prepara a todas as alterações aos espaços de conector de destino.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/preview.png)  
Pode inspecionar o objeto e quais regras aplicadas para um fluxo de atributos específico.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Registar
A página de registo é usada para ver o estado da sincronização de palavra-passe e o histórico. Para obter mais informações, consulte [resolver problemas de sincronização de hash de palavra-passe](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Propriedades do objeto de Metaverso
Em geral é melhor iniciar a procura da origem de espaço conector do Active Directory. Mas também pode iniciar a pesquisa do metaverse.

### <a name="search-for-an-object-in-the-mv"></a>Procurar um objeto de MV
Na **Synchronization Service Manager**, clique em **pesquisa de Metaverso**. Crie uma consulta que sabe, encontra o usuário. Pode procurar atributos comuns, como accountName (sAMAccountName) e userPrincipalName. Para obter mais informações, consulte [pesquisa de Metaverso](how-to-connect-sync-service-manager-ui-mvsearch.md).
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

Na **resultados da pesquisa** janela, clique com o objeto.

Se não encontrou o objeto, em seguida, ele ainda não atingiu o metaverse. Continuar para procurar o objeto no **do Active Directory** [espaço conector](#connector-space-object-properties). Se encontrar o objeto no **do Active Directory** espaço conector, em seguida, pode haver um erro de sincronização que está a bloquear o objeto do metaverso a chegar, ou pode existir uma regra de sincronização de controlo de âmbito do filtro aplicado.

### <a name="object-not-found-in-the-mv"></a>Objekt nebyl nalezen v a MV
Se o objeto está no **do Active Directory** CS, no entanto não estão presentes na MV, em seguida, filtro de âmbito é aplicado. 

* Para examinar o filtro de âmbito vá para o menu de aplicativo de desktop e clique em **Editor de regras de sincronização**. Filtre regras aplicáveis o objeto ao ajustar o filtro abaixo.

  ![Pesquisa de regras de sincronização de entrada](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

* Ver cada regra na lista acima e verifique o **Scoping filtro**. Na abaixo do controlo de âmbito do filtro, se o **isCriticalSystemObject** valor é nulo ou FALSO ou está vazio, em seguida, ele está no âmbito.

  ![Pesquisa de regras de sincronização de entrada](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

* Vá para o [importação CS](#cs-import) lista e verifique qual filtro está a bloquear o objeto para mover para a MV de atributo. Distância ou as **espaço conector** da lista de atributos mostrará apenas os atributos de não-null e não vazio. Por exemplo, se **isCriticalSystemObject** não está visível na lista, em seguida, isso significa que o valor deste atributo é nulo ou estar vazio.

### <a name="object-not-found-in-the-aad-cs"></a>Objekt nebyl nalezen v o CS do AAD
Se o objeto não está presente no **espaço conector** dos **Azure Active Directory**. No entanto, estiver presente na MV, em seguida, examinar o filtro de Scoping do **saída** regras de correspondente **espaço conector** e verifique se o objeto é filtrado devido a [MV atributos](#mv-attributes) não cumprir os critérios.

* Para ver o filtro de âmbito de saída, selecione as regras aplicável para o objeto ao ajustar o filtro abaixo. Ver cada regra e examinar o correspondente [atributo de MV](#mv-attributes) valor.

  ![Pesquisa de regras de saída Synchroniztion](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>Atributos de MV
No separador atributos, pode ver os valores e qual conector contribuiu com ele.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Se um objeto não está a sincronizar, observe, em seguida, os seguintes atributos no metaverso:
- É o atributo **cloudFiltered** presente e definido como **verdadeiro**? Se é, então ele foi filtrado, de acordo com os passos em [atributo filtragem com base em](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- É o atributo **sourceAnchor** presente? Caso contrário, tem uma topologia de floresta de recursos da conta? Se um objeto é identificado como uma caixa de correio vinculada (o atributo **msExchRecipientTypeDetails** tem o valor 2), em seguida, o sourceAnchor é que tenham feito parte da floresta com uma conta do Active Directory ativada. Certifique-se de que a conta mestre foi importada e sincronizada corretamente. A conta principal tem de estar listada no [conectores](#mv-connectors) para o objeto.

### <a name="mv-connectors"></a>Conectores de MV
O separador conectores mostra todos os espaços de conectores que tenham uma representação do objeto.  
![Gestor do serviço de sincronização](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  
Deve ter um conector para:

- Cada floresta do Active Directory do usuário é representada no. Essa representação pode incluir foreignSecurityPrincipals e objetos de contato.
- Um conector no Azure AD.

Se perder o conector para o Azure AD, em seguida, leia [atributos de MV](#mv-attributes) para verificar os critérios para a ser aprovisionada para o Azure AD.

Este separador também permite que navegue para o [objeto do espaço conector](#connector-space-object-properties). Selecione uma linha e clique em **propriedades**.

## <a name="next-steps"></a>Passos Seguintes
- [Sincronização do Azure AD Connect](how-to-connect-sync-whatis.md).
- [O que é a identidade híbrida? ](whatis-hybrid-identity.md).
