## Bancos de dados de chave : valor


### Principais características:
- [x] alta performance
- [x] Velocidade em consultas
- [x] Simplicidade nas queries
- [x] facilidade de particionamento
- [x] Escalonamento horizontal(computação distribuida)  
- [x] Dados de alta volumetria: big data
- [x] Realizar streaming de dados (processamento em tempo real)
- [x] Segurança anti-tragédias com a replicação em clusters (servers)
- [x] schemaless - sem esquema pré-definido sendo cada conjunto de dados com as `colunas` ou `chaves` sem padrão definido

### Lista de exemplos de software de databases com esse perfil de nosql
- [x] postgresql
- [x] redis
- [x] amazon dynamoDB
- [x] Azure cosmosDB

### Postgresql

- [x] Data type `HSTORE`:

  - Tipo de dado encontrado no postgresql que simula uma estrutura `json` para o modelo kvp (i.e.: par chave valor)
  - [x] Ativar o hstore:
  ```
  create extension hstore
  ```
