---
title: "Iniciando com Go: MongoDB com mgo"
date: 2014-07-31 23:46:07
description: "Já pensou em usar MongoDB em seus projetos Go?"
---

Hoje vou falar como podemos usar o MongoDB, famoso banco de dados não-relacional, em nossas aplicações Go. Para isso vamos usar o pacote mgo, do brasileiro [Gustavo Niemeyer](http://niemeyer.net/). O mgo é um pacote que tem varias funcionalidades legais, como se pode ver no seu site [aqui](http://labix.org/mgo).

Bom, o primeiro passo é baixar o pacode mgo via go get, o qual podemos fazer assim:

```bash
go get gopkg.in/mgo.v2
```

Depois de baixado, podemos criar nossa aplicação, que vai poder inserir e buscar dados no mongo. Vamos chamar esse arquivo de `book.go`. Com o arquivo criado, vamos começar a programar.

```go
package main

import (
  "fmt"
  "gopkg.in/mgo.v2"
  "gopkg.in/mgo.v2/bson"
)
```

Nessa primeira parte, eu criei o package main e fiz o importe de 3 pacotes: `fmt`, que possui varias funções legais para formatação, `mgo`, que o pacote que se comunica com o mongo e o `mgo/bson`, uma implementação da especificação BSON (binary json) para Go.

Aqui eu defino uma struct `Book`, que vai conter os atributos da minha collection.

```go
type Book struct {
  Title string
  Pages string
}
```

Agora vamos a função principal:

```go
func main() {
  session, err := mgo.Dial("localhost:27017")
  if err != nil {
    panic(err)
  }
  defer session.Close()
  [...]
```

Aqui eu defino o endereço e a porta que o mgo vai se conectar com o MongoDB. No meu caso, localhost:27017. Eu recebo 2 variaves de retorno, `session`, para quando se conectar com sucesso e `err`. Temos um if, para quando err for diferente de nil, disparar um `panic` para que `defer session.Close()` seja executado e feche a conexão com o mongo.
Continuando na `func main`, vamos agora selecionar a collection que vamos trabalhar e inserir alguns dados nela:

```go
c := session.DB("test").C("book")
err = c.Insert(Book{"GoT", "600"}, Book{"Divergent", "456"})
if err != nil {
  panic(err)
}
```

Atribuimos a `c` a sessão do mongo com a collection `book` do banco `test`. Em seguida inserimos 2 livros na collection `book` e se houver erro guardamos em `err` e disparamos o panic para fechar a conexão. Se `err == nil`, vida que segue :D

Vamos finalmente fazer a busca:

```go
result := Book{}
err = c.Find(bson.M{"title": "GoT"}).One(&result)
if err != nil {
  panic(err)
}

fmt.Println("Pages:", result.Pages)
```

Bom pessoal, era mais ou menos isso que tinha pra mostrar pra vocês!
Abraços.
