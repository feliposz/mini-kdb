---
title: Javascript
---

# Funções 


    alert(msg) // mostra mensagem
    valor = prompt(msg) // recebe um valor
    resultado = confirm(msg) // exibe mensagem e retorna true se usuário confirmar ou false se cancelar
    console.log(msg) // grava mensagens no console


# Elementos da página:

    var obj = document.getElementById("id");

    text.value: obtem/altera valor de um input box de texto.

    label.textContent: obtem/altera o valor de um label (span)

    eval(): converte valor texto para numérico



# Tipos de dados

    numeros: 0, -5, 1.234
    strings: 'abc', "def", ""
    booleans: true, false, a > b, 1 === 2

Use a função typeof(x) para determinar o tipo dos dados. O resultado é uma string:

    typeof("x") => "string"
    typeof(123) => "number"
    typeof(true) => "boolean"
    typeof(Math) => "object"
    typeof(Math.sqrt) => "function"

# Type coercion

    var s = "1";
    var n = +s; // = 1

    var n = 1;
    var s = ""+n; // = "1"

    var d = new Date; // Date object
    var t = +new Date; // Integer timestamp in milliseconds

    var n = 255;
    n.toString(2); // Number to binary
    n.toString(8); // Number to octal
    n.toString(16); // Number to hexa
    n.toString(36); // Number to base-36 (0-9a-z)

    parseInt('10011011', 2) // Binary to integer
    parseInt('777', 8) // Octal to integer
    parseInt('DEADBEEF', 16) // Hexa to integer
    parseInt('FELIPO', 36) // base-36 to integer


# ASCII Character code

    var s = "abc";
    var ascii = s.charCodeAt(0); // => 97 ("a")
    var letter = String.fromCharCode(ascii); // => "a"


# Operadores

    operadores aritméticos: + - * / %
    operadores lógicos: && || !
    operadores de comparação: > < === != >= <=
    operadores de precedência: ( )


# Estruturas de controle

    if (condicao) { verdadeiro; } else { falso; }

    for (inicio; condicao; incremento) { bloco; }

    while (condicao) { bloco; }
    
    do { bloco; } while (condicao);



# Manipulando strings:

    "batman".substring(2, 4) === "tm"

    "string".length: tamanho de uma string




# Variáveis locais vs. globais:


    var nome_var = valor; (Cria variável local da função ou global se criada fora de função.)

    nome_var = valor; (Cria variável sempre global se ela não existir no contexto atual. Evite usar este tipo de declaração de varíavel.)



# Funções

    function nome(p1, p2) {
    ...
    return x;
    }

    // ou

    var nome = function(p1, p2) {
    ...
    return x;
    };



# Arrays

    var a = [0, 1, 2, 3, 4];
    a.length // Tamanho do array.
    keys(a) // Chaves em uso.
    novo_array = a.map(fn); // Executa fn(x) para cada elemento x, retornando novo_array com os valores resultantes.
    novo_array = a.filter(fn); // Executa fn(x) para cada elemento, retornando apenas aqueles que retornarem "true" no teste.
    result = a.reduce(fn); // Executa fn(a, b) para cada 2 elementos do array retornando a redução total no fim. Útil para somatórios, produtos, concatenação, etc.
    a.forEach(fn); // Executa fn(x) para cada elemento do array.

    Outros método: reduceRight, insert, push, pop, reverse, slice, indexOf, lastIndexOf, join


# Matemática

    // Usar o objeto Math. Exemplo: Math.sqrt(2)

    abs, acos, asin, atan, atan2, ceil, cos, exp, floor, log, max, min, pow, random, round, sin, sqrt, tan, toString

    Constantes: PI, E, LN10, LN2, LOG10E, SQRT2


# Números

    var n = 1;

    n.toFixed(2); Converte em string com duas casas decimais.



    // Objetos:

    var nome_objeto = {

        atributo1: valor1,

        atributo2: valor2,

        metodo: function(p1, p2) {
            this.atributo1 = p1 / 100;
            this.atributo2 = p2 * 10;
            return p1 + p2;
        }
    };

    nome_objeto.atributo1 = nome_objeto.atributo2 * 2;
    nome_objeto["atributo1"] = nome_objeto["atributo2"] * 2;
    x = nome_objeto.metodo(1, 2);
    x = nome_objeto["metodo"](1, 2);
    x = nome_objeto["metodo"].call(nome_objeto, 1, 2);

    var outro_objeto = new Object();
    outro_objeto.atributo = valor;
    outro_objeto.metodo = function(x) {
        return x * 2;
    };


    // Construtor - Método 1
    // Desvantagens: Criação dos objetos é mais lenta e usa mais memória, pois cada objeto tem uma cópia das funções.
    // Vantagens: Mais simples e chamada dos métodos é mais rápida. Permite acesso a variável privada.
    // Indicado para objetos únicos ou com poucas instâncias.

    function MeuObjeto(x, y) {
        var _n = "Variável privada: " + x + ", " + y;
        this.x = x;
        this.y = y;    
        
        this.move = function (dx, dy) {
            this.x += dx;
            this.y += dy;
        };
        
        this.print = function() {
            console.log(this.x, this.y, _n);
        };
    }

    // Construtor - Método 2
    // Vantagens: Criação dos objetos é mais rápida e usa menos memória (as funções são compartilhadas)
    // Desvantagens: Mais complexo e a chamada dos métodos tem um pequeno overhead. Não permite acesso a variável privada.
    // Indicado para objetos com muitas instâncias.

    var obj = new MeuObjeto(10, 20);

    function MeuObjeto(x, y) {
        var _n = "Variável privada não acessível aos métodos do prototype."
        this.x = x;
        this.y = y;
    }
        
    MeuObjeto.prototype.move = function (dx, dy) {
        this.x += dx;
        this.y += dy;
    };

    MeuObjeto.prototype.print = function() {
        console.log(this.x, this.y);
    };


    var obj = new MeuObjeto(10, 20);
