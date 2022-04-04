---
title: Node.js
---


Aplicação node simples
------

Criando um projeto básico:

    mkdir app
    cd app
    npm init

Alguns módulos úteis:

    npm install express --save
    npm install grunt --save
    npm install gulp --save-dev


Configurar NPM para trabalhar com Proxy no Windows
------

No arquivo C:\Users\USUARIO\.npmrc incluir estas linhas:

    strict-ssl=false
    registry=http://registry.npmjs.org/
    proxy=http://usuario:senha@servidor.proxy:8080
    https-proxy=http://usuario:senha@servidor.proxy:8080

Referência => <http://stackoverflow.com/questions/7559648/is-there-a-way-to-make-npm-install-the-command-to-work-behind-proxy>


Argumentos de linha de comando
------

    argc -> process.argv.length
    argv -> process.argv[0], process.argv[1]...


Instalação do módulo para acesso ao Oracle DB
------

Configurar variáveis de ambiente:

    set OCI_LIB_DIR=C:\Oracle\product\11.2.0\client_1\oci\lib\msvc
    set OCI_INC_DIR=C:\Oracle\product\11.2.0\client_1\oci\include

Apontar para o caminho do Python 2.7:

    set PATH=C:\Programas\python-2.7.9;%PATH%

Preparar variáveis de ambiente do compilador C++:

    "C:\Program Files\Microsoft Visual Studio 14.0\VC\bin\vcvars32.bat"

Acessar o diretório do projeto:

    cd C:\Dev\js\node-ora

Instalar o módulo:

    npm install oracledb



Gerador de aplicação express
------

Gerador de "scaffold" para aplicações express.

Instalar:

    npm install -g express-generator

Gerar aplicação na pasta .\exemplo_app e instala dependências:

    express exemplo_app
    cd exemplo_app
    npm install

Executar aplicação em modo de depuração:

    SET DEBUG=exemplo_app:*
    npm start

Para gerar aplicação usando view engine EJS (o padrão é JADE):

    express -e exemplo_ejs


Configurar rotas separadas por módulos
------

Na aplicação (app.js) principal:

    var exemplo = require("./routers/exemplo.js");

    app.use("/exemplo", exemplo);


Em /routers/exemplo.js:

    var express = require("express");
    var router = express.Router();

    router.get("/", function (req, res) {
        res.send("exemplo módulo roteador");
    });

    module.exports = router;

Configurar e rodar instância do MongoDB Server
------

Diretório de instalação do exemplo: C:\mongodb

Arquivo de configuração (C:\mongodb\mongodb.cfg):

    systemLog:
        verbosity: 1
        destination: file
        path: C:\mongodb\data\log\mongod.log
        logAppend: true
    storage:
        dbPath: C:\mongodb\data\db
        engine: mmapv1
        journal:
            enabled: true
    net:
        bindIp: 127.0.0.1
        port: 27017
    setParameter:
        enableLocalhostAuthBypass: true

Executando a instância:

    C:\mongodb\MongoDB\Server\3.2\bin\mongod.exe --noauth --config C:\mongodb\mongodb.cfg
  
Cliente:

    C:\mongodb\MongoDB\Server\3.2\bin\mongo

Express
------

Um exemplo simples de "Hello World":

    var express = require('express');
    var app = express();

    app.get('/', function (req, res) {
      res.send('hello world');
    });

    app.post('/hello', function (req, res) {
      var result = {};
      result.hello = 'world';
      res.send(result);
    });

    app.listen(3000);

Servir arquivos estáticos:

    var path = require('path');
    app.use(express.static(path.join(__dirname, 'public')));

Configurar uma view engine:

    // Indica caminho onde estão as views
    app.set('views', path.join(__dirname, 'views'));

    // Escolher a view engine a ser utilizada (requer instalação)
    app.set('view engine', 'jade');
    // ou
    app.set('view engine', 'ejs');


Usando MORGAN para logar mensagens no console de desenvolvimento:

    // npm install morgan --save
    var logger = require('morgan');
    app.use(logger('dev'));

Usando BODY-PARSER para tratar os campos de formulário na requisição (x-www-form-urlencoded):

    // npm install body-parser --save
    var parser = require('body-parser');
    app.use(parser.urlencoded({extended: false}));

    app.post('/exemplo', function (req, res) {
        res.send(req.body.exemplo);
    });

Parâmetros e query string:

    // /exemplo/123 => 123
    app.get('/exemplo/:id', function (req, res) {
        res.send(req.params.id);
    });

    // /busca?termo=teste&pagina=3 => Termo: teste Pagina: 3

    app.get('/busca', function (req, res) {
        res.send('Termo: ' + req.query.termo + ' Pagina: ' + req.query.pagina);
    });

Definindo rotas/handlers
------

Sintaxe:

    app.VERB(path, [callback...], callback);

Exemplos:

    // Respondendo uma solicitação GET simples
    app.get('/', function (req, res) {
      res.send('hello');
    });

    // Respondendo uma solicitação POST com erro
    app.post('/something_wrong', function (req, res) {
      res.status(500);
      res.render('error');
    });

    // Criando um handler que manipula request/response
    app.use(function (req, res, next) {
      req._something = 'empty';
      res._something = 'handler';
      next();
    });

    // Criando um handler que utiliza o request/response manipulado
    app.use(function (req, res, next) {
      console.log('This > ' + req._something + ' -> ' + res._something);
      next();
    });

    // Devolve uma view renderizada
    app.get('/foo', function (req, res) {
      res.render('foo', {title: 'Bar'});
    });

Requisição
------

    app.METHOD(PATH, function (req, res) {

        req.params.XXX // Parâmetros identificados em path
        req.query.XXX  // Valores passados na querystring ex: ?a=1&b=2&c=3
        req.body.XXX   // Valores do formulário (necessita do middleware body-parser)

    });


Resposta
------

    app.METHOD(PATH, function (req, res) {

        res.send('abc');                     // Responde com conteúdo simples
        res.sendFile('caminho/arquivo.txt'); // Responde com conteúdo do arquivo
        res.json(obj);                       // Devolve um objeto JSON
        res.render('nomeview', obj);         // Renderiza view conforme view engine utilizada

    });


JADE
------

    h1= title
    p Hello from #{title}

EJS
---

Variável simples:

    <h1><%= title %></h1>
    <p>Hello from <%= title %></p>

Incluir parciais:

    <% include ../partials/footer %>

Loop:
    <% items.forEach(function(item) { %>
        <li><%= item.name %> - <%= item.value %></li>
    <% }); %>


GULP
------

Instalando (desenvolvimento):

    npm i --save-dev gulp

Exemplo mínimo:

    var gulp = require('gulp');

    gulp.task('hello', function () {
      console.log('hello');
      return;
    });

    gulp.task('default', ['hello']);

Executando:

    gulp                // executa as tarefas padrão
    -ou-
    gulp hello          // executa uma tarefa específica

Rodando lint (verifica boas práticas) nos arquivos .js:

    npm i --save-dev jshint gulp-jshint

    var gulp = require('gulp');
    var jshint = require('gulp-jshint');

    gulp.task('lint', function () {
      return gulp.src('js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
    });

    gulp.task('default', ['lint']);


## Errors

On windows, when starting a project with `npm run start:dev` or similar you get an error message saying 
'NODE_ENV' is not recognized.

Your `package.json` says something like:

```
"scripts": {
    "start:dev": "NODE_ENV=development nodemon"
}
```

To fix, install `win-node-env` :

```
npm install -g win-node-env
npm run start:dev
```

