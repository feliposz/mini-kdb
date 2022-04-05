---
title: Python
---


Operadores
==========

```python
+ - * ** / // %
== != <> < > >= <=
in, not in
and, or, not
is, is not

'a' + 'b' == 'ab'
'a' * 3 = 'aaa'

** # potenciação
// # divisão inteira (python 3)
/  # divisão decimal (python 3)

= += -= *= /= //= %= **=
```

Formatação de string
====================

```python
'aspas simples quando precisar usar "aspas duplas" na string'

"aspas duplas quando precisar usar 'aspas simples' na string"

"""
aspas triplas com "
funcionam em várias linhas
"""

'''
aspas triplas com ' também funcionam
mas é recomendável evitar
'''

"Um número inteiro %d" % num_int
"Um número inteiro %d e uma string '%s' !" % (num_int, texto)
"Formatação de um float %.2f " % num_float
"Elemento nome: %(nome) endereço: %(endereco)" % {'nome': 'Fulano', 'endereco': 'Rua ABC, 123'}

'%d'  # inteiros
'%f'  # float
'%r'  # igual a repr(...)
'%s'  # igual a str(...)
'%c'  # igual a chr(...)
'%%'  # % literal

'\r'  # volta ao início da linha
'\n'  # próxima linha
'\b'  # backspace
'\t'  # tab
'\\'  # \ literal
```

Python 2 x 3
============

```python
# Print é uma função

py2: print "hello"
py3: print("hello")

py2: print "hello",
py3: print("hello", end="")

# Range é "iterável"

py2: range(10)
py3: list(range(10))

py2: xrange(10)
py3: range(10)

# / não trunca o resultado, retorna um valor "float". Use // para divisão inteira

py2: 2 / 3 == 0 ou 2.0 // 3.0 = 0.0
py3: 2 // 3 == 0

py2: 2.0 / 3.0 == 0.66666666666666663
py3: 2 / 3 == 0.66666666666666663

# Em python 3, input() sempre retorna uma string. É necessário forçar a conversão explicitamente.
# Evite usar input() em python 2. Use raw_input()

py2: texto = raw_input('texto:')
py3: texto = input('texto':)

py2: numero_inteiro = input('inteiro:')
py3: numero_inteiro = int(input('inteiro:'))

py2: numero_decimal = input('decimal:')
py3: numero_decimal = float(input('decimal:'))
```


Listas e dicionários
====================

```python
lista = [1, 2, 3]
lista[0]
lista[:1]
lista[-1]
clone_lista = lista[:]

lista.sort() # ordenação "in place"
sorted(lista) # cria uma nova lista ordenada

#métodos: append, insert, pop, remove, reverse, sort, count

dicionario = {'abc': 123, 'def': 456}
dicionario['abc'] = 789

dicionario.keys() # ['abc', 'def']
dicionario.values() # [789, 456]
```

Parâmetros de linha
===================

```python
from sys import argv

# separar em variáveis
script, first, second, third = argv
```

Lendo e gravando arquivos
=========================

```python
# Abrir o arquivo (modo texto, para leitura)
arq = open('nome.arquivo')

# Ler todo o conteúdo do arquivo como string
conteudo = arq.read()

# Ler uma linha
linha = arq.readline()

# Fechar o arquivo
arq.close()

# Abrir para gravação
arq = open('nome.arquivo', 'w')

# Escreve no arquivo
arq.write('abc123\n')
arq.write('def456\n')

# Esvaziar um arquivo
arq.truncate()

# Verificar se um arquivo existe
from os.path import exists
arq_existe = exists('abc.txt')

# Abrir arquivo UTF-8 com BOM (Byte Order Mark) opcional
arq_utf8_bom = open('arquivo_utf8_bom.csv', encoding='utf-8-sig')
```

Função
======

```python
def nome_funcao(arg1, arg2, arg3):
    return 'alguma coisa'

def funcao_argumentos_variaveis(*args):
    for i, v in enumerate(args):
        print('Argumento %d: %r' % (i, v))
```

Estruturas de controle
======================

```python
if cond1:
    cmd1
elif cond2:
    cmd2
else
    cmd3

for elem in lista:
    print(elem)

for k, v in dicionario:
    print('%s => %s' % (k, v))

for i in range(5): # 0, 1, 2, 3, 4
    print(i)

for i in range(5, 10): # 5, 6, 7, 8, 9
    print(i)

for i in range(10, 30, 5): # 10, 15, 20, 25
    print(i)

while cond1:
    cmd1
    cmd2
    cmd3

# Loop infinito
while True:
    cmd1
    cmd2

# Loop com teste no final do/while
while True:
    cmd1
    cmd2
    if cond:
        break

# continue - pula a execução do restante do bloco no loop atual e passa para o próximo
while cond1:
    cmd1
    if cond2:
        continue
    cmd2
```

