>[!NOTE]
>Para recursos que não está corrigidos, pode abrir um pedido de suporte para pedir o aumento das quotas. **Não** crie contas dos Serviços de Multimédia do Azure adicionais numa tentativa de obter limites superiores.

| Recurso | Limite Predefinido | 
| --- | --- | 
| Contas dos Serviços de Multimédia do Azure (AMS) numa subscrição individual | 25 (fixo) |
| Unidades Reservadas de Multimédia (RUs) por conta do AMS |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
| Trabalhos por conta do AMS | 50,000<sup>(2)</sup> |
| Tarefas em cadeia por trabalho | 30 (fixo) |
| Elementos por conta do AMS | 1 000 000|
| Elementos por tarefa | 50 |
| Elementos por trabalho | 100 |
| Localizadores exclusivos associados a um elemento ao mesmo tempo | 5<sup>(4)</sup> |
| Canais em direto por conta do AMS |5|
| Programas no estado de paragem por canal |50|
| Programas no estado parado por canal |3|
| Pontos finais de transmissão em fluxo no estado em execução por conta do AMS|2|
| Unidades de transmissão em fluxo por ponto final de transmissão em fluxo |10 |
| Contas de armazenamento | 1000<sup>(5)</sup> (fixo) |
| Políticas | 1,000,000<sup>(6)</sup> |
| Tamanho dos ficheiros| Em alguns cenários, não há um limite no tamanho máximo de ficheiro suportado para processamento nos Media Services. <sup>7</sup> |
  
<sup>1</sup> As RUs S3 não estão disponíveis na Índia Ocidental. Se alterar o tipo (por exemplo, a partir de S2 para S1), os limites de RU máximas são repostos.

<sup>2</sup> Este número inclui trabalhos em fila, concluídos, ativos e cancelados. Não inclui trabalhos eliminados. Pode utilizar **IJob.Delete** ou o pedido HTTP **DELETE** para eliminar os trabalhos antigos.

A partir de 1 de Abril de 2017, qualquer registo de tarefas na sua conta mais antiga do que 90 dias serão automaticamente eliminado, juntamente com os respetivos registos de tarefa associados, mesmo que o número total de registos é inferior a quota máxima. Se precisar de arquivar as informações de tarefas, pode utilizar o código descrito [aqui](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> ao efetuar um pedido de lista tarefa entidades, um máximo de 1.000 tarefas é devolvido por pedido. Se tiver de estar a par de todos os Trabalhos submetidos, pode utilizar “top/skip”, conforme descrito em [OData system query options (Opções de consultas de sistema OData)](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Os localizadores não foram concebidos para gerir o controlo de acesso por utilizador. Para conceder direitos de acesso diferentes a utilizadores individuais, utilize soluções de Gestão de Direitos Digitais (Digital Rights Management, DRM). Para obter mais informações, veja [esta](../articles/media-services/media-services-content-protection-overview.md) secção.

<sup>5</sup> As contas de armazenamento têm de estar na mesma subscrição do Azure.

<sup>6</sup> Existe um limite de um milhão de políticas para diferentes políticas do AMS (por exemplo, para a política Locator ou ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Se estiver sempre a utilizar os mesmos dias, permissões de acesso, etc., deve utilizar o mesmo ID de política. Para obter informações e um exemplo, consulte [esta](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) secção.

<sup>7</sup>se estiver a carregar conteúdo para um recurso de Media Services processá-la com uma dos processadores de suporte de dados no serviço do Azure (ou seja, codificadores como motores codificador de multimédia Standard e o fluxo de trabalho do suporte de dados codificador Premium ou análise como enfrentam Detector), em seguida, deve estar ciente das restrições nos tamanhos de ficheiro máximo suportados. 

O tamanho máximo suportado para um único blob está atualmente até 5 TB de armazenamento de Blobs do Azure. No entanto, os limites adicionais aplicam-se nos serviços de suporte de dados do Azure com base nos tamanhos VM que são utilizados pelo serviço. A tabela seguinte mostra os limites em cada de unidades reservadas de multimédia (S1, S2, S3.) Se o ficheiro de origem é maior do que os limites definidos na tabela, a tarefa de codificação irá falhar. Se a codificação de origens de resolução de 4K de longa duração, é necessário utilizar unidades reservadas de multimédia S3 para alcançar o desempenho necessário. Se tiver conteúdo de 4K, que é superior a 260 GB limite de unidades reservadas de multimédia S3, contacte-nos amshelp@microsoft.com para potenciais mitigações suportar o seu cenário.

| Tipo de Unidade Reservada de Multimédia | Tamanho máximo de entrada (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
