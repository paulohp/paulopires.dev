---
title: "Node Streams"
date: 2014-08-20 00:00:07
description: "Veja como streams são simples e melhoram bastante seu codigo"
---

Desde que comecei a trabalhar com Node, a uns 2 anos, buscava entender uma coisa: Stream. Até um certo momento, não era uma coisa útil pra mim, mas depois de conhecer essa abstração, acho que é impossível programar no Node.js sem conhece-la bem. Falei que tinha uma abstração, mas na realidade são duas. Streams de leitura (ReadStream), e de escrita (WriteStream). Eles são implementados ao longo de muitos objetos Node e representam o fluxo de entrada e saída de dados. Se você trabalha com Node.js, já deve ter trombado com stream por ai.

##ReadStream
Vamos imaginar que ReadStream seria uma torneira de dados. E depois que voce criar uma, voce pode:

**Esperar por dado**

Ao conecta-lo com o evento de "data" você sera notificado toda vez que um "pedaço" (formalmente conhecido como `chunk`) for entregue pela nossa stream. Essas entregas podem ser feitas como buffer ou string.

```js
var streamDeLeitura = ...
streamDeLeitura.on('data', function(data) {
//Aqui data é um buffer
});

var streamDeLeitura = ...
streamDeLeitura.setEncoding('utf8')
streamDeLeitura.on('data', function(data) {
//Aqui data é uma string
});
```

**Saber quando acaba**

Você nunca sabe quando a agua da torneira vai acabar, mas as streams sim, e você pode saber quando isso ocorre escutando o evento `end`, tipo assim:

```js
var streamDeLeitura = ...
streamDeLeitura.on('end', function() {
console.log("A stream e a agua da torneira acabaram :P");
});
```

**Pausar e resumir**

Você pode pausar uma stream tão facilmente quanto fechar uma torneira.

```js
readStream.pause()
```

Para resumir, é tão simples quanto isso:

```js
readStream.resume()
```

##WriteStream
Uma stream de escrita, formalmente conhecida como `WriteStream`, é conhecida por enviar ou escrever dados. Podendo ser uma conexão com a rede ou um arquivo. Com uma WriteStream podemos:

**Escreve nela**

Você pode escrever um buffer ou uma string chamando `write`:

```js
var streamDeEscrita = ...
streamDeEscrita.write('isso é uma string com encoding UTF8');
```

Nesse ponto, o Node assume que nos passamos uma string com encoding UTF-8. Mas você pode especificar um outro encoding, assim:

```js
var streamDeEscrita = ...
streamDeEscrita.write('T2xhciwgdGFpcj8gdG9yIQ==', 'base64');
```

##Alguns exemplos de Streams
Vamos ver um pouco delas no mundo real.

**Streams do sistema de arquivos**

Você pode criar uma stream de leitura de um arquivo fazendo assim:

```js
var fs = requeire("fs")
var rs = fs.createReadStream("arquivo.txt", { encoding: "utf8" })
```

Aqui lemos um arquivo com encoding UTF8 através de uma stream.
E para gravar nele, é bem simples:

```js
var fs = require("fs")
var ws = fs.createWriteStream("arquivo.txt", { encoding: "utf8" })
```

Bom e assim termino esse post basico e já deu pra ver um pouco do poder das Streams. Você pode saber mais por aqui: http://nodejs.org/api/stream.html. Em breve voltarei e falarei de back pressure e pipe.
