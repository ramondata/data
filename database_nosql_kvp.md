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

  - Obtendo uma lista com o nome das `keys` do hstore:
    ```
    -- visualizar como um conjunto
    select distinct akeys(sentimentos)
    from meus_sentimentos_expostos

    --or

    -- visualizar uma lista no retorno
    select distinct skeys(sentimentos)
    from meus_sentimentos_expostos
    ```

  - Para obter os `valores` ao invés das chaves, basta substituir a função akeys,skeys por avals ou svals

  - Para fazer uma consulta que retorne apenas os registros que possuam determinada chave:
    ```
    select *
    from meus_sentimentos_expostos
    where sentimentos ? 'data'
    ```
    - Esse tipo de consulta é extremamente útil pelo fato de cada registro ter um esquema distinto, ou seja, sem esquema algum determinado.

  - Outra forma de fazer filtros com a coluna do tipo hstore:
  ```
  select * from meus_sentimentos_expostos
  where sentimentos @> '"sentimento"=>"alegria"'::hstore
  ``` 
- [x] Data type `json` e `jsonb`:

  - Formato de transferencia de dados mais usado atualmente.
  - Seu predecessor é o xml(possui schema) e o formato que vem pra substituir ou facilitar ainda mais a transferencia de dados e o yaml ou yml.
  - No momento de criar a tabela no postgresql, informe o tipo de dado como json para a tabela especificada.
    ```
    create table pedidos (
    num_pedido serial primary key,
    detalhes json not null
    )
    ```
  - Abaixo um simples insert:
    ```
    insert into pedidos (detalhes) values (
    '{
      "cliente":"chiquinha",
      "produto":"sanduiche presunto",
      "qtd":"3",
      "peso":"0.5kg"
    }'
    )
    ```

  - Vamos realizar uma simples consulta:
    ```
    -- a coluna detalhes é do tipo json
    select detalhes -> 'cliente' as cliente
    from pedidos limit 5
    ```

  - Para conseguirmos utilizar de fato um `distinct` ou `order by`, precisamos converter o tipo da coluna de `json` para `text`:
    ```
    -- o sinal ->> além de acessar o dado na coluna de tipo json, converte este dado para o tipo texto
    
    select distinct
    detalhes ->> 'cliente' as cliente
    from pedidos
    order by 1
    ```

  - Realizando uma filtragem com o tipo json na coluna selecionada:
    ```
    select * from pedidos
    where (detalhes ->> 'cliente'::text) = 'chiquinha'
    ```

  - Para visualizar melhor cada par chave-valor na consulta acima, podemos usar a função json_each(coluna_json), como abaixo:
    ```
    select json_each(detalhes)
    from pedidos
    where (detalhes ->> 'cliente'::text) = 'chiquinha'
    ```

  - Quando uma chave tem vários valores inseridos, podemos acessar cada um individualmente da forma abaixo:
    ```
    -- exemplo de json com varios itens em uma chave:
    {
    "itens" : [
        {"bike":"caloi", "qtd":"2"},
        {"carro":"ecosport", "qtd":"1"},
        {"carro":"honda city", "qtd":"1"}
    ]
    }
    ```

    ```
    select detalhes -> 'itens' ->> 0 -- o numero zero representa o primeiro elemento da lista dentro das chaves acima
    from pedidos
    ```

  - Verificando os tipos de dados:
    ```
    -- verificando a coluna
    select json_typeof(detalhes) as detalhes from pedidos

    -- verificando um chave do json
    select json_typeof(detalhes -> 'itens') as itens
    from pedidos
    ```



















    



