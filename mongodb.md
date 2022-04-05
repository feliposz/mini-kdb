---
title: MongoDB
---

Referências
-----------

Instalador: <http://www.mongodb.org/downloads>
Tutorial: <http://mongly.openmymind.net/tutorial/index>
E-book: <http://openmymind.net/2011/3/28/The-Little-MongoDB-Book>


Servidor
------

Subir servidor com dados em um determinado caminho (configuração padrão)

    mongod --data=/caminho/database

Executar servidor com configuração específica e sem autenticação

    mongod.exe --noauth -f D:\Portable\Programas\mongodb\mongodb.config

Exemplo de config:

    systemLog:
        destination: file
        path: "D:/Portable/Programas/mongodb/log/mongod.log"
        logAppend: true
    storage:
        dbPath: "D:/Portable/Programas/mongodb/data"
        journal:
            enabled: true
    setParameter:
        enableLocalhostAuthBypass: false

Cliente
------

Conectar num cliente mongodb local

    mongo

Ajuda

    help

Exibe bancos de dados disponíveis

    show dbs

Seleciona banco de dados (cria um se não existir)

    use demo

Exibe coleções (tabelas)

    show collections

Comandos "CRUD"
------

Nota: Ao inserir o primeiro objeto será criada a coleção se já não houver

Inserir um objeto

    db.dogs.insert({"name" : "Rusty", "breed" : "Mutt" })

Inserir múltiplos objetos

    db.dogs.insert([
        {"name" : "Tater", "breed" : "Mutt", "isCute" : true }, 
        {"name" : "Lucy", "breed" : "Mutt" }, 
        {"breed" : "Labradoodle", "name" : "Lulu" }, 
        {name: "Snoopy", breed: "Beagle", isCartoon: true}
    ])

Listar todos os objetos

    db.dogs.find()

Listar objeto filtrando

    db.dogs.find({name: "Rusty"})

Atualizar objeto inteiro (removendo campos não atualizados)

    db.dogs.update({name: "Lulu"}, {breed: "Labradoodle"})

Atualizar objeto parcialmente (mantém campos existentes)

    db.dogs.update({name: "Rusty"}, {$set: {name: "Tater", isCute: true}})

Remove objeto específico

    db.dogs.remove({breed: "Mutt"})

Remove TODOS os objetos

    db.dogs.remove({})

Aplicando ordenação, limites, etc

    db.dogs.find().sort({name: -1}).skip(1).limit(2)

Obter o nome do bando de dados em uso

    db.getName()

Obter o nome das coleções (tabelas)

    db.getCollectionNames();

Conta quantos documentos há em uma coleção

    db.COLECAO.count();

Lista todos os objetos de uma coleção

    db.COLECAO.find();

Filtra os objetos

    db.COLECAO.find({campoTexto: 'valor'});

Múltiplas condições, usando comparadores < >=

    db.COLECAO.find({campoNumerico: {$lt: 100}, campoNumerico: {$gte: 200}});

O segundo argumento de find() são os campos a serem buscados

    db.COLECAO.find({}, {campo1: 1, campo2: 1});

Ordenação

    db.COLECAO.find().sort({campoEmordemAscendente: 1, campoEmOrdemDescendente: -1});

Limitar a quantidade

    db.COLECAO.find().limit(N);

Ignorar os primeiros N registros

    db.COLECAO.find().skip(N);

Uma consulta combinando filtro, seleção de campos, ordenação, limitar a X documentos e pular Y linhas

    db.COLECAO.find({campoFiltrado: 'valor'}, {campoSelecionado: 1})
            .sort({campoOrdenacao:-1})
            .limit(X)
            .skip(Y);

Conta documentos usando filtro -> semelhante a db.COLECAO.find({filtro...}).count();

    db.COLECAO.count({campoFiltro: {$lt: valorMaximo});

Inserindo documentos em uma coleção (não é obrigatório que tenha os mesmos campos

    db.COLECAO.insert({campo1: valor1, campo2: valor2, campo3: valor3});
    db.COLECAO.insert({campo1: valor1, campo2: valor2);

Selecionar objetos conforme os campos que possuem

    db.COLECAO.find({campo3: {$exists: true}});

Substitui um registro inteiro (neste caso, busca exatamente o documento por seu id)

    db.COLECAO.update({_id: ObjectId('????????????????????????')}, {campo1: outroValor1, campo2: outroValor2});

Atualiza somente um ou mais campos específicos (sem alterar os demais)

    db.COLECAO.update({_id: ObjectId('????????????????????????')}, {$set: {campo1: valor1}});

Incrementa o valor de um campo numérico de UM objeto (o primeiro localizado)

    db.COLECAO.update({campoFiltro: 'filtro'}, {$inc: {contador: 1}});

Atualiza o objeto se existir, ou insere um novo valor (3o. parâmetro: upsert = true)

    db.COLECAO.update({campoFiltro: 'filtro'}, {$inc: {contador: 1}}, true);

Acrescenta um valor em um campo que é uma lista

    db.COLECAO.update({filtro: 'valor'}, {$push: {lista: 'novoElemento'}}, false, true);

Atualiza todos os objetos filtrados (4o. parâmetro = true)

    db.COLECAO.update({filtro: 'valor'}, {$set: {campo: 'novoValor'}}, false, true);


Mongoose
------

Instalar no projeto

    npm install mongoose --save

Importar e conectar

    var mongoose = require('mongoose');
    mongoose.connect('mongodb://localhost/NOMEBD');

Definir schema

    var dogSchema = new mongoose.Schema({
        name: String,
        age: Number,
        breed: String
    });

Modelo

    var Dog = mongoose.model("Dog", dogSchema);

Criar novo objeto e salvar

    var myDog = new Dog({name: "Rusty", age: 11, breed: "Mutt" });
    myDog.save(function (err, dog) {
        if (err) {
            console.error(err);
            return;
        }
        console.log(dog);
    });

Criar e salvar numa ação

    Dog.create({
        "name" : "Rusty", 
        "age": 11, 
        "breed" : "Mutt"
    }, function (err, dog) {
        // fazer algo
    });

