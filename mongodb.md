# MongoDB

Servidor
------

Subir servidor com dados em um determinado caminho (configuração padrão)

    mongod --data=/caminho/database


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

