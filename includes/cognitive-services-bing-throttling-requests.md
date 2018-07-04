O serviço e o seu tipo de subscrição determinar o número de consultas que pode realizar por segundo (QPS). Deve garantir que a sua aplicação inclui a lógica necessária para se manter na sua quota. Se exceder o seu QPS, o pedido falha com o código de estado HTTP 429. A resposta também inclui o cabeçalho Tentar-Após, que contém o número de segundos que deve aguardar antes de enviar outro pedido.  
  
### <a name="denial-of-service-dos-versus-throttling"></a>Denial of Service (DOS) e Limitação

O serviço diferencia entre um ataque DOS e violação de QPS. Se o serviço suspeitar de um ataque denial of service, o pedido é bem-sucedido (o código de estado HTTP é 200 OK); no entanto, o corpo da resposta está vazio.