Exemplo de script com main
===========================

```python
def fn1():
    pass

def fn2():
    pass

if __name__ == '__main__':
    fn1()
    fn2()
```

Algumas palavras reservadas
===========================

Keyword   | Descrição
----------|------------------------
`and`     | operador lógico 'e'
`as`      | usando com as expressões "except NomeExcecao as ex" e "with ... as x"
`assert`  | verifica uma condição para o programa e gera uma exceção se for falso
`break`   | sai do laço while/for atual
`class`   | define uma nova classe
`continue`| pula a execução do restante do bloco no laço atual e passa para o próximo
`def`     | define uma função
`del`     | remove uma variável, um elemento de uma lista/array ou uma entrada em um dicionário/hash
`elif`    | encadeia condições num comando if
`else`    | se a condição de if, e todas as outras condições de elif forem falsa, executa o bloco else
`except`  | usado com try. captura uma exceção
`exec`    | interpreta uma string como uma linha de comando python
`finally` | usado com try. sempre executado, mesmo que ocorra uma exceção
`for`     | permite fazer um loop em uma lista, dicionário ou qualquer objeto 'iterável'
`from`    | permite importar funções/classes a partir de um módulo mas colocando os nomes importados no namespace local
`global`  | informa o interpretador que uma varíavel é global, caso contrário, ao ser atribuída será criada uma variável local por padrão
`if`      | desvio condicional. usado com if/elif/else
`import`  | importar um módulo para uso. as funções importadas ficam em seus namespaces separados
`in`      | testa se um valor está contido em uma lista/tupla ou se uma chave existe em um dicionário. também usado em for/in
`is`      | testa se duas variáveis apontam para o mesmo objeto em memória (obs: para testar se um objeto é uma instância de uma classe, usar a função "isinstance(obj, classe)")
`lambda`  | permite criar uma função anônima
`not`     | operador lógico de negação 'não'
`or`      | operaodor lógico 'ou'
`pass`    | não faz nada. usado onde é obrigatório um comando, mas não é desejável nenhuma ação (por exemplo, ao ignorar uma exceção propositalmente, ou ao criar um 'stub' de uma função ou classe)
`print`   | exibe uma mensagem/valor no console. é um comando em python 2 e uma função em python 3
`raise`   | gera uma exceção
`return`  | retorna de uma função. o valor é opcional
`try`     | início de um bloco de tratamento de exceções try/except/finally
`while`   | executa um laço de repetição enquanto uma condição for verdadeira. pode ser usado com um loop infinito (while True) e com um teste de condição para interrupção no meio/final do laço (if cond: break)
`with`    | instancia um objeto para se usado num contexto e destrói o objeto no final do bloco (para tratamento de arquivos, conexão de banco de dados, etc.)
`yield`   | retorna um valor para um objeto iterável

Tipos
=====

Tipo      | Descrição
----------|------------------------
`None`    | representa um valor vazio
`boolean` | True/False
`strings` | armazena textos
`numbers` | números inteiros
`floats`  | números decimais
`lists`   | listas de dados/array
`dicts`   | dicionário em que os valores são acessados por chave/hash/tabela associativa


Classe
======

```python
# Exemplo de definição de classe
class NomeClasse(ClasseBaseOpcional):

    # construtor
    def __init__(self):
        super(NomeClasse, self).__init__(self) # Chamar o construtor da classe base
        self.membro1 = 123
        self.membro2 = 'abc'
        self.membro3 = None

    # métodos
    def metodo1(self, param1, param2):
        self.membro1 = param1 + param2

    def membro2(self)
        return self.membro2

    # método não implementado
    def stub(self):
        pass
        
    
    # --------- métodos especiais ---------
    
    # objeto funciona como um array/dicionário, ex: nome_obj[key]
    __dict = {}
    
    def __getitem__(self, key):
        return __dict[key]

    def __setitem__(self, key, value):
        __dict[key] = value
        
    def __detitem__(self, key):
        del __dict[key]


exemplo = NomeClasse()
exemplo.metodo1(2, 3)
print(exemplo.metodo2())

# classe vazia
class Vazia:
    pass

v = Vazia()
v.oi = 'abc'
print(v.oi) # abc
print(v.__dict__) # {'oi': 'abc'}
```

Criando um projeto
==================

Ref: <http://learnpythonthehardway.org/book/ex46.html>

