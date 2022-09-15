REST: Resources + Verbos HTTP
=============================

**Representational State** Transfer

Estado representacional:
  Estado: dado que muda no tempo.
  Representacional: representar um dado?

sql
json
xml  
comma/tab separated values

Usuário: João Pedro   Data de Nascimento: 18/08/94   Altura: 1.73   Situação: ativo

### Diferentes Representações (formato)

```SQL
INSERT INTO usuarios (id, nome, data_nascimento, altura)
VALUES (25, 'João Pedro', '1994-08-18', 1.73);
```

```JSON
{
    'id': 25,
    'nome': 'João Pedro',
    'dataNascimento': '1994-08-18',
    'altura': 1.73
}
```

```XML
<usuario id='25'>
    <nome>João Pedro</nome>
    <data-nascimento>1994-08-18</data-nascimento>
    <altura>1.73</altura>
</usuario>
```

### Representação de um Recurso

Recurso é um conjunto representável de dados e com um caminho identificador.

Recurso é similar à ideia de "entidade" (endpoint aponta para um recurso).

http://dominio/api/usuario/logar (verbos não são declarados no endpoint)

Não é **objeto/método**. É como se fosse só o "objeto".

```java
class Usuario {
    void logar() {

    }
}
```

Excluir um usuário: (não-REST)

```plain
POST http://dominio/api/usuario?id=25&acao=excluir
```

Usuário é um RESOURCE.
Para transformar os dados, usamos os método HTTP `[POST, GET, PUT, PATCH, DELETE]`.

```plain
DELETE http://dominio/api/usuarios/25
                         /recurso/identificador
```

```plain
// ler um usuário específico
GET http://dominio/api/usuarios/25
// ler todos os usuários
GET http://dominio/api/usuarios
// ler um grupo filtrado de usuários, evitar, porque "ativos" não é um recurso, mas um estado
// GET http://dominio/api/usuarios/ativos
GET http://dominio/api/usuarios?situacao=ativo
// Existe a ideia de sub-recurso, ex: todos os posts do usuário 25
GET http://dominio/api/usuarios/25/posts
GET http://dominio/api/usuarios/25/posts/26f2af24
// possível, mas estranho do ponto de vista de análise
GET http://dominio/api/posts/26f2af24/usuario
// singular é uma relação única do subrecurso com o recurso raiz
GET http://dominio/api/usuarios/25/perfil
```

Adicionar um usuário. Do recurso que representa uma "coleção" de usuários, colocar mais um usuário.

```http
POST http://dominio/api/usuarios
Content-Type: application/json

{ 
    'nome': 'Márcio Torres',
    'dataNascimento': '1976-10-25'
}

// resposta:
201 CREATED
Location: http://dominio/api/usuarios/26
```

Ler o usuário Márcio

```http
GET http://dominio/api/usuarios/26 HTTP/1.1
Accept: application/xml

// resposta:
<usuario id="26">
    <nome>Márcio Torres</nome>
    <data-nascimento>1976-10-25</data-nascimento>
    <situacao>ativo</situacao>
</usuario>
```

```http
PUT http://dominio/api/usuarios/26
Content-Type: application/json

{ 
    'nome': 'Márcio Torres',
    'dataNascimento': '1986-10-25'
}
// RESPOSTA
204 NO CONTENT
```

```http
POST http://dominio/api/usuarios/26/depositos

{
    'valor': 432.23,
    'cartao': '12323434343433243'
}
// RESPOSTA: aceito em processamento (assíncrona).
202 ACCEPTED
Location: http://dominio/api/pedidos/2342
```