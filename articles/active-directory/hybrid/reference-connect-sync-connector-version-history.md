---
title: Histórico de versões do conector | Documentos da Microsoft
description: Este tópico apresenta uma lista de todas as versões dos conectores para o Forefront Identity Manager (FIM) e o Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 95f2ffb1a51184f1194f87a4a5e9a54e682edf80
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312005"
---
# <a name="connector-version-release-history"></a>Histórico de Versões do Conector
Os conectores do Forefront Identity Manager (FIM) e o Microsoft Identity Manager (MIM) são atualizados com frequência.

> [!NOTE]
> Este tópico é no FIM e o MIM apenas. Estes conectores não são suportados para instalação no Azure AD Connect. Conectores de lançamento são pré-instalado no AADConnect quando atualizar para o especificado a compilação.


Este tópico lista todas as versões dos conectores que foram lançadas.

Links relacionados:

* [Transferir conectores mais recente](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Conector LDAP genérico](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap) documentação de referência
* [Conector do SQL genérico](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql) documentação de referência
* [Conector de serviços Web](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) documentação de referência
* [Conector do PowerShell](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-powershell) documentação de referência
* [Conector do Lotus Domino](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-domino) documentação de referência


## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Problemas fixos:
* Resolvido ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent(Message: A device attached to the system is not functioning)
* Nesta versão dos conectores terá de atualizar o redirecionamento de associação de 3.3.0.0-4.1.3.0 para 4.1.4.0 no miiserver.exe.config
* Serviços Web genérico:
    * Não foi possível guardar a resposta de um JSON válido resolvida na ferramenta de configuração
* SQL genérico:
    * A exportação gera sempre apenas consulta update para a operação de eliminação. Adicionado para gerar uma consulta delete
    * A consulta SQL que obtém objetos para a operação de importação de Delta, se "Delta estratégia" for 'Controlo de alterações' foi corrigida. Nessa implementação conhecido limitação: importação de Delta com o modo de 'Controlo de alterações' não controla alterações em atributos com múltiplos valores
    * Foi adicionada possibilidade para gerar uma consulta delete para o caso, quando é necessário eliminar o último valor do atributo com múltiplos valores e esta linha não contém quaisquer outros dados, exceto o valor que é necessário eliminar.
    * System.ArgumentException manipulação quando implementado parâmetros de saída por SP 
    * Consulta incorreta para efetuar a operação de exportação para o campo que tem o tipo varbinary (Max)
    * Problema com a variável de parameterList foi inicializado duas vezes (nas funções ExportAttributes e GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Problemas fixos:

* Lotus Notes:
  * Opção de filtragem certifiers personalizado
  * Importação da classe ImportOperations corrigir a definição das operações que podem ser executadas no modo de "Vistas" e que, no modo de 'Search'.
* LDAP genérico:
  * Diretório de OpenLDAP utiliza o DN como âncora em vez de entryUUI. Nova opção de conector GLDAP que permite modificar a âncora
* SQL genérico:
  * Exportação fixa no campo que tem o tipo varbinary (Max).
  * Ao adicionar dados binários a partir de uma origem de dados ao objeto CSEntry, DataTypeConversion a função falhou em zero bytes. Função DataTypeConversion fixa da classe CSEntryOperationBase.




### <a name="enhancements"></a>Aprimoramentos:

* SQL genérico:
  * A capacidade de configurar o modo de execução do procedimento com parâmetros nomeados armazenado ou não com o nome é adicionado numa janela de configuração do agente de gestão do SQL genérico na página 'Global Parameters'. Na página 'Global Parameters' aqui é a caixa de verificação com a etiqueta 'Use parâmetros nomeado para executar um procedimento armazenado' que é responsável por modo para o procedimento de execute armazenado com parâmetros nomeada ou não.
    * Atualmente, a capacidade de executar o procedimento armazenado com parâmetros nomeados funciona apenas para bases de dados IBM DB2 e MSSQL. Para bases de dados Oracle e o MySQL, essa abordagem não funciona: 
      * As sintaxes SQL do MySQL não suporta parâmetros nomeados em procedimentos armazenados.
      * O controlador ODBC para o Oracle não suporta parâmetros nomeados de parâmetros nomeados em procedimentos armazenados)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Problemas fixos:

* Serviços Web genérico:
  * Foi corrigido um problema impedir que um projeto SOAP que está a ser criado quando havia dois ou mais pontos finais.
* SQL genérico:
  * Na operação de importação do GSQL foi não a conversão de tempo corretamente, quando os salva em espaço conector. O formato de data e hora de padrão para o espaço conector do GSQL foi alterado de 'DD-MM-AAAA: ssZ' para 'aaaa-MM-AAAA: ssZ'.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Problemas fixos:

* Serviços Web genérico:
  * A ferramenta de como Wsconfig não tiver convertido corretamente a matriz de Json da "pedido de exemplo" para o método de serviço REST. Esta ação causou problemas com a serialização essa matriz Json para o pedido REST.
  * Ferramenta de configuração do Web Service Connector não suporta a utilização dos símbolos de espaço em nomes de atributo JSON 
    * Um padrão de substituição pode ser adicionado manualmente para o ficheiro de WSConfigTool.exe.config, por exemplo ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```
> [!NOTE]
> Chave de JSONSpaceNamePattern é necessária para exportação que irá receber o seguinte erro: mensagem: nome vazio não é legal. 

* Lotus Notes:
  * Quando a opção **permitir certifiers personalizados para unidades de organização/organizacional** está desativada, em seguida, o conector falha durante a exportação (atualização) depois do fluxo de exportação, todos os atributos são exportados para o Domino, mas no momento da exportação um KeyNotFoundException é retornado para sincronização. 
    * Isto acontece porque a operação de mudança de nome falhará quando tentar alterar o DN (nome de utilizador atributo), alterando um dos atributos abaixo:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - UO
      - altcommonname

  * Quando **permitir certifiers personalizados para unidades de organização/organizacional** opção estiver ativada, mas certifiers necessários são ainda vazios, em seguida, KeyNotFoundException ocorre.

### <a name="enhancements"></a>Aprimoramentos:

* SQL genérico:
  * **Cenário: reestruturado implementado:** "*" funcionalidade
  * **Descrição da solução:** alterado abordagem para [manipulação de atributos de referência com múltiplos valor](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).


### <a name="fixed-issues"></a>Problemas fixos:

* Serviços Web genérico:
  * Não é possível importar a configuração do servidor se o conector do serviço Web estiver presente
  * Conector de serviço Web não está a funcionar com vários serviços Web

* SQL genérico:
  * Não existem tipos de objeto são listados para o atributo de referência de valor único
  * Importação delta no registo de alterações quando o valor é removido da tabela de valor múltiplos o objeto de eliminações de estratégia
  * OverflowException no conector GSQL com DB2 nas / 400

Lotus:
  * Opção adicional para enable\disable pesquisar UOs antes de abrir a página de GlobalParameters

## <a name="114430"></a>1.1.443.0

Lançamento: Março de 2017

### <a name="enhancements"></a>Aprimoramentos

* SQL genérico:</br>
  **Os sintomas de cenário:** é uma limitação bem conhecida com o conector do SQL, onde podemos apenas permitir uma referência ao tipo de um objeto e exigem uma referência cruzada com membros. </br>
  **Descrição da solução:** no passo de processamento para referências foram "*" opção for escolhida, todas as combinações de tipos de objeto serão retornadas para o motor de sincronização.

>[!Important]
- Esta ação irá criar vários marcadores de posição
- É necessário para se certificar de que a nomenclatura é exclusiva entre tipos de objeto.


* LDAP genérico:</br>
 **Cenário:** quando apenas alguns contentores estiverem selecionadas na partição específica, em seguida, a pesquisa ainda será feita na partição inteira. Específicos serão filtrados pelo serviço de sincronização, mas não por MA que poderá provocar a degradação do desempenho. </br>

 **Descrição da solução:** código do conector alterado GLDAP para torná-lo go possível por meio de todos os contentores e pesquisar objetos em cada um deles, em vez de procurar na partição inteira.


* Lotus Domino:

  **Cenário:** suporte de eliminação de correio Domino para uma remoção de pessoa durante uma exportação. </br>
  **Solução:** suporte de eliminação de correio configuráveis para uma remoção de pessoa durante uma exportação.

### <a name="fixed-issues"></a>Problemas fixos:
* Serviços Web genérico:
 * Ao alterar o URL do serviço em Default como wsconfig SAP projetos por meio da ferramenta de configuração do serviço Web, em seguida, acontece o seguinte erro: não foi possível localizar uma parte do caminho

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* LDAP genérico:
 * Conector de GLDAP não vê todos os atributos no AD LDS
 * Quebras de assistente quando nenhum atributo UPN é detectado do esquema de diretório LDAP
 * Delta Imports falhar com erros de deteção não presentes durante a importação completa, quando o atributo "objectclass" não está selecionado
 * Uma página de configuração de "Configurar partições e hierarquias", não mostra todos os objetos que tipo é igual para a partição para novos servidores genérica  
LDAP MA. Eles mostraram apenas os objetos da partição de RootDSE.


* SQL genérico:
 * Correção para a marca d'água do SQL genérico do erro de atributo com múltiplos valores não importado de importação Delta
 * Ao exportar deleted\added valores de atributo com múltiplos valores, não são deleted\added na origem de dados.  


* Lotus Notes:
 * Um campo específico "FullName" é apresentado corretamente no metaverso no entanto, ao exportar para notas o valor do atributo é nulo ou está vazio.
 * Corrigir para erro de duplicação Certifier
 * Quando o objeto sem quaisquer dados é selecionado no conector Lotus Domino com outros objetos, em seguida, vamos receber o erro de deteção ao efetuar a importação completa.
 * Quando a importação Delta é está a ser executado no conector Lotus Domino, no final dessa execução, o Microsoft.IdentityManagement.MA.LotusDomino.Service.exe do serviço, por vezes, devolve um erro da aplicação.
 * Associação ao grupo global funciona bem e é mantida, exceto quando em execução a exportação para tentar remover um utilizador de associação mostra conclusão com êxito com uma atualização, mas o utilizador, na verdade, não ser removido da associação ao Lotus Notes.
 * Uma oportunidade de escolher o modo de exportação como "Item Append na parte inferior" foi adicionado na configuração de GUI do Lotus MA para acrescentar novos itens na parte inferior, durante a exportação para atributos com múltiplos valores.
 * Conector irá adicionar a lógica necessária para excluir o arquivo da pasta de email e ID de cofre.
 * Elimine a associação não funcionar para cruzada NAB membro.
 * Valores devem ser eliminados com êxito do atributo com múltiplos valor

## <a name="111170"></a>1.1.117.0
Data da versão: Março de 2016

**Novo conector**  
Lançamento de inicial do [conector do SQL genérico](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).

**Novos recursos:**

* Conector LDAP genérico:
  * Foi adicionado suporte para a importação delta com Isode.
* Conector de serviços da Web:
  * Atualizada a atividade de csEntryChangeResult e a atividade de setImportErrorCode para permitir que os erros ao nível do objeto a serem retornados para o motor de sincronização.
  * Atualizar os modelos de SAP6 e SAP6User para usar a nova funcionalidade de erro de nível de objeto.
* Conector do Lotus Domino:
  * Para exportar, terá um certifier por catálogo de endereços. Agora, pode utilizar a mesma palavra-passe para todos os certifiers para facilitar a gestão.

**Problemas fixos:**

* Conector LDAP genérico:
  * Para o IBM Tivoli DS, alguns atributos de referência não foram detetados corretamente.
  * Para o LDAP aberto durante uma importação delta, foram truncados no início e fim de cadeias de caracteres com espaços em branco.
  * Para Novell e NetIQ, uma exportação que mover um objeto entre UOs/contentores e ao mesmo tempo mudar o nome do objeto falhou.
* Conector de serviços da Web:
  * Se o serviço web tiver vários pontos finais para a mesma ligação, em seguida, o conector não corretamente detetar estes pontos finais.
* Conector do Lotus Domino:
  * Uma exportação do atributo fullName a uma base de correio não funcionava.
  * Uma exportação que tanto adicionados e removidos membro de um grupo exportado apenas os membros adicionados.
  * Se um documento de notas é inválido (o isValid de atributo definido como false), em seguida, a falha do conector.

## <a name="older-releases"></a>Versões mais antigas
Antes de Março de 2016, os conectores foram lançados como tópicos de suporte.

**LDAP genérico**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, Setembro de 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, Março de 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, Janeiro de 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, Setembro de 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, Março de 2014

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, Setembro de 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, Setembro de 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, Setembro de 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, Março de 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, Agosto de 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, Fevereiro de 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, Outubro de 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, Agosto de 2013

## <a name="troubleshooting"></a>Resolução de problemas 

> [!NOTE]
> Quando a atualização do Microsoft Identity Manager ou AADConnect com uso de qualquer um dos conectores de ECMA2. 

Tem de atualizar a definição do conector após a atualização para corresponder ou receberá o erro seguinte no início de registo de eventos de aplicação para comunicar o aviso ID 6947: "versão da assemblagem na configuração do conector AAD ("X.X.XXX. X") é anterior à versão real ("X.X.XXX. X") de"C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

Para atualizar a definição:
* Abra as propriedades para a instância do conector
* Clique na ligação / ligar para o separador
  * Introduza a palavra-passe da conta de conector
* Clique em cada um dos separadores de propriedade, por sua vez
  * Se este tipo de conector têm um separador de partições, com um botão de atualização, clique no botão de atualização nessa guia
* Depois de todos os separadores de propriedade foram acessados, clique no botão OK para guardar as alterações.


## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre o [do Azure AD Connect](how-to-connect-sync-whatis.md) configuração.

Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](whatis-hybrid-identity.md).