Pacotes uteis:

- [pip](http://pypi.python.org/pypi/pip)
- [distribute](http://pypi.python.org/pypi/distribute)
- [nose](http://pypi.python.org/pypi/nose)
- [virtualenv](http://pypi.python.org/pypi/virtualenv)

Estrutura:

    skeleton/
         NAME/
             __init__.py
         bin/
         docs/
         setup.py
         tests/
             NAME_tests.py
             __init__.py

Exibir SQL formatado
====================

Ref: <https://github.com/andialbrecht/sqlparse>

    pip install sqlparse

```python
import sqlparse
print(sqlparse.format('select * from dual', reindent=True))
```

Windows
=======

Rodar script sem abrir janela.

1. Renomear extensão para .pyw no lugar de .py
2. Rodar com pythonw.exe no lugar de python.exe


Funções diversas
================

```python
# Agrupamento de dígitos

import locale
locale.setlocale(locale.LC_ALL, 'ptb')
locale.format('%.2f', 12345678.9, grouping=True)

# Abrir um browser para exibir uma URL específica

import webbrowser
webbrowser.open('http://www.google.com', new=0, autoraise=True);

# Pausar o programa por 10 segundos

import time
time.sleep(10)
```

Oneliners
=========

Descobrir o diretório site-packages da instalação

    python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"

Quantos bytes tem um kbyte, mbyte, gbyte e tbyte?

    python -c "for x, y in zip(('Byte', 'KByte', 'MByte', 'GByte', 'TByte'), (1 << 10*i for i in range(5))): print(x, y)"

Quais valores podem ser representados com N bytes?

    python -c "for n in range(1, 11):print('bytes={0:2} bits={1:2} lo={2:25} hi={3:25} unsig={3:25}'.format(n,n*8,-2**(n*8)//2,2**(n*8)//2-1,2**(n*8)-1))"

Converter para base 64

    python -c "import base64, sys; base64.encode(open(sys.argv[1], 'rb'), open(sys.argv[2], 'wb'))" original.file converted.u64

Converter de base 64

    python -c "import base64, sys; base64.encode(open(sys.argv[1], 'rb'), open(sys.argv[2], 'wb'))" original.u64 original.file

Converter arquivo CSV para JSON

    python -c "import csv,json;print(json.dumps(list(csv.reader(open(r'test.csv')))))"

Servidor HTTP estático na porta 8086 em uma linha

> !!! Atenção !!! Usar só para testes, abre uma brecha de segurança!

    cd \caminho\com\arquivos
    python -m http.server 8086

Contar quantas linhas tem o arquivo de entrada

    python -c "import sys; print(len(sys.stdin.readlines()))" < arquivo.txt

3 primeiras linhas da entrada

    python -c "import sys; print(''.join(sys.stdin.readlines()[:3]))" < arquivo.txt

3 últimas linhas da entrada

    python -c "import sys; print(''.join(sys.stdin.readlines()[-3:]))" < arquivo.txt

Inverter as linhas da entrada

    python -c "import sys; print(''.join(sys.stdin.readlines()[::-1]))" < arquivo.txt

Ordenar as linhas da entrada

    python -c "import sys; print(''.join(sorted(sys.stdin.readlines())))" < arquivo.txt

Imprimir somente o último campo de cada linha de um arquivo com campos separados por vírgula

    python -c "import sys; print('\n'.join([line.strip().split(',')[-1] for line in sys.stdin.readlines()]))" < arquivo.csv

Encontrar palíndromos

    python -c "import sys; print('\n'.join([line.strip() for line in sys.stdin.readlines() if line.strip() == line.strip()[::-1]]))" < words.txt

Caixa de mensagem

    python -c "import tkinter.messagebox as mb; mb.showinfo('Titulo', 'Mensagem')"
    python -c "import tkinter.messagebox as mb; x = mb.askquestion('Titulo', 'Mensagem')"

Calendário

Texto

    python -c "import calendar as cal; import datetime as dt; print(cal.calendar(dt.datetime.today().year))"

HTML

    python -c "import calendar as cal;import datetime as dt;from lxml import etree,html;print(etree.tostring(html.fromstring(cal.HTMLCalendar().formatyear(dt.datetime.today().year)),encoding='unicode',pretty_print=True))" > calendario.html

Saída colorida

    python -c "from colorama import *;print(Fore.YELLOW+Back.BLUE+'Yellow+blue');print(Fore.BLACK+Back.GREEN+'Black+green');print(Style.RESET_ALL)"

Processamento paralelo
======================

```python
import multiprocessing
import math

print(list(multiprocessing.Pool(processes=4).map(math.exp,range(1,11))))
```

Anaconda + Http Proxy
=====================

    set HTTPS_PROXY=https://servidor.proxy:8080
    set HTTP_PROXY=http://servidor.proxy:8080

    conda install XXXX

Análise de dados
================

```python
# Importar numpy (np) e matplotlib.pyplot (plt)
%pylab

# Conexão ao banco e query para obter os dados a serem analisados
import cx_Oracle
db = cx_Oracle.connect('USUARIO/SENHA@BANCO')
cursor = db.cursor()
jm00 = list(cursor.execute("select * from monjob_execucao where id_monjob_procedimento = 'JM00' and data_inicio is not null and data_fim is not null and resultado = 0 order by data_inicio"))

# Converter dados carregados em matriz
X = np.array(jm00)

# Executa query, converte cursor para lista e desta para uma matriz
query = '''
select *
  from monjob_execucao
 where id_monjob_procedimento = 'JM00'
   and data_inicio is not null
   and data_fim is not null
   and resultado = 0
 order by data_inicio
'''
X = np.array(list(cursor.execute(query)))

# Lista de colunas do cursor e seus índices
list(enumerate(cursor.description))

# Transformar os dados de uma coluna em uma matriz
redo_write = np.array([x[13] for x in jm00])

# Histograma para uma determinada coluna da fonte de dados. Ex: CPU Used
plt.hist([x[16] for x in jm00])

# Gráfico de dispersão. Ex: Physical Read+Write X CPU Used
plt.scatter([x[11] + x[12] for x in jm00], [x[16] for x in jm00])

# Pega somente as colunas numéricas
X = (np.array(jm00)[:, 6:]).astype(int64)

# Pandas + Seaborn

import pandas as pd
import seaborn as sns
cols = ['REDE_TX', 'REDE_RX', 'DBLINK_TX', 'DBLINK_RX', 'LOGICAL_READ',
        'PHYSICAL_READ', 'PHYSICAL_WRITE', 'REDO_WRITE', 'PARSE_CPU',
        'PARSE_ELAPSED', 'CPU_USED', 'CPU_RECURSIVE', 'EXECUTE_COUNT',
        'USER_COMMIT', 'USER_ROLLBACK']

# Cria um dicionário em que cada chave é uma "coluna" dos dados
data_dict = {}
for i, col in enumerate(cols):
    data_dict[col] = X[:, i]

# Cria um dataframe do pandas com o dicionário criado e gera uma plotagem de pares com o Seaborn
data = pd.DataFrame(data_dict)
sns.pairplot(data)

# Criar dataframe direto do resultado da query
df = pd.read_sql_query(query, db) #query: string, db: conexão

           
# Histogramas de todas as colunas em subplots
ncols = 3
nrows = 7
for i, column_name in enumerate(df.columns):
    subplot(nrows, ncols, i+1) # Primeiro subplot começa com 1 e não 0
    title(column_name)
    try:
        hist(log(df[column_name]), 30);
    except:
        print('Não foi possível plotar {0}'.format(column_name))
        pass

# Equivalente mais fácil:
df.plot.hist(subplots=True, layout=(nrows, ncols))

# Plotagem simples dos dados em ordem sequencial
ncols = int(ceil(sqrt(len(df.columns)))) # Número de colunas no subplot
nrows = int(ceil(len(df.columns) / ncols)) # Linhas
for i, column_name in enumerate(df.columns):
    subplot(nrows, ncols, i+1) # Primeiro subplot começa com 1 e não 0
    title(column_name)
    try:
        plot(df[column_name], 'r.');
    except:
        print('Não foi possível plotar {0}'.format(column_name))
        pass

# Equivalente mais fácil:
df.plot.line(subplots=True, layout=(nrows, ncols), style=['.',]*len(df.columns))

# Algums estatísticas básicas dos dados
df.describe()

# Curva de Gauss
x = linspace(mu-0.1, mu+0.1, 100)
y = (1/sqrt(sigma*sqrt(2*pi))*exp((-1/2)*((x-mu)/sigma)**2))
plot(x,y)
```

Pandas
======

Mais exemplos:
- <http://pandas.pydata.org/pandas-docs/stable/10min.html>
- <http://pandas.pydata.org/pandas-docs/stable/cookbook.html>

```python
# Principais imports
import pandas as pd
import numpy as np

### Criação de objetos ###

# Cria uma série de dados numérica (indexado por padrão de 0 a N)
s = pd.Series([1, 3, 5, np.nan, 6, 8])

# Cria uma série de dados alfanumérica
s2 = pd.Series(list('abcdefghij'))

# É possível forçar um tipo de dados e uma indexação específica (1, 3, 5, ...)
pd.Series(1, index=(range(1, 10, 2)), dtype='float32')

# Retorna uma nova série sem os valores vazios (N/A)
s.dropna()

# Cria uma série de datas
dates = pd.date_range('2016-01-01', periods=12)

# Cria uma série de dados aleatórios (a partir de uma matriz aleatório do numpy) indexados pela data (série gerada acima) e com as colunas A, B, C, D (string "quebrada" em caracteres)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))

# Cria uma série de dados alimentados por um dicionário
pd.DataFrame({ 'A': 1,                          # Este valor será repetido em todas as linhas
               'B': list(range(0, 101, 10))})   # Esta coluna terá os valores 0, 10, 20, 30, ...

# Um exemplo mais complexo
df2 = pd.DataFrame({ 'A': 1.0,                                               # O mesmo valor é replicado em todas as linhas
                     'B': pd.Timestamp('20130101'),                          # A mesma data é replicada
                     'C': pd.Series(1, index=(range(4)), dtype='float32'),   # Série de dados indexado de 0-3 com o valor 1 (formatado como float 32-bits)
                     'D': np.array([3] * 4, 'int'),                          # Gera um array NumPy a partir de uma lista de 4 elementos com o valor fixo 3, todos os elementos inteiros
                     'E': pd.Categorical(['test', 'train', 'test', 'train']),# Categoria de dados
                     'F': 'foo' })                                           # A mesma string é replicada

df2.dtypes     # tipos das colunas

# Consultando os dados
df.head(3)    # primeiras N linhas (padrão 5)
df.tail(3)    # últimas N linhas (padrão 5)
df.index      # índices da série de dados
df.columns    # colunas da série de dados
df.values     # valores como uma matriz NumPy
df.describe() # estatísticas básicas dos dados
df.T          # tabela transposta
df.sort_index(axis=1, ascending=False) # reordena as colunas em ordem decrescente pelo título | eixo 1 = colunas
df.sort_index(axis=0, ascending=False) # reordena as linhas em ordem decrescente pelo índice (label) | eixo 0 = linhas
df.sort_values(by='B')                 # reordena os valores por uma coluna específica

# Selecionando os dados por "label" (usando exemplo indexado por datas acima)
df['20130101'] -ou- df[dates[0]]
df.loc['20130101'] -ou- df.loc[dates[0]]
df.loc['20130101', ['A', 'B']] -ou- df.loc[dates[0], ['A', 'B']]
df.loc[:, ['A', 'B']]
df.at[dates[0], 'A']

# Seleção por posição
df.iloc[0]
df.iloc[0:3]
df.iloc[0:3, 1:3]
df.iloc[[0,2,4], [1,3]]
df.iat[1,1]

# Seleção booleana
df[df.A > 0 and df.B < 0]
df[df < 0].fillna(value=0) + df[df > 0].fillna(value=0) == df
df.isnull()

# Plotagem
ts = pd.Series(np.random.randn(1000), index=pd.date_range('20130101', periods=1000)) # Cria uma séries aleatórias com 1000 elementos indexada por data
ts = ts.cumsum() # Soma cumulativa por coluna. Ex: 1 2 3 4 5 6 vira -> 1 3 6 10 15 21
ts.plot() # Faz a plotagem dos dados

df4 = pd.DataFrame(np.random.randn(1000, 4), index=pd.date_range('20130101', periods=1000), columns=['alfa', 'beta', 'gamma', 'delta']) # Cria uma séries aleatórias com 1000 linhas e 3 colunas indexada por data
df4 = df4.cumsum()
df4.plot()

# Dados faltantes (np.nan / NAN)

# Exemplo (gera um df aleatório com dados faltando):
dates = pd.date_range('20130101', periods=6)
df2 = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
df2 = df2[df2 > 0]

# Nota: Não modifica os valores, apenas gera cópia modificada
df2.dropna(how='any')              # se houve alguma coluna faltante, remove a linha
df2.dropna(how='all', subset)      # se todas as colunas do subset estiveram vazias, remove a linha
df2.fillna(value=-1)               # preenche valores NAN
df2.fillna(value=-1, inplace=True) # modifica o original!
df2.isnull()                       # máscara booleana dos valores vazios
df2[df2.isnull()] = -1             # modifica o original!

# Operações
df.mean()           # Média por coluna
df.mean(1)          # Média por linha
df.apply(np.cumsum) # Equivalente df.cumsum()
df.apply(lambda x: x.max() - x.min())

# Histograma
s = pd.Series(np.random.randint(0, 9, size=100))
s.value_counts()
s.hist()

# Manipulação de dados

# Concatenação
df1 = pd.DataFrame(np.random.randn(6, 4), columns=list('ABCD'))
df2 = pd.DataFrame(np.random.randn(6, 4), columns=list('ABCD'))
pd.concat([df1, df2])         # Concatena verticalmente
pd.concat([df1, df2], axis=1) # Concatena horizontalmente

# Junção/merge
df1 = pd.DataFrame({'key': ['a', 'b', 'c'], 'val': [1, 2, 3]})
df2 = pd.DataFrame({'key': ['a', 'b', 'c', 'c', 'b', 'a'], 'val': [10, 20, 30, 40, 50, 60]})
pd.merge(df1, df2, on='key')

# Exportando dados
df.to_excel('filename.xls', 'Sheet1')
df.to_csv('filename.csv', sep=';', quotechar='"', dateformat='dd-mm-yyyy')

# Importando dados
df = pd.read_excel('filename.xls', 'Sheet1')
df = pd.read_csv('filename.csv', sep=';', quotechar='"', index_col=0)

# Colunas com nome minúsculo
df.columns = [c.lower() for c in df.columns]

# Renomear colunas
df.rename(columns={'col1': 'coluna_1', 'col2': 'coluna_2'}, inplace=True)

# Modificando todos os valores de uma coluna
df.nome_coluna.apply(lambda x: x.strip())

# Criar coluna no final, preenchendo valores com zero
df['nova_coluna'] = pd.Series('', index=df.index)

# Criar coluna no começo (ou posição específica)
df.insert(loc=0, column='nova_coluna', value='')
```

# Exemplo Pandas+Oracle

```python
import cx_Oracle
import pandas as pd
connection = cx_Oracle.connect('usuario', 'senha', 'banco')
with connection:
    try:
        df_tables = pd.read_sql_query('select * from user_tables', connection)
    except cx_Oracle.DatabaseError as dberror:
        print(dberror)

# Pivot

df.head()

#        WAITCLASS         SAMPLE_TIME  DB_TIME
# 0    Application 2016-10-06 08:15:00      0.0
# 1         Commit 2016-10-06 08:15:00      0.0
# 2    Concurrency 2016-10-06 08:15:00      0.0
# 3  Configuration 2016-10-06 08:15:00      0.0
# 4            CPU 2016-10-06 08:15:00      0.0

pdf = df.pivot(index='SAMPLE_TIME', columns='WAITCLASS', values='DB_TIME')

pdf.head()

# WAITCLASS              CPU  Commit  Concurrency  Configuration  Network  Other  Scheduler  System I/O  User I/O 
# SAMPLE_TIME                                                                                                     
# 2015-08-17 12:47:00  0.000       0            0              0        0  0.000          0           0         0 
# 2015-08-17 12:48:00  0.002       0            0              0        0  0.001          0           0         0 
# 2015-08-17 12:49:00  0.001       0            0              0        0  0.002          0           0         0 
# 2015-08-17 12:50:00  0.000       0            0              0        0  0.000          0           0         0 
# 2015-08-17 12:51:00  0.000       0            0              0        0  0.000          0           0         0 
 
# Estilo GGPlot
from matplotlib import style
style.use('ggplot')

# Estilo temporário

with plt.style.context(('dark_background')):
    plt.plot(np.sin(np.linspace(0, 2 * np.pi)), 'r-o')

plt.show()

# Áreas empilhadas
pdf.plot(kind='area', 
         stacked=True, 
         title='DB Time over the last hour', 
         color=['red', 'green', 'orange', 'darkred', 'brown', 'brown', 'pink', 'lightgreen', 'cyan', 'blue'])
plt.show()
```

Oracle e Python
================

```python
import cx_Oracle

# Abrir conexão (todas equivalentes)
db = cx_Oracle.connect('USUARIO/SENHA@BANCO')
db = cx_Oracle.connect('USUARIO', 'SENHA', 'BANCO')
db = cx_Oracle.connect('USUARIO', 'SENHA', 'SERVIDOR:6500/BANCO')
db = cx_Oracle.connect('USUARIO', 'SENHA', '(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST=SERVIDOR)(PORT=6500)))(CONNECT_DATA=(SID=BANCO)))')
db = cx_Oracle.connect('USUARIO', 'SENHA', '(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=SERVIDOR)(PORT=6500))(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=BANCO)))')

# Criar descrição de conexão TNS a partir do host, porta e serviço
dsn_tns = cx_Oracle.makedsn('SERVIDOR', 6500, 'BANCO')
# Resultado: dsn_tns = '(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST=SERVIDOR)(PORT=6500)))(CONNECT_DATA=(SID=BANCO)))'

# Fechar conexão
db.close()

# Atributos da conexão
db.autocommit
db.current_schema
db.dsn
db.encoding
db.maxBytesPerCharacter
db.nencoding
db.tnsentry
db.username
db.version

# Criar um cursor (necessário para executar queries e comandos)
cursor = db.cursor()

# Algumas formas de executar queries e obter resultados

# Executa query e retorna uma linha
cursor.execute('SELECT * FROM DUAL')
row = cursor.fetchone()
print(row)

# Executa query e retorna várias linhas de uma vez passando parâmetros para a query por nome
cursor.execute('SELECT * FROM arq_conteudo WHERE id_arq_conteudo BETWEEN :minid AND :maxid', minid=50, maxid=60)
rows = cursor.fetchall() # Alternativa: rows = list(result)
print(rows)
print(cursor.bindnames()) # Lista os parâmetros do comando preparado -> ['minid', 'maxid']

# Executa query e retorna várias linhas de uma vez passando parâmetros para a query por posição (o nome dos parâmetros não importa!)
cursor.execute('''SELECT id_cartao, nome, status_cartao, id_tipocar
                    FROM cartao
                   WHERE status_cartao = :alfa
                     AND id_tipocar = :beta
                     AND ROWNUM <= :gama ''', ('A', 'P', 10))
while True:
    row = cursor.fetchone() # Alternativa abaixo...
    if not row:
        break
    print(row)


# Executa query e retorna uma linha de cada vez usando o cursor como um 'iterator'
cursor.execute(b'SELECT * FROM arq_conteudo ORDER BY 1')
for row in cursor:
    print(row)

# Prepara uma query para execução e roda múltiplas vezes alterando apenas o parâmetro
cursor.prepare('SELECT * FROM arq_conteudo WHERE id_arq_conteudo = :id')
for id in (10, 20, 40):
    cursor.execute(None, id=id) # Não é necessário repetir a query, usar None usa a query já preparada
    row = cursor.fetchone()
    if row:
        print(row)

# Executando DDL e inserção múltipla

# DDL para criar tabela
db = cx_Oracle.connect('USUARIO/SENHA@BANCO')
cursor = db.cursor()
ddl_cmd = '''
CREATE TABLE tmp_teste7777
(
  module_name VARCHAR2(100) NOT NULL,
  file_path   VARCHAR2(300) NOT NULL
)
'''
cursor.execute(ddl_cmd)

# Preenche uma lista com dados para inserção
from sys import modules
module_data = []
for module_name, module_info in modules.items():
    try:
        module_data.append((module_name, module_info.__file__))
    except AttributeError:
        pass

# Prepara o DML para inserção e executa em seguida passando a lista dos dados
cursor.prepare('INSERT INTO tmp_teste7777 VALUES (:1, :2)')
cursor.executemany(None, module_data)
db.commit()
cursor.execute('SELECT COUNT(*) FROM tmp_teste7777')
print(cursor.fetchone())

# DDL para remover tabela
cursor.execute('DROP TABLE tmp_teste7777')
```

# Exemplos Oracle (Schema HR)

```python

import cx_Oracle
import datetime
db = cx_Oracle.connect('hr/password@oracle')
c = db.cursor()

# Inserção usando dicionário e variáveis de substituição nomeadas

employee_data = {
    employee_id: 1000,
    first_name: 'Nome',
    last_name: 'Sobrenome',
    email: 'EMAIL',
    phone_number: '6457',
    hire_date: datetime.date(2012, 9, 17),
    job_id: 'IT_PROG',
    salary: 1000,
    commission_pct: 0,
    manager_id: 103,
    department_id: 60
}

insert_cmd = '''
INSERT INTO employees
( employee_id, first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id )
VALUES
( :employee_id, :first_name, :last_name, :email, :phone_number, :hire_date, :job_id, :salary, :commission_pct, :manager_id, :department_id )
'''

c.execute(insert_cmd, employee_data)
db.commit()

# Separando tupla em campos
c.execute('SELECT * FROM employees WHERE employee_id = :employee_id', employee_id=1000)
row = c.fetchone()
(employee_id, first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id) = row
print(row)
print(datetime.date.today() - hire_date)
```

# CSV

```python
import cx_Oracle
import csv
db = cx_Oracle.connect('hr/password@oracle')
cursor = db.cursor()

# Exportar tabela para CSV
fileobj = open('employees.csv', 'w')
writer = csv.writer(fileobj, lineterminator="\n", delimiter=';', quoting=csv.QUOTE_NONNUMERIC)
cursor.execute('SELECT * FROM employees')
for row in cursor:
    writer.writerow(row)
fileobj.close()

# Importar dados em CSV para tabela
cursor.execute('CREATE TABLE employees2 AS SELECT * FROM employees WHERE 1 = 2') # Duplicar a estrutura de uma tabela em uma nova tabela vazia
reader = csv.reader(open('employees.csv'), delimiter=';')
employees = list(reader)
cursor.execute("ALTER SESSION SET NLS_DATE_FORMAT = 'yyyy-mm-dd hh24:mi:ss'") # Ajuste na formatação da data antes da formatação
cursor.execute("ALTER SESSION SET NLS_NUMERIC_CHARACTERS = '.,'") # Ajuste no separador de decimal e agrupamento
cursor.prepare('INSERT INTO employees2 VALUES (:1, :2, :3, :4, :5, :6, :7, :8, :9, :10, :11)')
cursor.executemany(None, employees)
db.commit()
```

```python
# Ler arquivo de entrada CSV, gerar um resumo (agrupamento) e salvar resultado em outro CSV
import pandas as pd
estoque_conductor = r'Arquivo Estoque Titulos em Aberto e Parcial.csv'
df_conductor = pd.read_csv(estoque_conductor, sep=',', parse_dates=True, infer_datetime_format=True, quotechar='"', quoting=1, encoding='UTF-8', low_memory=False)
df_lastro = d.groupby('DtVencimento').agg({'NUMERO': 'count', 'Documento': 'count'}).sort_index()
df_lastro.to_csv(r'saida_lastro.csv')

#Melhor tratamento das datas e valores
df2 = pd.read_csv(estoque_conductor, sep=',', dtype='object', parse_dates=['DtAquisicao', 'DtVencimento','DtLiquidacao'], quotechar='"', quoting=1, encoding='UTF-8', low_memory=False)
f = lambda x: float(x.replace('R$ ', '').replace('.', '').replace(',', '.'))
df2[['VALORPARCELA', 'ValorAquisicao', 'ValorNominal', 'ValorBaixadoCdt']] = df2[['VALORPARCELA', 'ValorAquisicao', 'ValorNominal', 'ValorBaixadoCdt']].applymap(f)
```

# Transações

```python
db.begin();         # Inicia transação
db.commit();        # Confirma transação
db.rollback();      # Cancela transação

# Nota: Todo comando DDL (CREATE, DROP, ALTER) possui um "commit" implícito
db.close()          # Ao fechar a conexão, é feito ROLLBACK e não commit!
db.autocommit = 1   # Ativa autocommit (commit após cada instrução DML)
db.autocommit = 0   # Desativa autocommit
```




# Reunindo dados de 3 bases para fazer uma plotagem


```python
# Conexão nas bases
import cx_Oracle
import pandas as pd

dbA = cx_Oracle.connect(xxxxxxxx);
dbB = cx_Oracle.connect(xxxxxxxx);
dbC = cx_Oracle.connect(xxxxxxxx);


# Queries utilizadas
qry_A = """
select trunc(data_operacao, 'd') as data, count(*) as qtde_A_
  from trs_operacao
 where id_trs_estado = 'C'
   and data_operacao >= trunc(sysdate-360) 
   and data_operacao < trunc(sysdate)
 group by trunc(data_operacao, 'd')
 order by 1
"""

qry_BC = """
select trunc(datacomp, 'd') as data, count(*) as qtde
  from contrato
 where datacomp >= trunc(sysdate-360)
   and datacomp < trunc(sysdate)
   and id_cartao is not null
 group by trunc(datacomp, 'd')
"""


# Cria os dataframes com os dados de cada base
df_A = pd.read_sql_query(qry_A, dbA, index_col='DATA')
df_B = pd.read_sql_query(qry_BC, dbB, index_col='DATA')
df_C = pd.read_sql_query(qry_BC, dbC, index_col='DATA')


# Faz o join 
df = df_A.join(df_B, how='outer').join(df_C, how='outer', lsuffix='_B', rsuffix='_C').fillna(value=0)
df.columns = ['A', 'B', 'C']


# Faz a plotagem do gráfico de área (empilhado)
df.plot.area()

# Exibe o gráfico
import matplotlib.pyplot as plt
plt.show()


# Percentual
df2 = df.divide(df.sum(axis=1), axis=0)
df2.plot.area()
plt.show()

dbA.close()
dbB.close()
dbC.close()
```

