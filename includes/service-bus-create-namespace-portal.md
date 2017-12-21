Para começar a utilizar as entidades de mensagens do Service Bus no Azure, tem de, primeiro, criar um espaço de nomes que seja exclusivo em todo o Azure. Um espaço de nomes fornece um contentor de âmbito para abordar os recursos do Service Bus na sua aplicação.

Para criar um espaço de nomes:

1. Inicie sessão no [Portal do Azure][Azure portal].
2. No painel de navegação esquerdo do portal, clique em **+ criar um recurso**, em seguida, clique em **integração empresarial com**e, em seguida, clique em **Service Bus**.
3. Na caixa de diálogo **Criar espaço de nomes**, introduza um nome de espaço de nomes. O sistema verifica imediatamente a disponibilidade do nome.
4. Após se certificar de que o espaço de nomes está disponível, selecione o escalão de preço (Básico, Standard ou Premium).
5. No campo **Subscrição**, selecione a subscrição do Azure em que pretende criar o espaço de nomes.
6. No campo **Grupo de recursos**, selecione um grupo de recursos existente em que o espaço de nomes será colocado ou crie um novo.      
7. Em **Localização**, selecione o país ou a região em que o espaço de nomes deverá ser alojado.
   
    ![Create namespace][create-namespace]
8. Clique em **Criar**. O sistema cria o espaço de nomes e ativa-o. Poderá ter de aguardar alguns minutos enquanto o sistema aprovisiona recursos para a sua conta.

### <a name="obtain-the-management-credentials"></a>Obter as credenciais de gestão
Criar um novo espaço de nomes automaticamente gera uma regra de assinatura de acesso partilhado (SAS) inicial com um par de chaves primárias e secundárias que cada conceder controlo total sobre todos os aspetos do espaço de nomes associado. Consulte [barramento de serviço de autenticação e autorização](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) para obter informações sobre como criar regras mais com mais restrita direitos para os remetentes regulares e os recetores. Para copiar a regra inicial, siga estes passos: 

1.  Clique em **todos os recursos**, em seguida, clique no nome do espaço de nomes criado recentemente.
2. Na janela do espaço de nomes, clique em **políticas de acesso partilhado**.
3. No **políticas de acesso partilhado** ecrã, clique em **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. No **política: RootManageSharedAccessKey** janela, clique em que a cópia junto à **chave de cadeia – primária da ligação**, para copiar a cadeia de ligação para a sua área de transferência para utilização posterior. Cole este valor no Bloco de Notas ou noutra localização temporária.
   
    ![connection-string][connection-string]

5. Repita o passo anterior, copie e cole o valor da **Chave primária** para uma localização temporária para utilizar mais tarde.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
