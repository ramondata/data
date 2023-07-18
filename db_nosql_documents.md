### noSQL orientado a documentos

  - Cada registro é um organizado em chave : valor
  - Cada registro no database é um documento 
  - Documentos são organizados em coleções de documentos que possuem algo em comum, como uma transição de venda.
  - schemaless

  > tabelas são como coleções
  > registros são como documentos

- Documento é uma unidade, uma linha de tabela ou também comumente chamada de tupla ao invés de linha/registro
- Os documentos podem ser simples ou compostos. Compostos como listas ou documentos dentro de documentos

### Formatos de arquivos permitidos no modelo nosql orientado a documentos:

- xml (extensible markup language)
- json (java script object notation)
- jsonB (json performatico do postgresql)
- bson (binary json)
- yaml/yml (yet another markup language)
- formatos de textos simples (csv, txt)

- json é o mais usado.

Exemplo de documento XML:

```
<?xml version="1.0">
<!doctype acervo>
<acervo>
  <data_record>
  <idtitulo>1</idtitulo>
    <acervo_detalhes>{
      "a" : "a",
      "b" : "b",
      "c" : [1,2,3],
      "d" : [10, "d", 20, 25]}
    </aceervo_detalhes>
  </data_record>
</acervo>
```

Exemplo de documento json:

```
{
  "a" : "a",
  "b" : "b",
  "c" : {[1,2,3], [10,10,10], "cc"},
  "d" : [10, "d", 20, 25]
}
```

Comandos usuais de usuários com sql basico:

`CRUD`: create, read, update, delete


Usaremos o mongodb:

- banco orientado a documentos, nosql, projetado para sistema de processamento distribuído.
- Respeita o modelo CAP e não ACID

comandos mql:


- [x] visualize os dbs disponiveis
```
show dbs
```
  
- [x] crie o seu próprio database no mongodb
```
use my_own_database
```

- [x] Crie sua coleção de documentos, semelhante a uma tabela no modelo relacional
```
db.createCollection("nome_da_colecao", {capped: true, size:5000, max: 100})

-- capped: Liga a limitação da coleção
-- size: tamanho em bytes
-- max: máximo de documentos nessa coleção
```


- [x] Visualize suas coleções dentro do database atual
```
show collections
```


- [x] Inserindo um documento na sua coleção
```
db.nome_da_colecao.insert(
  {
    nome : 'Ronaldo',
    idade : '70',
    profissao : 'null',
    estado : 'ES',
    estado_civil : 'casado'
  }
)
```

- [x] Realizando a leitura/ consulta do documento criado
```
db.nome_da_colecao.find()
```

