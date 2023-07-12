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
  - Utilize o hstore ao criar uma tabela nova, aplique o hstore como o tipo de dado referencia do campo
    ```
    create table meus_sentimentos_expostos (
    nome varchar,
    sobrenome varchar,
    idade integer,
    sentimentos hstore
    )
    ```

  - Realizando um insert manual basico para verificar:
    ```
    insert into meus_sentimentos_expostos
    values (
    'Mickey',
    'Mouse',
    120,
    '"data"=>"11/07/2023", "sentimento"=>"satisfacao"'
    )
    ```

  - Consultado esse dado:
    ```
    select
    nome,
    sobrenome,
    idade,
    sentimentos -> 'data' as datas
    from meus_sentimentos_expostos
    ```
    > Mickey | Mouse | 120 | 11/07/2023

  - Realizando uma atualizção em uma tupla da tabela:
    ```
    update meus_sentimentos_expostos
    set sentimentos =
    replace(sentimentos::text, '"sentimento"=>"satisfacao"'::text, '"sentimento"=>"aconchego"'::text)::hstore
    where sentimentos -> 'data' = '11/07/2023'
    ```
