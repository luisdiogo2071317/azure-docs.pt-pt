O armazenamento é limitado pelo espaço em disco ou por um limite restritivo no *número máximo* de índices ou documentos, o que chegar primeiro.

| Recurso | Gratuito | Básica | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Contrato de Nível de Serviço (SLA) |N.º <sup>1</sup> |Sim |Sim |Sim |Sim |Sim |
| Armazenamento por partição |50 MB |2GB |25 GB |100 GB |200 GB |200 GB |
| Partições por serviço |N/A |1 |12 |12 |12 |3 <sup>2</sup> |
| Tamanho da partição |N/A |2GB |25 GB |100 GB |200 GB |200 GB |
| Réplicas |N/A |3 |12 |12 |12 |12 |
| Índices máximos |3 |5 <sup>3</sup>|50 |200 |200 |1000 por partição ou 3000 por serviço |
| Indexadores máximos |3 |5 <sup>3</sup>|50 |200 |200 |Sem suporte de indexador |
| Origens de dados máximas |3 |5 <sup>3</sup>|50 |200 |200 |Sem suporte de indexador |
| Documentos máximos <sup>3</sup> |10,000 |1 milhão |15 milhões por partição ou 180 milhões por serviço |60 milhões por partição ou 720 milhões por serviço |120 milhões por partição ou 1,4 biliões por serviço |1 milhão por índice ou 200 milhões por partição |

<sup>1</sup> funcionalidades de pré-visualização e o escalão gratuito não são fornecidos com os contratos de nível de serviço (SLAs). Para todos os escalões faturáveis, SLAs entram em vigor quando aprovisionar redundância suficiente para o seu serviço. Dois ou mais réplicas são necessárias para o SLA de consulta (leitura). Três ou mais réplicas são necessárias para consulta e indexação SLA (leitura / escrita). O número de partições não é uma consideração de SLA. 

<sup>2</sup> O S3 HD tem um limite restritivo de 3 partições, que é inferior ao limite de partição do S3. O limite de partição inferior é imposto porque a contagem de índice do S3 HD é significativamente mais elevada. Dado que existem limites de serviço para recursos informáticos (processamento e armazenamento) e conteúdo (índices e documentos), o limite de conteúdo é atingido primeiro.

>[!Important]
> **<sup>3</sup>**  a partir do enlace tardio 2017, serviços de pesquisa do Azure criados recentemente tenham sido aprovisionados utilizando a mais poderosas subjacente as configurações de hardware que permitem para alguns limites ser alteradas em determinados regiões (Sul do Brasil, Canadá Central, Índia central, EUA leste, EUA Centro-Norte, Europa do Norte, EUA Centro-Sul, Sudeste asiático, RU Sul, Europa Ocidental e EUA oeste):
>
>* Básico e padrão camada de serviços de pesquisa criados depois de 2017 de enlace tardio não tem quaisquer limites de contagens de documentos apenas os limites de armazenamento são impostos nestes serviços. 
>* Para os serviços de alta densidade de S3 criados após o enlace tardio 2017, o documento de milhões de 200 por partição foi removido, mas o documento de milhões de 1 por permanece de limite de índice.
>* Um limite de aumento de 15 índices, indexadores e origens de dados, os serviços básicos criados após o enlace tardio 2017 ter.
>
>Para saber mais sobre os limites se aplicam a um serviço existente, utilize o portal do Azure para ver informações de limite na página de descrição geral do seu serviço.