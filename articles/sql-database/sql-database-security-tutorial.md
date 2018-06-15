---
title: Proteger a sua Base de Dados SQL do Azure | Microsoft Docs
description: Saiba mais sobre as técnicas e funcionalidades destinadas a proteger a sua Base de Dados SQL do Azure.
services: sql-database
author: DRediske
manager: craigg
ms.service: sql-database
ms.custom: mvc,security
ms.topic: tutorial
ms.date: 04/24/2018
ms.author: daredis
ms.openlocfilehash: 54ec3c1386d6ce2023106367a6af1915e754948f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32193466"
---
# <a name="secure-your-azure-sql-database"></a>Proteger a sua Base de Dados SQL do Azure

A Base de dados SQL protege os seus dados ao: 
- Limitar o acesso à base de dados através de regras de firewall 
- Utilizar mecanismos de autenticação que precisam da respetiva identidade
- Autorizar os dados através de permissões e associações baseadas em funções 
- Segurança ao Nível da Linha
- Máscara de dados dinâmica

A Base de Dados SQL também tem monitorização, auditoria e deteção de ameaças sofisticadas. 

Pode melhorar a proteção da sua base de dados contra utilizadores mal intencionados ou o acesso não autorizado com apenas alguns passos simples. Neste tutorial, ficará a saber como: 

> [!div class="checklist"]
> * Configurar regras de firewall ao nível do servidor para o seu servidor no portal do Azure
> * Configurar regras de firewall ao nível da base de dados para a sua base de dados com o SSMS
> * Ligar à sua base de dados através de uma cadeia de ligação segura
> * Gerir o acesso de utilizador
> * Proteger os seus dados com encriptação
> * Ativar a auditoria da Base de Dados SQL
> * Ativar a deteção de ameaças da Base de Dados SQL

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, certifique-se de que tem o seguinte:

- Instalou a versão mais recente do [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Instalou o Microsoft Excel
- Criou um servidor e uma base de dados SQL do Azure. Consulte [Criar uma base de dados SQL do Azure no portal do Azure](sql-database-get-started-portal.md), [Criar uma base de dados SQL do Azure única com a CLI do Azure](sql-database-get-started-cli.md) e [Criar uma base de dados SQL do Azure única com o PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Criar uma regra de firewall ao nível do servidor no portal do Azure

As bases de dados SQL estão protegidas por uma firewall no Azure. Por predefinição, todas as ligações ao servidor e às bases de dados contidas no servidor são rejeitadas, exceto quando se trata de ligações de outros serviços do Azure. Para obter mais informações, veja [Regras de firewall ao nível do servidor da Base de Dados SQL e ao nível da base de dados](sql-database-firewall-configure.md).

A configuração mais segura consiste em definir a opção "Permitir acesso a serviços do Azure" como DESATIVADA. Se precisar de ligar à base de dados a partir de uma VM do Azure ou de um serviço cloud, deve criar um [IP Reservado](../virtual-network/virtual-networks-reserved-public-ip.md) e permitir o acesso do endereço IP reservado apenas através da firewall. 

Siga estes passos para criar uma [regra de firewall ao nível do servidor da Base de Dados SQL](sql-database-firewall-configure.md) para o seu servidor de modo a permitir ligações a partir de um endereço IP específico. 

> [!NOTE]
> Se tiver criado uma base de dados de exemplo no Azure com a ajuda de um dos tutoriais ou guias de início rápido anteriores e estiver a realizar este tutorial num computador com o mesmo endereço IP que tinha quando percorreu esses tutoriais, pode ignorar este passo porque já terá criado uma regra de firewall ao nível do servidor.
>

1. Clique em **Bases de dados SQL** no menu do lado esquerdo e clique na base de dados para a qual quer configurar a regra de firewall na página **Bases de dados SQL**. É aberta a página de descrição geral da base de dados, que mostra o nome de servidor completamente qualificado (como **mynewserver-20170313.database.windows.net**) e oferece opções para configuração adicional.

      ![regra de firewall do servidor](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Clique em **Configurar firewall do servidor** na barra de ferramentas, conforme mostrado na imagem anterior. É aberta a página **Definições da firewall** do servidor da Base de Dados SQL. 

3. Clique em **Adicionar IP do cliente** na barra de ferramentas para adicionar o endereço IP público do computador ligado ao portal ou introduza manualmente a regra de firewall e, em seguida, clique em **Guardar**.

      ![configurar regra de firewall do servidor](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Clique em **OK** e, em seguida, clique no **X** para fechar a página **Definições da firewall**.

Agora, já pode ligar a qualquer base de dados do servidor com o endereço IP ou o intervalo de endereços IP especificado.

> [!NOTE]
> A Base de Dados SQL comunica através da porta 1433. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 1433 poderá não ser permitido pela firewall da rede. Se assim for, não poderá ligar ao seu servidor de Base de Dados SQL do Azure, a menos que o departamento de TI abra a porta 1433.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Criar uma regra de firewall ao nível da base de dados com o SSMS

As regras de firewall ao nível da base de dados permitem-lhe criar definições de firewall diferente para bases de dados diferentes no mesmo servidor lógico, bem como criar regras de firewall portáteis, ou seja, regras que acompanham a base de dados durante uma [ativação pós-falha](sql-database-geo-replication-overview.md) em vez de serem armazenadas no servidor SQL. As regras de firewall ao nível da base de dados só podem ser configuradas através de instruções Transact-SQL e só depois de ter configurado a primeira regra de firewall ao nível do servidor. Para obter mais informações, veja [Regras de firewall ao nível do servidor da Base de Dados SQL e ao nível da base de dados](sql-database-firewall-configure.md).

Siga estes passos para criar uma regra de firewall específica da base de dados.

1. Estabeleça ligação à sua base de dados, por exemplo, através do [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. No Object Explorer, clique com o botão direito do rato na base de dados para a qual quer adicionar uma regra de firewall e clique em **Nova Consulta**. É aberta uma janela de consulta em branco que está ligada à sua base de dados.

3. Na janela de consulta, modifique o endereço IP para o seu endereço IP público e, em seguida, execute a seguinte consulta:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Na barra de ferramentas, clique em **Executar** para criar a regra de firewall.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Ver como ligar uma aplicação à sua base de dados através de uma cadeia de ligação segura

Para garantir uma ligação encriptada segura entre uma aplicação cliente e a Base de Dados SQL, a cadeia de ligação tem de ser configurada de modo a:

- Pedir uma ligação encriptada e
- Não confiar no certificado de servidor. 

Isto estabelece uma ligação com TLS (Transport Layer Security) e reduz o risco de ataques do tipo man-in-the-middle. Pode obter cadeias de ligação configuradas de forma correta para a sua Base de Dados SQL para controladores de cliente suportados a partir do portal do Azure, conforme mostrado para o ADO.net nesta captura de ecrã. Para obter informações sobre a TLS e a conectividade, veja [Considerações sobre a TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

1. Selecione **Bases de dados SQL** no menu do lado esquerdo e clique na sua base de dados na página **Bases de dados SQL**.

2. Na página **Descrição Geral** da sua base de dados, clique em **Mostrar cadeias de ligação de bases de dados**.

3. Reveja a cadeia de ligação **ADO.NET** completa.

    ![Cadeia de ligação de ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Criar utilizadores de base de dados

Antes de criar os utilizadores, primeiro tem de escolher um dos dois tipos de autenticação suportados pela Base de Dados SQL Azure: 

**Autenticação de SQL**, que utiliza o nome de utilizador e a palavra-passe para inícios de sessão e utilizadores válidos apenas no contexto de uma base de dados específica no âmbito de um servidor lógico. 

**Autenticação do Azure Active Directory**, que utiliza identidades geridas pelo Azure Active Directory. 

Se quiser utilizar o [Azure Active Directory](./sql-database-aad-authentication.md) para se autenticar na Base de Dados SQL, tem de existir um Azure Active Directory povoado antes de poder continuar.

Siga estes passos para criar um utilizador que utiliza a Autenticação de SQL:

1. Estabeleça ligação à sua base de dados, por exemplo, através do [SQL Server Management Studio](./sql-database-connect-query-ssms.md), com as suas credenciais de administrador de servidor.

2. No Object Explorer, clique com o botão direito do rato na base de dados à qual quer adicionar um novo utilizador e clique em **Nova Consulta**. É aberta uma janela de consulta em branco que está ligada à base de dados selecionada.

3. Na janela da consulta, introduza a seguinte consulta:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Na barra de ferramentas, clique em **Executar** para criar o utilizador.

5. Por predefinição, o utilizador pode ligar à base de dados, mas não tem permissões para ler ou escrever dados. Para conceder estas permissões ao utilizador recém-criado, execute os dois comandos seguintes numa nova janela de consulta.

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

É considerada uma melhor prática criar estas contas sem ser de administrador ao nível da base de dados para ligar à sua base de dados, a menos que precise de executar tarefas de administrador, como criar novos utilizadores. Reveja o [Tutorial do Azure Active Directory](./sql-database-aad-authentication-configure.md) sobre como efetuar a autenticação com o Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Proteger os seus dados com encriptação

A encriptação de dados transparente (TDE) da Base de Dados SQL do Azure encripta automaticamente os dados inativos, sem que seja preciso fazer quaisquer alterações à aplicação que acede à base de dados encriptada. Para as bases de dados recém-criadas, a TDE está ativada por predefinição. Para ativar a TDE para a sua base de dados ou para verificar se a TDE está ativada, siga estes passos:

1. Selecione **Bases de dados SQL** no menu do lado esquerdo e clique na sua base de dados na página **Bases de dados SQL**. 

2. Clique em **Encriptação de dados transparente** para abrir a página de configuração da TDE.

    ![Encriptação de Dados Transparente](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Se necessário, defina **Encriptação de dados** como ATIVADA e clique em **Guardar**.

O processo de encriptação é iniciado em segundo plano. Pode monitorizar o progresso ao estabelecer ligação à Base de Dados SQL com o [SQL Server Management Studio](./sql-database-connect-query-ssms.md) e consultar a coluna encryption_state da vista [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql?view=sql-server-2017). O estado 3 indica que a base de dados está encriptada. 

## <a name="enable-sql-database-auditing-if-necessary"></a>Ativar a auditoria da Base de Dados SQL, se necessário

A Auditoria de Base de Dados SQL do Azure controla os eventos da base de dados e escreve-os num registo de auditoria na sua conta de Armazenamento do Azure. A auditoria pode ajudá-lo a manter a conformidade regulamentar, compreender a atividade da base de dados e obter informações detalhadas sobre discrepâncias e anomalias que possam indiciar potenciais violações de segurança. Siga estes passos para criar uma política de auditoria predefinida para a sua base de dados SQL:

1. Selecione **Bases de dados SQL** no menu do lado esquerdo e clique na sua base de dados na página **Bases de dados SQL**. 

2. No painel Definições, selecione **Auditoria e Deteção de Ameaças**. Tenha em atenção que a auditoria ao nível do servidor está desativada e que existe uma ligação **Ver definições do servidor** que lhe permite ver ou modificar as definições de auditoria de servidor neste contexto.

    ![Painel de Auditoria](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Se preferir ativar um tipo de Auditoria (ou uma localização?) diferente do especificado ao nível do servidor, selecione a opção **ATIVADA** em Auditoria e escolha o Tipo de Auditoria **Blob**. Se a auditoria de Blobs do servidor estiver ativada, a auditoria configurada da base de dados coexistirá com a auditoria de Blobs do servidor.

    ![Ativar a auditoria](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Selecione **Detalhes de Armazenamento** para abrir o painel Armazenamento de Registos de Auditoria. Selecione a conta de armazenamento do Azure onde os registos serão guardados e o período de retenção, após o qual os registos antigos serão eliminados, e, em seguida, clique em **OK** na parte inferior. 

   > [!TIP]
   > Utilize a mesma conta de armazenamento para todas as bases de dados auditadas para tirar o máximo partido dos modelos de relatórios de auditoria.
   > 

5. Clique em **Guardar**.

> [!IMPORTANT]
> Se pretender personalizar os eventos auditados, pode fazê-lo através do PowerShell ou da API REST. Consulte [Auditoria da base de dados SQL](sql-database-auditing.md) para obter mais informações.
>

## <a name="enable-sql-database-threat-detection"></a>Ativar a deteção de ameaças da Base de Dados SQL

A Deteção de Ameaças oferece uma nova camada de segurança, o que permite aos clientes detetar e responder a potenciais ameaças à medida que estas ocorrem, ao fornecer alertas de segurança relativamente a atividades anómalas. Os utilizadores podem explorar os eventos suspeitos com a Auditoria de Base de Dados SQL para determinar se estes resultam de uma tentativa de acesso, violação ou exploração dos dados na base de dados. A Deteção de Ameaças simplifica a resolução de potenciais ameaças à base de dados sem que seja necessário ser um especialista em segurança ou gerir sistemas de monitorização de segurança avançados.
Por exemplo, a Deteção de Ameaças deteta determinadas atividades anómalas da base de dados que indiciem potenciais tentativas de injeção de SQL. A injeção de SQL é um dos problemas comuns de segurança das aplicações Web na Internet, sendo utilizada para atacar aplicações condicionadas por dados. Os atacantes tiram partido das vulnerabilidades das aplicações para injetar instruções SQL maliciosas nos campos de entrada das aplicações, com o objetivo de violar ou modificar os dados contidos na base de dados.

1. Navegue para o painel de configuração da base de dados SQL que pretende monitorizar. No painel Definições, selecione **Auditoria e Deteção de Ameaças**.

    ![Painel de navegação](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. No painel de configuração **Auditoria e Deteção de Ameaças**, selecione a opção **ATIVADA** em Auditoria, o que apresentará as definições de deteção de ameaças.

3. Selecione a opção **ATIVADA** em Deteção de Ameaças.

4. Configure a lista de emails que irão receber alertas de segurança após a deteção de atividades anómalas da base de dados.

5. Clique em **Guardar** no painel **Auditoria e Deteção de Ameaças** para guardar a auditoria nova ou atualizada e a política de deteção de ameaças.

    ![Painel de navegação](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Se forem detetadas atividades anómalas da base de dados, receberá uma notificação por e-mail após a deteção das mesmas. A mensagem de e-mail fornecerá informações sobre o evento de segurança suspeito, incluindo a natureza das atividades anómalas, o nome da base de dados, o nome do servidor e a hora do evento. Além disso, fornecerá informações sobre as causas possíveis e as ações recomendadas para investigar e atenuar a potencial ameaça à base de dados. Os passos seguintes explicam o que deve fazer se receber uma mensagem de e-mail desse género:

    ![Mensagem de e-mail sobre deteção de ameaças](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Na mensagem de e-mail, clique na ligação **Registo de Auditoria do SQL Azure**, o que irá iniciar o portal do Azure e mostrar os registos de auditoria relevantes ocorridos por volta da hora do evento suspeito.

    ![Registos de auditoria](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Clique nos registos de auditoria para ver mais informações sobre as atividades suspeitas da base de dados, como a instrução SQL, a razão da falha e o IP de cliente.

    ![Detalhes do registo](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. No painel Registos de Auditoria, clique em **Abrir no Excel** de modo a abrir um modelo do Excel pré-configurado para importar e executar uma análise mais aprofundada do registo de auditoria por volta da hora do evento suspeito.

    > [!NOTE]
    > O Excel 2010 ou posterior precisa do Power Query e da definição **Combinação Rápida**.

    ![Abrir registos no Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Para configurar a definição **Combinação Rápida**: no separador **POWER QUERY** do friso, selecione **Opções** para apresentar a caixa de diálogo Opções. Selecione a secção Privacidade e escolha a segunda opção, "Ignorar os Níveis de Privacidade e melhorar potencialmente o desempenho":

    ![Combinação rápida de Excel](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Para carregar os registos de auditoria de SQL, certifique-se de que os parâmetros no separador de definições estão especificados corretamente e, em seguida, selecione o friso "Dados" e clique no botão "Atualizar Tudo".

    ![Parâmetros do Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Os resultados são apresentados na folha **Registos de Auditoria de SQL**, o que lhe permite executar uma análise mais aprofundada das atividades anómalas detetadas e atenuar o impacto do evento de segurança na aplicação.


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu a melhorar a proteção da sua base de dados contra utilizadores mal intencionados ou o acesso não autorizado com apenas alguns passos simples.  Aprendeu a: 

> [!div class="checklist"]
> * Configurar regras de firewall para o servidor e/ou a base de dados
> * Ligar à sua base de dados através de uma cadeia de ligação segura
> * Gerir o acesso de utilizador
> * Proteger os seus dados com encriptação
> * Ativar a auditoria da Base de Dados SQL
> * Ativar a deteção de ameaças da Base de Dados SQL

Avance para o próximo tutorial para saber como implementar uma base de dados distribuída geograficamente.

> [!div class="nextstepaction"]
>[Implementar uma base de dados distribuída geograficamente](sql-database-implement-geo-distributed-database.md)

