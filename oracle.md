---
title: Oracle
---


Referência e exemplos (snippets) de comandos SQL e PL/SQL e outras ferramentas para Oracle Database.

> **Aviso:** Os comandos foram usados e testados nas versões 9 e 11.

# SQL

## Sintaxe de consultas

~~~sql
SELECT [DISTINCT] campos 
FROM tabelas
WHERE condicoes/joins
GROUP BY campos de agrupamento
HAVING condicoes envolvendo funcoes de grupo
ORDER BY campo de ordenacao [DESC]
~~~

## Operadores lógicos

~~~sql
= < > <= >= != <>
IS NULL, IS NOT NULL, IN, NOT IN, LIKE, NOT LIKE, EXISTS (?), valor BETWEEN menor_valor AND maior_valor
AND, OR, NOT
~~~

LIKE aceita `_` para substituir um caractere ou `%` para vários.

Para utilizar os próprios caracteres `%` ou `_`, usar a keyword ESCAPE e um caractere de escape.

    texto LIKE '%\%%' ESCAPE '\'

Para máscaras mais flexíveis, ver o uso de REGEXP_LIKE.

## Valores Nulos

Obs: = NULL e != NULL não dão o resultado esperado, use IS NULL / IS NOT NULL

## Operadores

    + - * /
    MOD - Resto da divisão. Exemplo: MOD(10, 3) --> 1
    || - Concatena duas strings. Exemplo: 'Tes' || 'te' --> 'Teste'

## Funções de grupo

    MIN, MAX, AVG, SUM, COUNT

Obs: Não consideram campos com valor NULL (use NVL para substituir valor nulo por outro)


## Funções

- `UPPER`: Converte todas as letras para maiúscula
- `LOWER`: Converte todas as letras para minúscula
- `INITCAP`: Converte as letras iniciais para maiúsculas e as demais para minúsculas.
- `NVL(campo, valor_padrao)`: troca o valor do campo para valor_padrao caso seja nulo, ou mantém valor original.
- `NVL2(campo, valor_nao_nulo, valor_nulo)`: retorna valor_nao_nulo se o valor da expressão 'campo' não for nulo e retorna valor_nulo se for. OBS - NVL2 só funciona em queries e não em blocos PL/SQL.
- `COALESCE(valor1, valor2, valor3, ...)`: retorna o primeiro valor não-nulo.
- `NULLIF(expr1, expr2)`: retorna NULL se expr1 e expr2 forem iguais ou expr1 se forem diferentes.
- `LENGTH(string)`: Mostra o comprimento de uma string
- `SUBSTR(string, pos_inicio, tamanho)`: Retorna parte de uma string
- `CONCAT(s1, s2)`: Concatena duas strings em uma só (veja ||)

- `LPAD(string, n, caractere)`: Preenche string com até n vezes 'caractere' à esquerda. Obs: Trunca à esquerda se string for maior que 'n'.
- `RPAD(string, n, caractere)`: Igual, mas à direita.

- `TO_DATE('11/12/13', 'DD/MM/YY')`: Converte texto para data.
- `TO_CHAR(campo_data, 'MM/YY')`: Converte de data para texto no formato especificado.
- `SYSDATE`: Retorna data e hora atual do sistema em que está o banco de dados
- `CURRENT_DATE`: Retorna data e hora no fuso horário da sessão
- `SYSTIMESTAMP/CURRENT_TIMESTAMP/LOCALTIMESTAMP `: Retorna data e hora com precisão de milissegundos (útil para benchmarks)
- `DBTIMEZONE/SESSIONTIMEZONE `: Fuso horário do banco de dados/da seção
- `ADD_MONTHS(data, num)`: Adiciona 'num' meses à data atual (num pode ser negativo).
- `MONTHS_BETWEEN(data1, data2)`: Calcula quantos meses há entre as duas datas. Se data1 for posterior à data2, o valor retornado é positivo. Se data1 é anterior à data2, o valor retornado é negativo.
- `NEXT_DAY(data, 'FRIDAY')`: retorna a próxima sexta-feira depois da data
- `LAST_DAY`: retorna o último dia do mês da data especificada.

- `TRUNC`: trunca um valor (zera casas significativas). Ex: TRUNC(123.456, 2) trunca para até 2 casas decimais. Para data/hora, trunca para meia-noite.

- `ROUND(valor, casas)`: arredonda um valor para o número de casas definidas.

- `TRUNC(data, 'MONTH')`: Força data para início do mês
- `ROUND(data, 'MONTH')`: Força data para início do mês atual (<= dia 15) ou próximo mês (> 15)

- `REGEXP_LIKE(texto, 'expressao')`: Retorna verdadeiro se a expressão regular 'expressao' estiver presente no texto.

## Formatação (TO_DATE/TO_CHAR)

- `DD`: Dia com dois dígitos
- `MM`: Mês com dois dígitos
- `YY`: Ano com dois dígitos
- `DY`: Dia da semana com três letras
- `DAY`: Dia da semana por extenso
- `Ddsp`: Dia do mês por extenso (one, two, three, etc.)
- `Ddspth`: Dia do mês por extenso em ordinal (first, second, third, etc.)
- `MON`: Mês abreviado para três letras
- `MONTH`: Mês por extenso
- `YY`: Somente os 2 últimos dígitos do ano
- `YYYY`: Ano completo em 4 dígitos
- `YEAR`: Ano por extenso
- `FM`: Suprime espaços em branco ou zeros à esquerda (??)
- `"literal"`: Coloca o conteúdo entre aspas da forma que está
- `FM$9,999,999`: Separa casas de milhar
- `9`: representa um dígito numérico
- `0`: força a exibição de um zero
- `$`: Símbolo de dólar flutuante
- `L`: Usa o símbolo da moeda local
- `.`: Vírgula decimal (no idioma português, é exibido como ,)
- `,`: Indicador de milhar (no idioma português, é exibido como .)

Extraindo partes de uma data:

~~~sql
SELECT EXTRACT(YEAR FROM hire_date) AS ano
     , EXTRACT(MONTH FROM hire_date) AS mes
     , EXTRACT(DAY FROM hire_date) AS ano
  FROM employees
 ORDER BY 1;
~~~

~~~sql
SELECT cliente, nascim, sysdate, extract(year from (sysdate-nascim) year to month) as idade from clientes where rownum <= 20;
~~~

Avança 1 dia

~~~sql
SELECT SYSDATE + 1 FROM DUAL;
SELECT SYSDATE + INTERVAL '1' DAY FROM DUAL;
~~~

Avança 1 hora

~~~sql
SELECT SYSDATE + 1/24 FROM DUAL;
SELECT SYSDATE + INTERVAL '1' HOUR FROM DUAL;
~~~

Avança 1 minuto

~~~sql
SELECT SYSDATE + 1/1440 FROM DUAL;
SELECT SYSDATE + 1/24/60 FROM DUAL;
SELECT SYSDATE + INTERVAL '1' MINUTE FROM DUAL;
~~~

Avança 1 segundo

~~~sql
SELECT SYSDATE + 1/86440 FROM DUAL;
SELECT SYSDATE + 1/24/60/60 FROM DUAL;
SELECT SYSDATE + INTERVAL '1' SECOND FROM DUAL;
SELECT SYSDATE + 0.00001 FROM DUAL; -- (impreciso para mais de um segundo!)
~~~

Avança 1 ano

~~~sql
SELECT ADD_MONTHS(SYSDATE, 12) FROM DUAL;
SELECT SYSDATE + INTERVAL '1' YEAR FROM DUAL;
~~~

Avança 1 mês

~~~sql
SELECT ADD_MONTHS(SYSDATE, 1) FROM DUAL;
SELECT SYSDATE + INTERVAL '1' MONTH FROM DUAL;
~~~

Avança 1 semana
~~~sql
SELECT SYSDATE + 7 FROM DUAL;
SELECT SYSDATE + INTERVAL '7' DAY FROM DUAL;
~~~

Último segundo do dia

~~~sql
SELECT TRUNC(SYSDATE+1) - 1/86440 FROM DUAL;
SELECT TRUNC(SYSDATE+1) - 1/24/60/60 FROM DUAL;
SELECT TRUNC(SYSDATE+1) - INTERVAL '1' SECOND FROM DUAL;
SELECT TRUNC(SYSDATE) + 0.99999 FROM DUAL;
~~~


Gerar números aleatórios
------------------------

Gera um número aleatório >= 0 e < 1 (0 a 0.9999...).

~~~sql
SELECT dbms_random.value() from dual;

DBMS_RANDOM.VALUE()
-------------------
        ,660340453
~~~

Gera um número aleatório >= 1 e < 10 (entre 1.000 e 9.999...)

~~~sql
SELECT dbms_random.value(1, 10) from dual;

DBMS_RANDOM.VALUE(0,10)
-----------------------
             7,04307917
~~~

Irá gerar um número entre 1 e 9

~~~sql
SELECT trunc(dbms_random.value(1, 10)) from dual;

TRUNC(DBMS_RANDOM.VALUE(1,10))
------------------------------
                             7
~~~




## Versão do Oracle

~~~sql
SELECT * FROM v$version WHERE banner LIKE 'Oracle%';
~~~




## Transações

`COMMIT`: Confirma todas as operações executadas desde o último COMMIT/ROLLBACK até este ponto.

`ROLLBACK`: Reverte todas as operações executadas desde o último COMMIT/ROLLBACK até este ponto.

`SAVEPOINT nome_do_ponto`: Cria um ponto intermediário até onde pode ser feito um ROLLBACK parcial.

`ROLLBACK TO nome_do_ponto`: Reverte todas as alterações feitas após o savepoint.



Manipulação de dados
--------------------

Inserção de dados:

~~~sql
INSERT INTO tabela VALUES (valores de todos os campos)

INSERT INTO tabela (campo1, campo2)
VALUES (valor1, valor2)
~~~

Remoção de dados:

~~~sql
DELETE FROM tabela -- Atenção: remove TODOS os registros se não usar cláusula WHERE

DELETE FROM tabela WHERE condicoes

TRUNCATE tabela -- Remove os dados liberando espaço. Obs: não funciona se constraints estiverem ativas.

DELETE FROM tabela WHERE ROWNUM <= 100000;
COMMIT;
-- Ao remover muitos registros, é recomendável limitar o número de registros removidos por transação e repetir o comando múltiplas vezes.
~~~


Atualização:

~~~sql
UPDATE tabela
SET campo1 = valor1, campo2 = valor2
WHERE condicoes
~~~

Sequences:

~~~sql
nome_da_sequence.NEXTVAL -- Gera um novo valor e retorna
nome_da_sequence.CURRVAL -- Retorna o último valor devolvido por NEXTVAL 
~~~

Atenção:
- NÃO retorna o valor "atual" da sequence!
- Só funciona se NEXTVAL foi usado antes.



Sintaxe "clássica" para JOINs
-----

### INNER JOIN

~~~sql
SELECT tab1.col1, tab1.col2, tab2.col1, tab2.col2
FROM tab1, tab2
WHERE tab1.col1 = tab2.col1
~~~

Exemplos:

~~~sql
SELECT f.sobrenome, f.depart_id, d.nome
FROM funcionario f, departamento d
WHERE f.depart_id = d.id

SELECT d.id "Cód. Departamento", r.id "Cód. Região", r.nome "Nome Região"
FROM departamento d, regiao r
WHERE d.regiao_id = r.id
~~~


### OUTER JOIN

Lista todos os clientes, inclusive os que não possuem representante de vendas.

~~~sql
SELECT c.nome, f.sobrenome, f.id
FROM cliente c, funcionario f
WHERE c.repr_vendas_id = f.id (+)
ORDER BY f.id
~~~

### SELF JOIN

~~~sql
SELECT f.sobrenome || ' trabalha para ' || g.sobrenome
FROM funcionario f, funcionario g
WHERE f.gerente_id = g.id;
~~~

Sintaxe ISO para JOIN
---------------------

Todas as correspondências de A e B

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a INNER JOIN tab2 b
  ON a.c1 = b.c1 AND a.c2 = b.c2;
~~~

Join resumido com a cláusula USING

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a INNER JOIN tab2 b USING (c1, c2);
~~~

Natural Join (CUIDADO com campos de nome igual mas sem correspondência)

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a NATURAL JOIN tab2 b;
~~~

TODAS as linhas de A e as linhas de B que tem correspondência com A

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a LEFT OUTER JOIN tab2 b
  ON a.c1 = b.c1 AND a.c2 = b.c2;
~~~

TODAS as linhas de B e as linhas de A que tem correspondência com B

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a RIGHT OUTER JOIN tab2 b
  ON a.c1 = b.c1 AND a.c2 = b.c2;
~~~

TODAS as linhas de A e B com correspondência, todas as linhas de A sem correspondência em B e todas as linhas de B sem correspondência em A

~~~sql
SELECT a.c1, a.c2, b.c2, b.c3, b.c4
FROM tab1 a FULL OUTER JOIN tab2 b
  ON a.c1 = b.c1 AND a.c2 = b.c2;
~~~

## Non-Equijoins

~~~sql
CREATE TABLE job_grades (
  grade        CHAR(1)      NOT NULL,
  lowest_sal   NUMBER(10,2) NOT NULL,
  highest_sal  NUMBER(10,2) NOT NULL
);

ALTER TABLE job_grades
ADD (CONSTRAINT job_grades_pk PRIMARY KEY (grade));

INSERT INTO job_grades VALUES ('A', 1000, 2999);
INSERT INTO job_grades VALUES ('B', 3000, 5999);
INSERT INTO job_grades VALUES ('C', 6000, 9999);
INSERT INTO job_grades VALUES ('D', 10000, 14999);
INSERT INTO job_grades VALUES ('E', 15000, 24999);
INSERT INTO job_grades VALUES ('F', 25000, 40000);

SELECT e.last_name, e.salary, jg.grade
FROM employees e, job_grades jg
WHERE e.salary BETWEEN jg.lowest_sal AND jg.highest_sal;
~~~

## Natural Joins vs. Equijoin

~~~sql
select department_id, department_name, location_id, city
from departments natural join locations;

select department_id, department_name, locations.location_id, city
from departments, locations
where departments.location_id = locations.location_id;

select department_id, department_name, locations.location_id, city
from departments join locations
on departments.location_id = locations.location_id;

SELECT employee_id, city, department_name
FROM employees e
JOIN departments d
ON   d.department_id = e.department_id
JOIN locations l
ON   d.location_id = l.location_id;
~~~

Hints
-----

~~~sql
SELECT /*+ INDEX(recebimento RECEBIMENTO_IDX1) */ * FROM recebimento
WHERE id_divida IN ( 'F022531926', 'F022604510', 'F022606172',
'F022415187', 'F022486229', 'F022598143', 'F022400931', 'F022541030', 'F022023985', 'F022604015',
'F021951506', 'F022556968', 'F022037057', 'F022541214', 'F022604259', 'F022603325', 'F022607168',
'F022604116', 'F021906232', 'F022409373', 'F022558075', 'F021888482', 'F022477617', 'F022604349',
'F022604511', 'F022603598', 'F022604640', 'F022604184')
AND codmloja = 'F227'
AND codfilial = '01'
AND id_tppag = 'B'
AND datadep >= '01/09/2014'
;
~~~

Referências:
- <http://www.devmedia.com.br/utilizando-hints-no-oracle/4557>
- <https://docs.oracle.com/cd/B12037_01/server.101/b10752/hintsref.htm>



Tabelas
-------

Remoção de tabelas:

~~~sql
DROP TABLE nome_tabela;
~~~

Criação de tabela:

~~~sql
CREATE TABLE tabela (
  campo_id  tipo(tamanho) NOT NULL,
  campo     tipo(tamanho),
  ...
  CONSTRAINT tabela_campo_pk PRIMARY KEY (campo_id)
);
~~~

Tabela com auto-referência e referência externa:

~~~sql
CREATE TABLE empregado (
  emp_id      NUMBER(4) NOT NULL,
  nome        CHAR(20),
  cargo       CHAR(20),
  gerente_id  NUMBER(4) CONSTRAINT emp_gerente_id
    REFERENCES empregado(emp_id),
  contratacao DATE,
  salario     NUMBER(7,2),
  depart_id   NUMBER(2) NOT NULL,
  CONSTRAINT emp_depart_id
    FOREIGN KEY (depart_id)
    REFERENCES depart (depart_id),
  CONSTRAINT emp_primary_key PRIMARY KEY (emp_id)
);
~~~

Com valor padrão para campo:

~~~sql
CREATE TABLE test (
  data_criacao  DATE DEFAULT SYSDATE
);
~~~

Criando tabela a partir de outra tabela/consulta:

~~~sql
CREATE TABLE copia_tabela AS
SELECT campo1, campo2, campo3 FROM outra_tabela
WHERE condicoes...
~~~

Criar uma tabela vazia com os mesmos campos de outra tabela:

~~~sql
CREATE TABLE copia_estrutura AS
SELECT * FROM outra_tabela WHERE 1=2;
-- Nota 1: A condição falsa (1=2) faz com que nenhuma linha seja retornada, mas o comando create table aproveita a estrutura das colunas.
-- Nota 2: Não são replicadas constraints, triggers, etc.
~~~

Se for necessário recriar uma tabela com todas as constraints:

~~~sql
SET LONG 10000
SET WRAP ON
SELECT dbms_metadata.get_ddl( 'TABLE', 'TABLE_NAME' ) AS script FROM DUAL;
~~~


Alterando o nome de uma tabela:

~~~sql
RENAME nome_objeto TO novo_nome
~~~

Criação de View:

~~~sql
CREATE VIEW nome_view AS
SELECT campo1, campo2, campo3, campo4 campo_renomeado
FROM tabela
WHERE condicoes...
~~~

Acrescentando-se a opção WITH CHECK OPTION CONSTRAINT nome_constraint_ck, ao inserir/alterar valores na view será verificado se os valores atendem as condições da cláusula WHERE, ou os valores inseridos na tabela base da view não ficará visível após operação.

Acrescentando-se a opção WITH READ ONLY torna a view somente leitura.

Consultando views: Acesse USER_VIEWS

Para verificar as colunas de uma view que podem sofrer alteração: USER_UPDATABLE_COLUMNS




Recuperar tabela excluída
-------------------------

Se o parâmetro `recyclebin` estiver habilitado, é possível recuperar uma tabela excluída (DROP TABLE) logo após a exclusão (e até algum tempo depois).

Verificar se parâmetro está habilitado:

~~~sql
SELECT name, value FROM v$parameter WHERE name = 'recyclebin';
~~~

Exemplo de uso:

~~~sql
DROP TABLE tab_test;
~~~

Consultar as tabelas que estão na "lixeira".

~~~sql
SELECT owner,object_name, original_name, type, droptime
FROM dba_recyclebin
WHERE original_name = 'TAB_TEST';
~~~

Para recuperar a tabela:

~~~sql
FLASHBACK TABLE tab_test TO BEFORE DROP;
~~~

Para limpar a "lixeira":

~~~sql
PURGE RECYCLEBIN;
~~~



Recuperar dados de uma tabela em um determinado momento
-------------------------------------------------------

Modelo:

~~~sql
SELECT * FROM tabela AS OF TIMESTAMP SYSTIMESTAMP - INTERVAL '1' MINUTE;
~~~

Exemplo:

~~~sql
15:30:53 SQL> CREATE TABLE test_flash AS SELECT 123 AS num FROM DUAL;

Tabela criada.

15:31:22 SQL> SELECT * FROM test_flash;
             NUM
----------------
             123

1 linha selecionada.

15:31:29 SQL> UPDATE test_flash SET num = 456 WHERE num = 123;

1 linha atualizada.

15:31:47 SQL> SELECT * FROM test_flash;
             NUM
----------------
             456

1 linha selecionada.

15:32:20 SQL> SELECT * FROM test_flash AS OF TIMESTAMP SYSTIMESTAMP - INTERVAL '1' MINUTE;
             NUM
----------------
             123

1 linha selecionada.
~~~



Índices
-------

São criados automaticamente quando é criada chave-primária ou chave única (PK, UK) ou podem ser criados manualmente.

Remover índice:

~~~sql
DROP INDEX tabela_campo_idx
~~~

Criar índice:

~~~sql
CREATE INDEX tabela_campo_idx ON tabela(campo)
~~~

Dicionário de dados:
- USER_INDEXES
- USER_IND_COLUMNS



Alterações de tabelas
---------------------

Adicionar uma coluna:

~~~sql
ALTER TABLE nome_tabela
ADD (nome_coluna tipo(tamanho))
~~~

Obs: Não é possível remover uma coluna, só recriando a tabela.

Aumentar tamanho de uma coluna:

~~~sql
ALTER TABLE nome_tabela
MODIFY (nome_coluna tipo(novo_tamanho))
~~~

Obs: Só é possível aumentar coluna, nunca diminuir. Tipo deve ser o mesmo!

Tornar coluna obrigatória/opcional:

~~~sql
ALTER TABLE nome_tabela
MODIFY (nome_coluna NULL)

ALTER TABLE nome_tabela
MODIFY (nome_coluna NOT NULL)
~~~

Adicionar uma constraint:

~~~sql
ALTER TABLE tabela
ADD CONSTRAINT tabela_campo_fk
FOREIGN KEY (campo)
REFERENCES outra_tabela(campo_chave)
~~~

Removendo uma constraint:

~~~sql
ALTER TABLE tabela
DROP CONSTRAINT nome_constraint
~~~

Deletar chave primária em cascada (em todas as tabelas que tenham relação):

~~~sql
ALTER TABLE tabela_mestra
DROP PRIMARY KEY CASCADE;
~~~

Desabilitar constraint (sem remover):

~~~sql
ALTER TABLE tabela
DISABLE CONSTRAINT nome_constraint CASCADE
~~~

Habilitar constraint:

~~~sql
ALTER TABLE tabela
ENABLE CONSTRAINT nome_constraint
~~~



Dicionário do esquema
---------------------

Tabelas com comentários sobre tabelas do esquema:

- ALL_COL_COMMENTS / USER_COL_COMMENTS
- ALL_TAB_COMMENTS / USER_TAB_COMMENTS

Adicionar comentário:

~~~sql
COMMENT ON TABLE nome_tabela IS 'comentário'
COMMENT ON COLUMN tabela.coluna IS 'comentário'
~~~

Tabelas com constraints:

- USER_CONSTRAINTS
- USER_CONS_COLUMNS

Listar tabelas disponíveis:

~~~sql
SELECT object_name FROM user_objects
WHERE object_type = 'TABLE';
~~~

Direitos de acesso:

- ROLE_SYS_PRIVS
- ROLE_TAB_PRIVS
- USER_ROLE_PRIVS
- USER_SYS_PRIVS
- USER_TAB_PRIVS_MADE
- USER_TAB_PRIVS_RECD
- USER_COL_PRIVS_MADE
- USER_COL_PRIVS_RECD



Controle de acesso
------------------

Criação de usuário:

~~~sql
CREATE USER nome_usuario IDENTIFIED BY senha_usuario
~~~

Trocar a senha:

~~~sql
ALTER USER nome_usuario IDENTIFIED BY nova_senha
~~~

Criação de um papel:

~~~sql
CREATE ROLE nome_papel
~~~

Atribuindo um papel a um usuário

~~~sql
GRANT nome_papel TO nome_usuario
~~~

Concedendo direitos de acesso:

~~~sql
GRANT create table, create sequence, create view
TO nome_usuario
~~~

Direito de consulta:

~~~sql
GRANT select, ... TO nome_papel
~~~

Direito de atualizar colunas específicas:

~~~sql
GRANT update(coluna1, coluna2, ...)
ON tabela
TO usuario, papel, ...
~~~

Usuário pode repassar direitos:

~~~sql
GRANT select ON tabela TO usuario WITH GRANT OPTION
~~~

Dando direitos para todos:

~~~sql
GRANT select ON tabela TO PUBLIC
~~~

Remover direitos

~~~sql
REVOKE select ON tabela FROM usuario
~~~


Sinônimos:

~~~sql
CREATE SYNONYM xyz FOR usuario.objeto

CREATE SYNONYM abc FOR objeto

CREATE PUBLIC SYNONYM def FOR usuario.objeto

DROP SYNONYM xyz
~~~



Sequências
----------

Criando:

~~~sql
CREATE SEQUENCE nome_sequencia
INCREMENT BY 1   -- (valor para incrementar)
START WITH 51    -- (valor de início da seq)
MAXVALUE 9999999 -- (valor máximo a que deve chegar)
NOCACHE
NOCYCLE
~~~

Alterando:

~~~sql
ALTER SEQUENCE nome_sequencia
INCREMENT BY 4
MAXVALUE 99999
CYCLE
NOCACHE
~~~

Não é possível alterar o valor inicial, exceto se remover (DROP SEQUENCE nome_sequencia) e recriar a sequence.

Usando:

~~~sql
INSERT INTO funcionario(id, nome)
VALUES (seq_funcionario_id.NEXTVAL, 'JOAO')
~~~




Subqueries
----------

Valor único:

- Ex: Funcionários com o mesmo cargo do funcionário 'Silva'.

~~~sql
SELECT sobrenome, cargo
FROM funcionario
WHERE cargo = (SELECT cargo
               FROm funcionario
               WHERE sobrenome = 'Silva');
~~~

- Ex: Funcionários com salário abaixo da média:

~~~sql
SELECT sobrenome, cargo, salario
FROM funcionario
WHERE salario < (SELECT AVG(salario) FROM funcionario);
~~~

Múltiplos valores:

- Ex: Funcionários em departamentos em que a região é 2.

~~~sql
SELECT sobrenome, nome, cargo
FROM funcionario
WHERE depart_id IN (SELECT id FROM departamento
                    WHERE regiao = 2);
~~~

Usando operadores (ANY, ALL):

~~~sql
SELECT colunas
FROM tabela
WHERE coluna >= ALL (SELECT coluna FROM outra_tabela WHERE ...);
~~~

OU... (repare que a condição é basicamente a mesma, mas um caso 'pode' ser mais eficiente que o outro)

~~~sql
SELECT colunas
FROM tabela
WHERE NOT coluna < ANY (SELECT coluna FROM outra_tabela WHERE ...);
~~~

OBS: = ANY é equivalente a IN, enquanto != ALL é equivalente a NOT IN

- Ex: Médias de salários por departamento maiores que a média do departamento 32

~~~sql
SELECT depart_id, AVG(salario)
FROM funcionario
GROUP BY depart_id
HAVING AVG(salario) > (SELECT AVG(salario)
                       FROM funcionario
                       WHERE depart_id = 32);
~~~

Subqueries nomeadas:

O comando WITH pode ser usado para executar uma query antes da execução da query principal e que pode ser referenciada posteriormente.

~~~sql
WITH media_sal AS ( SELECT AVG(salario) media FROM funcionarios )
SELECT nome, cargo, salario
  FROM funcionarios
 WHERE salario > ( SELECT media FROM media_sal );
~~~

Subquerie correlacionada:

- Ex: Retorna clientes ativos que possuem compras de pelo menos 100 reais

~~~sql
SELECT nome
  FROM cliente c
 WHERE status = 'A'
   AND EXISTS ( SELECT 1
                  FROM pedidos
                 WHERE id_cliente = c.id_cliente
                   AND valor >= 100
              );
~~~

- Ex: Inativa clientes que não tenham feito pedidos no último ano

~~~sql
UPDATE cliente c
   SET status = 'I'
 WHERE NOT EXISTS ( SELECT 1
                      FROM pedidos
                     WHERE id_cliente = c.id_cliente
                       AND data > TRUNC(SYSDATE) - 365
                  );
~~~

- Ex: Atualiza a coluna ULTIMO_PAGTO da tabela cliente com o último pagamento realizado daquele cliente

~~~sql
UPDATE cliente c
   SET ultimo_pagto = ( SELECT MAX(data)
                          FROM pagamentos
                         WHERE id_cliente = c.id_cliente
                      );
~~~

- Ex: Conta o número de funcionários por departamento (não requer JOIN, GROUP BY, ordenação, etc)

~~~sql
SELECT id_depto,
       nome,
       ( SELECT COUNT(*)
           FROM funcionarios
          WHERE id_depto = d.id_depto
       ) nro_funcionarios
  FROM departamentos d;
~~~

Equivalente ao anterior, sem subquery

~~~sql
SELECT d.id_depto,
       d.nome,
       COUNT(*)
  FROM departamentos d,
       funcionarios  f
 WHERE d.id_depto = f.id_depto (+)
 GROUP BY d.id_depto, d.nome ;
~~~

## Inline views/Pre-query

Equivalente aos anteriores.

~~~sql
SELECT d.id_depto,
       d.nome,
       f.qtde
  FROM departamentos d,
       ( SELECT id_depto,
                COUNT(*) qtde
           FROM funcionarios
          GROUP BY id_depto
       ) f
 WHERE d.id_depto = f.id_depto (+);
~~~



## Queries hierárquicas (CONNECT BY)

Neste exemplo a coluna que faz a conexão (id_ccg_mv_grupo) faz referência circular ao próprio elemento "pai".
Por isso foi necessário um case para forçar nulo quando for igual para evitar o "loop".

~~~sql
SELECT level, i.descricao, x.*
  from ( select id_ccg_movto     ,
                id_ccg_conta     ,
                id_ccg_item      ,
                data_procto      ,
                valor            ,
                id_ccg_operacao  ,
                id_ccg_op_evento ,
                ( case when id_ccg_movto = id_ccg_mv_grupo then null else id_ccg_mv_grupo end ) as id_ccg_mv_grupo
           from ccg_movto
       ) x,
       ccg_item i
 where x.id_ccg_item = i.id_ccg_item
 start with id_ccg_movto = 1411
 connect by prior id_ccg_movto = id_ccg_mv_grupo;
~~~



## "Fabricação" de datasets

Um dataset que retorna de 0 a 3

~~~sql
SELECT 0 x FROM DUAL UNION ALL
SELECT 1 x FROM DUAL UNION ALL
SELECT 2 x FROM DUAL UNION ALL
SELECT 3 x FROM DUAL;
~~~

Utiliza um dataset temporário para classificação de um produto

~~~sql
SELECT p.id_produto,
       p.nome,
       t.descricao classificacao
  FROM produtos p,
       ( SELECT 'Pequeno' descricao,  0 tamanho_min,   20 tamanho_max UNION ALL
         SELECT 'Médio'   descricao, 20 tamanho_min,   50 tamanho_max UNION ALL
         SELECT 'Grande'  descricao, 50 tamanho_min, 9999 tamanho_max
       ) t
 WHERE p.tamanho >= t.tamanho_min  -- Atenção para o uso de >= e < no lugar de BETWEEN para evitar casos
   AND p.tamanho  < t.tamanho_max; -- de valores em um intervalo que poderiam ficar perdidos. Por exemplo: 20.5
~~~

## Geração de linhas:

### Oracle 10g em diante

Gerar uma sequência de números

~~~sql
SELECT level from dual connect by level <= 10;
~~~

Gerar uma sequência de números de A até B (inclusive)

~~~sql
SELECT A + level - 1 from dual connect by level <= B - A + 1;
~~~

Uma sequência de dias

~~~sql
SELECT trunc(sysdate) + level - 1 from dual connect by level <= 10;
~~~

Uma sequência de meses

~~~sql
SELECT add_months(trunc(sysdate, 'mm'), level - 1) from dual connect by level <= 12;
~~~

Filtrando os números gerados

~~~sql
SELECT num from (select level as num from dual connect by level <= 10) where mod(num, 2) = 0;
~~~

Gerando uma sequência de números aleatórios

~~~sql
SELECT trunc(dbms_random.value(low => 1, high => 100)) as rnd from dual connect by level <= 10;
~~~

Todos os dias de um ano (considera anos bissextos)

~~~sql
SELECT to_date('01/01/2014') + level - 1 as dia from dual connect by to_date('01/01/2014') + level - 1 <= to_date('31/12/2014');
~~~

Usando xmltable

~~~sql
SELECT to_number(column_value) num from xmltable('for $i in 1 to 10 return $i');
~~~

### Versões antigas (Anterior a 9i)

Onde CUBE irá produzir uma combinação de todos os elementos. Por exemplo: GROUP BY CUBE(1, 2) irá produzir 11,12,21,22 e portanto 4 linhas. CUBE(1,2,3) irá produzir 8 linhas, CUBE(1,2,3,4) irá produzir 16 linhas, etc.

~~~sql
SELECT rownum from ( select 1 from dual group by cube(1,2,3,4,5,6,7,8,9) ) where rownum <= 365;
~~~

### Somente 11g (usando query recursiva)

~~~sql
with recurse_num(num) as (
  select 1 as num from dual
  union all
  select num+1 as num from recurse_num where num < 5
)
SELECT num from recurse_num;
~~~



## Operações de conjunto

- `UNION` - Retorna apenas as linhas únicas da união de dois datasets (possui um DISTINCT implícito).
- `UNION ALL` - Retorna todas as linhas da união de dois datasets, mesmo que haja duplicações. (A performance é melhor do que UNION pois não há necessidade de ordenação interna, comparação, distinct, etc.)
- `MINUS` - Retorna somente as linhas do primeiro dataset que não aparecem no segundo. O segundo dataset pode retornar mais linhas que o primeiro, isto não afeta o resultado.
- `INTERSECT` - Retorna somente as linhas que aparecem nos dois datasets.

Quando mais de uma operação é utilizada, elas são aplicadas na ordem em que aparecem. Para forçar uma ordem é possível usar parênteses.

Em operações de conjunto, se as colunas forem renomeadas o nome das colunas no primeiro select terá precedência. Por exemplo:

~~~sql
SELECT 123 ColunaA, 456 ColunaB FROM DUAL
UNION ALL
SELECT 789 ColunaC, 999 ColunaD FROM DUAL;
~~~

Neste exemplo, as colunas retornadas serão ColunaA e ColunaB, enquanto os nomes do segundo select serão desconsiderados.

Se for feita ordenação, a cláusula ORDER BY deve vir por último e será aplicada ao resultado geral. Exemplo:

~~~sql
SELECT a, b, c FROM tabela1
UNION ALL
SELECT d, e, f FROM tabela2
UNION ALL
SELECT g, h, i FROM tabela3
ORDER BY b; -- O nome da coluna deve ser o mesmo do primeiro select ou para evitar confusão, usar a posição (neste caso 2)
~~~



## Tempo e intervalos:

Tipos para data e hora com precisão de milisegundos:

    TIMESTAMP
    TIMESTAMP WITH TIME ZONE
    TIMESTAMP WITH LOCAL TIME ZONE

Tipo para intervalos de anos com precisão de meses (precisão entre parênteses):

    INTERVAL YEAR(2) TO MONTH

Tipo para intervalos de dias com precisão de segundos:

    INTERVAL DAY(3) TO SECOND(6)

Criar um valor de intervalo:

    NUMTOYMINTERVAL(2,'YEAR')
    NUMTOYMINTERVAL(2.5,'MONTH')
    NUMTODSINTERVAL(5369.2589,'SECOND')
    TO_YMINTERVAL('02-04')
    TO_DSINTERVAL('0 2:30:43')

Literais de data e hora:

~~~sql
-- É equivalente a usar TO_DATE('31/01/2012', 'DD/MM/YYYY')
-- O formato é fixo 'YYYY-MM-DD'.
SELECT DATE '2012-01-31' FROM DUAL;
v_data := DATE '2012-01-31';

-- É possível usar hora também: TIME 'HH:MI:SS'
SELECT TIME '23:59:59' FROM DUAL;

-- Timestamp: 'YYYY-MM-DD HH:MI:SS.xxxxxxxxx
SELECT TIMESTAMP '1998-12-31 08:23:46.368912341' FROM DUAL;

-- Timestamp: 'YYYY-MM-DD HH:MI:SS.xxxxxxxxx {+|-} HH:MI'
SELECT TIMESTAMP '1998-12-31 08:23:46.368912341 -03:00' FROM DUAL;

-- Intervalos: INTERVAL 'YY-MM' YEAR TO MONTH
SELECT INTERVAL '5-2' YEAR TO MONTH FROM DUAL;

-- Intervalos: INTERVAL 'd [h[:m[:s]]]' unit1[(precision1)] TO unit2[(frac_precision)]
SELECT INTERVAL '0 3:16:23.45' DAY TO SECOND FROM DUAL;
~~~



Manipulação de ordenação
------------------------

Para verificar a forma como os resultados são ordenados:

~~~sql
SELECT * from nls_session_parameters where parameter='NLS_SORT';

PARAMETER                      VALUE
------------------------------ ----------------------------------------
NLS_SORT                       WEST_EUROPEAN
~~~

Para forçar a ordenação de forma que números venham antes de caracteres.

~~~sql
ALTER session set nls_sort=binary;
~~~

Para forçar uma ordem qualquer:

~~~sql
ALTER session set nls_sort=french;
~~~

Para forçar uma ordenação independente da variável de ambiente:

~~~sql
SELECT * from loja where codmloja like '%713' order by NLSSORT(codmloja,'NLS_SORT=BINARY_AI');
~~~



Funções analíticas (analytics)
------------------------------

Cláusula OVER ()
----------------

Referências:
- <http://www.orafaq.com/node/55>
- <http://stackoverflow.com/questions/1092120/over-clause-in-oracle>


Trazendo o número total de linhas em cada linha da query

~~~sql
SELECT id_arq_conteudo,
       ROWNUM AS linha,
       COUNT(*) OVER() AS tot_linhas
  FROM arq_conteudo;
~~~

Acumulando um valor

~~~sql
SELECT dataven,
       valorpar,
       SUM(valorpar) OVER(ORDER BY dataven ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS valorpar_acumulado
  FROM parcela
 WHERE id_contrato = 'T36174037'
 ORDER BY dataven;
~~~

Particionando valores

~~~sql
SELECT id_contrato,
       datacomp,
       TO_CHAR(datacomp, 'Mon/YYYY') AS mes,
       valorcomp,
       SUM(valorcomp) OVER (PARTITION BY TRUNC(datacomp, 'MM')) AS valor_mensal,
       TO_CHAR(valorcomp / SUM(valorcomp) OVER (PARTITION BY TRUNC(datacomp, 'MM')) * 100, '999G990D00') || '%' AS perc_mensal
  FROM contrato
 WHERE ciccli = '18066560869'
 ORDER BY datacomp;
~~~

Calculando a média de um valor considerando uma linha antes e uma depois

~~~sql
SELECT mes,
       compras,
       valor,
       ROUND(AVG(compras) OVER (ORDER BY mes ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING ), 2) AS compras_media_3m,
       ROUND(AVG(valor)   OVER (ORDER BY mes ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING ), 2) AS valor_media_3m
  FROM (
        SELECT TRUNC(datacomp, 'MM') as mes,
               COUNT(*) AS compras,
               SUM(valorcomp) AS valor
          FROM contrato
         WHERE ciccli = '18066560869'
         GROUP BY TRUNC(datacomp, 'MM')
       )
 ORDER BY mes;
~~~

Calculando o mínimo, máximo, média e valor acumulado ao longo do tempo

~~~sql
SELECT datacomp,
       valorcomp,
       MIN(valorcomp) OVER (ORDER BY datacomp) AS menor_compra_ate_data,
       MAX(valorcomp) OVER (ORDER BY datacomp) AS maior_compra_ate_data,
       AVG(valorcomp) OVER (ORDER BY datacomp) AS media_ate_data,
       SUM(valorcomp) OVER (ORDER BY datacomp) AS total_comprado_ate_data
  FROM contrato
 WHERE ciccli = '18066560869'
 ORDER BY datacomp;
~~~

Número sequencial (ROW_NUMBER) de um registro dentro de uma partição

~~~sql
SELECT ROWNUM AS linha,
       ROW_NUMBER() OVER (PARTITION BY id_arq_tipo ORDER BY id_arq_conteudo) AS linha_tipo,
       id_arq_tipo,
       id_arq_conteudo
  FROM arq_conteudo
 ORDER BY id_arq_tipo, id_arq_conteudo;
~~~

Cláusula LISTAGG () WITHIN GROUP ()
-----------------------------------

~~~sql
SELECT listagg('''' || documento || '''', ', ') within group (order by documento)
  from (
        select distinct p.documento
          from crb_parcela p, crb_contrato t, crb_sacado s
         where p.id_crb_contrato = t.id_crb_contrato
           and t.id_crb_sacado = s.id_crb_sacado
           and s.identificacao between '30000000000' and '39999999999'
           and p.vencto_original < trunc(sysdate)
           and t.id_crb_cessao = 267
       )
 where rownum <= 35;
~~~



Performance
-----------

Algumas vezes, o Oracle não faz uso do índice correto, por conta de estatísticas desatualizadas. É possível forçar a geração de novas estatísticas para uma tabela com o comando ANALYZE TABLE:

~~~sql
ANALYZE TABLE nome_tabela COMPUTE STATISTICS;
~~~

Se a tabela for muito grande, é possível calcular uma estimativa com base numa parte da tabela apenas, seja em percentual ou número de linhas:

~~~sql
ANALYZE TABLE nome_tabela ESTIMATE STATISTICS SAMPLE 10 PERCENT;
-- ou --
ANALYZE TABLE nome_tabela ESTIMATE STATISTICS SAMPLE 500 ROWS;
~~~



# PL/SQL


Atribuição é `:=`

~~~sql
-- Comentário de uma linha começa com '--'

/* Comentários de múltiplas linhas utilizam
   os símbolos / e * para delimitar o início e
   os símbolos * e / para delimitar o final.
 */
~~~

Condição

~~~sql
IF condicao THEN
   abc;
ELSIF condicao THEN
   def;
ELSE
   ghi;
END IF;
~~~

Case PL/SQL

~~~sql
CASE valor
  WHEN 1 THEN
    acao1a;
    acao1b;
  WHEN 2 THEN
    acao2;
  ELSE
    acao_padrao1;
    acao_padrao2;
    acao_padrao3;
END CASE;
~~~

Loops

~~~sql
LOOP
  abc;
  EXIT WHEN condicao; -- pode ser feito em todos os tipos de loop
  def;
  IF condicao THEN
     ...
     ...
     EXIT; -- sai do loop diretamente
  END IF;
  ghi;
END LOOP;

WHILE condicao LOOP
  acao1;
  acao2;
  acao3;
END LOOP;
~~~

- Os contadores/cursores dos loops FOR não devem ser declarados
- O contador só é válido dentro do loop e não deve ser alterado
- Pode ser utilizada a palavra chave REVERSE para invertir a ordem da contagem
- Os limites inferiores não devem ser invertidos neste caso (Não usar 100..1, por exemplo)

~~~sql
FOR v_count IN [REVERSE] 1..100 LOOP
  bloco;
END LOOP;

FOR r_registro IN cs_cursor LOOP
  bloco;
END LOOP;

FOR reg IN (
  SELECT ...
    FROM ...
   WHERE ...
)
LOOP
  DBMS_OUTPUT.PUT_LINE(reg.campo1 || ' ' || reg.campo2);
END LOOP;
~~~

Cursor com bloqueio de linha e atualização da linha atual do cursor

~~~sql
declare
  cursor cur_exemplo is
  select * from cliente
  FOR UPDATE; /* <--- */
BEGIN
  for reg in cur_exemplo loop
    update cliente
       set nome = upper(nome)
     WHERE CURRENT OF CUR_EXEMPLO; /* <--- */
  end loop;
end;
/
~~~


## Bloco anônimo

~~~sql
DECLARE
  -- declarações de variáveis
BEGIN
  -- bloco de comandos
END;
/
~~~

## Exibição de dados

~~~sql
DBMP_OUTPUT.PUT_LINE(...) -- Exibe um valor na saída do SQL*PLUS
~~~

Obs: Usar `SET SERVEROUTPUT ON FORMAT WRAPPED;` se rodar no SQL Developer.

## Variáveis e constantes

Exemplos de declaração de variáveis e constantes e uso:

~~~sql
DECLARE

  v_data   DATE;
  v_hoje   DATE := TRUNC(SYSDATE);
  v_amanha DATE := v_hoje + 1;

  v_num1   NUMBER;
  v_num2   NUMBER(10,6) := 123.345;

  c_pi     CONSTANT NUMBER := 3.1415926;
  c_pi2    CONSTANT NUMBER := c_pi * 2; -- Expressões são permitidas se um valor for atribuído a uma constante anteriormente

  v_texto  VARCHAR2(10) NOT NULL := 'teste'; -- Variáveis NOT NULL devem ser inicializadas

BEGIN

  v_data := TO_DATE('01/01/2012', 'DD/MM/YYYY');

  DBMS_OUTPUT.PUT_LINE('v_data.....: ' || v_data);
  DBMS_OUTPUT.PUT_LINE('v_hoje.....: ' || v_hoje);
  DBMS_OUTPUT.PUT_LINE('v_amanha...: ' || v_amanha);
  DBMS_OUTPUT.PUT_LINE('v_num1.....: ' || v_num1); -- valor nulo!
  DBMS_OUTPUT.PUT_LINE('v_num2.....: ' || v_num2);
  DBMS_OUTPUT.PUT_LINE('c_pi.......: ' || c_pi);
  DBMS_OUTPUT.PUT_LINE('c_pi2......: ' || c_pi2);
  DBMS_OUTPUT.PUT_LINE('v_texto....: ' || v_texto);

END;
/
~~~

## Cursores

Controlando um cursor explícito

~~~sql
DECLARE
  CURSOR nome_cursor IS consulta;
  var_registro nome_cursor%ROWTYPE;

  v_coluna1  NUMBER(10);
  v_coluna2  VARCHAR2(20);

BEGIN
  OPEN nome_cursor;
  LOOP
    FETCH nome_cursor INTO var_registro; -- Recupera um registro de cada vez
    EXIT WHEN nome_cursor%NOTFOUND; -- Testa se chegou no final
    ...
    /* Executa código com dados em var_registro
       acessíveis com var_registro.campo */
    ...
  END LOOP;
  CLOSE nome_cursor;

  OPEN nome_cursor;
  FETCH nome_cursor INTO v_coluna1, v_coluna 2; -- É possível extrair dados para variáveis individuais
  CLOSE nome_cursor;
END;
~~~

Atributos de um cursor:

    %ISOPEN    Cursor está aberto.
    %NOTFOUND  Comando FETCH não retornou uma linha.
    %FOUND     Comando FETCH retornou uma linha.
    %ROWCOUNT  Número de linhas retornadas até o momento

O cursor implícito "SQL" também possui estes atributos, mas ISOPEN é sempre falso.

> **Atenção:** COMMIT e ROLLBACK zeram o valor de SQL%ROWCOUNT!!!

Cursores com parâmetros

~~~sql
DECLARE

  CURSOR cs (p_num NUMBER, p_txt VARCHAR2)
  IS
  SELECT col1, col2
    FROM tab
   WHERE col1 > p_num
     AND UPPER(col2) = UPPER(p_txt);

BEGIN
  FOR r1 IN cs(10, 'ABC') LOOP
     ...
  END LOOP;
END;
/
~~~

Cursores para atualização (bloqueiam a linha)

~~~sql
DECLARE
  CURSOR cs IS
  SELECT ...
  FOR UPDATE [OF colunas...] -- define quais colunas serão bloqueadas para alteração
  [NOWAIT]; -- não espera, retorna um erro se a linha estiver bloqueada

BEGIN
  FOR r IN cs LOOP
    UPDATE tabela
       SET coluna = valor
     WHERE CURRENT OF cs; -- atualiza o registro atual apontado pelo cursor (deve ser usado com FOR UPDATE)
  END LOOP;
END;
/
~~~

## Registros

~~~sql
DECLARE
  r_emp    hr.employees%ROWTYPE; -- Declaração de um registro de uma tabela
  v_avgsal hr.employees.salary%TYPE -- Tipo de um campo

BEGIN

  -- Recuperando um registro
  SELECT * INTO r_emp FROM hr.employees WHERE employee_id = 100;
  -- Utilizando os campos
  DBMS_OUTPUT.PUT_LINE('Name: ' || r_emp.first_name || ' ' || r_emp.last_name);

  SELECT AVG(salary) INTO v_avgsal FROM hr.employees;
  DBMS_OUTPUT.PUT_LINE('Average Salary: ' || v_avgsal);

END;
/
~~~

## Função

~~~sql
CREATE OR REPLACE FUNCTION media_pond
(nota1 IN NUMBER, peso1 IN NUMBER,
 nota2 IN NUMBER, peso2 IN NUMBER)
RETURN NUMBER
IS
  mp NUMBER;
BEGIN
  mp := (nota1*peso1 + nota2*peso2) / (peso1 + peso2);
  RETURN mp;
END;
~~~

Exemplos de chamadas de função:

~~~sql
UPDATE tab_mp SET mp = media_pond(n1, p1, n2, p2);

SELECT fn_exemplo(123) FROM dual;

v_resultado := fn_exemplo(456);
~~~

## Procedimento

~~~sql
CREATE OR REPLACE PROCEDURE nome_procedure
(param_entrada     IN NUMBER,   -- Parâmetro de entrada
 param_saida      OUT VARCHAR2, -- Parâmetro de saída (deve ser uma variável na chamada)
 param_entsai  IN OUT DATE,     -- Parâmetro de entrada e saída
 param_padrao      IN DATE DEFAULT SYSDATE) -- Parâmetro que recebe valor padrão se não for passado
IS
  variaveis
BEGIN
  codigo
END nome_procedure; -- É opcional repetir o nome do procedimento no final
~~~

Exemplo de chamada de procedimento a partir do prompt do SQL Plus/Developer

~~~sql
EXEC nome_procedure(...);
~~~

Procedimento com parâmetros opcionais nomeados

~~~sql
CREATE OR REPLACE PROCEDURE teste_opcional
(p_alpha IN NUMBER DEFAULT NULL,
 p_beta  IN NUMBER DEFAULT NULL,
 p_gamma IN NUMBER DEFAULT NULL,
 p_delta IN NUMBER DEFAULT NULL)
IS
BEGIN
  ...
END;
~~~

Chamada com parâmetros nomeados:

~~~sql
teste_opcional(p_gamma => 123, p_alpha => 456);
~~~

## Subprogramas

~~~sql
DECLARE -- Neste exemplo é usado um bloco anônimo mas é válido em Procedures e Functions também
  ...

  FUNCTION abc (x IN NUMBER) RETURN NUMBER -- Função interna a este bloco/procedimento/função
  IS
  BEGIN
    ...
  END;

  PROCEDURE def (y OUT VARCHAR2) -- Procedimento interno a este bloco/procedimento/função
  IS
  BEGIN
    ...
  END;

BEGIN
  ...
END;
/
~~~

## Arrays e tabelas

~~~sql
declare

  type TabValues is table of varchar2(20) index by binary_integer; -- indexado por números inteiros
  l_tab_values TabValues;

  TYPE population_type IS TABLE OF NUMBER INDEX BY VARCHAR2(64); -- indexado por chave alfanumérica
  country_population population_type;
  continent_population population_type;

  j number;

BEGIN
  l_tab_values(1) := 'A';
  l_tab_values(2) := 'B';
  l_tab_values(3) := 'C';
  l_tab_values(5) := 'E';

  DBMS_OUTPUT.PUT_LINE('Acessando por número');
  for i in 1..3 loop
    DBMS_OUTPUT.PUT_LINE(i || ' => ' || l_tab_values(i));
  end loop;

  DBMS_OUTPUT.PUT_LINE('Acessando em sequência numérica');
  for i in l_tab_values.FIRST..l_tab_values.LAST loop
    begin
      DBMS_OUTPUT.PUT_LINE(i || ' => ' || l_tab_values(i));
    exception
      when others then
        DBMS_OUTPUT.PUT_LINE(sqlcode || ':' || sqlerrm);
    end;
  end loop;

  DBMS_OUTPUT.PUT_LINE('Tabela/array esparso');
  j := l_tab_values.FIRST;
  while j is not null loop
    DBMS_OUTPUT.PUT_LINE(j || ' => ' || l_tab_values(j));
    j := l_tab_values.NEXT(j);
  end loop;

  DBMS_OUTPUT.PUT_LINE('Tabela/array esparso em ordem reversa');
  j := l_tab_values.LAST;
  while j is not null loop
    DBMS_OUTPUT.PUT_LINE(j || ' => ' || l_tab_values(j));
    j := l_tab_values.PRIOR(j);
  end loop;

end;
/
~~~

Extraindo de uma tabela/query para um array

~~~sql
DECLARE
  TYPE EmpTabTyp IS TABLE OF employees%ROWTYPE INDEX BY PLS_INTEGER;
  emp_tab EmpTabTyp;

  TYPE varray_type IS VARRAY(5) OF INTEGER; -- Array com 5 elementos fixos

BEGIN
  /* Retrieve employee record. */
  SELECT * INTO emp_tab(100) FROM employees WHERE employee_id = 100;
END;
/
~~~

## Tabela de registros na memória

~~~sql
declare
  type RecDados is record (
    name        varchar2(64),
    time_waited number,
    total_waits number);

  type TabValues is table of RecDados index by binary_integer;

  l_tab_values TabValues;
  l_cnt        pls_integer;

BEGIN

  l_cnt := 1;
  for lst in (select * from v$system_event order by event) loop
    l_tab_values(l_cnt).name := lst.event;
    l_tab_values(l_cnt).time_waited := lst.time_waited;
    l_tab_values(l_cnt).total_waits := lst.total_waits;
    l_cnt := l_cnt + 1;
  end loop;

end;
/
~~~

## Pausa na execução de um script/bloco

~~~sql
exec dbms_lock.sleep(10);
~~~



## Tratando erros:

Executar ação quando comando não retornou resultados.

### Para UPDATE/INSERT:

~~~sql
UPDATE tabela SET coluna = valor WHERE condicao;

IF SQL%ROWCOUNT = 0 THEN
  ...
END IF;
~~~


### Para SELECT INTO:

~~~sql
BEGIN
  SELECT coluna INTO v_variavel FROM tabela WHERE condicao;
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    ...
END;
~~~

### Para DELETE:

~~~sql
DELETE tabela WHERE condicao;

IF SQL%NOTFOUND = 0 THEN
  ...
END IF;
~~~


## Tratamento de exceptions

~~~sql
BEGIN
  ...
EXCEPTION
  WHEN excecao1 THEN -- Trata uma exceção específica
    ...
  WHEN excecao2 OR excecao3 THEN -- É possível tratar mais de um caso na mesma cláusula
    ...
  WHEN OTHERS THEN -- Trata todas as outras exceções não especificadas (deve ser a última cláusula)
    ...
END;
~~~

Funções:

    SQLCODE   Retorna o código do erro ocorrido.
    SQLERRM   Retorna a mensagem de erro.

Exceções padrão do Oracle:

Exceção                  | Descrição
-------------------------|-----------------------------
NO_DATA_FOUND            | SELECT não retornou dados.
TOO_MANY_ROWS            | SELECT retornou mais de uma linha, quando era esperada uma só (SELECT INTO).
INVALID_CURSOR           | Operação ilegal de cursor.
ZERO_DIVIDE              | Tentativa de divisão por zero.
DUP_VAL_ON_INDEX         | Inserção de valor duplicado.
INVALID_NUMBER           | Erro na conversão de string para número.
VALUE_ERROR              | Erro de conversão, truncamento ou restrição de tamanho.
ACCESS_INTO_NULL         | Atribuição de valores a objeto não inicializado.
COLLECTION_IS_NULL       | Tentativa de aplicação de método diferente de EXISTS em varray ou tabela aninhada não inicializada.
CURSOR_ALREADY_OPEN      | Tentativa de abrir cursor já aberto.
LOGIN_DENIED             | Nome de usuário ou senha inválido.
NOT_LOGGED_ON            | Tentativa de executar operação sem estar conectado.
PROGRAM_ERROR            | Problema interno no código PL/SQL.
ROWTYPE_MISMATCH         | Variável de cursor de host e de cursor PL/SQL tem tipos incompatíveis.
STORAGE_ERROR            | Memória esgotada ou corrompida.
SUBSCRIPT_BEYOND_COUNT   | Tentativa de acesso a elemento de varray ou tabela aninhada além do tamanho.
SUBSCRIPT_OUTSIDE_LIMIT  | Tentativa de acesso a elemento de varray ou tabela aninhada fora da faixa válida.
TIMEOUT_ON_RESOURCE      | Ocorreu timeout enquanto o Oracle aguardava um recurso.


Definindo exceções do Oracle ou novas:

~~~sql
SET SERVEROUTPUT ON

DECLARE
  dia_invalido     EXCEPTION;
  mes_invalido     EXCEPTION;
  ano_invalido     EXCEPTION;
  hora_invalida    EXCEPTION;
  minuto_invalido  EXCEPTION;
  segundo_invalido EXCEPTION;
  data_invalida    EXCEPTION;
  erro_exemplo     EXCEPTION;
  PRAGMA EXCEPTION_INIT(dia_invalido, -1847);
  PRAGMA EXCEPTION_INIT(mes_invalido, -1843);
  PRAGMA EXCEPTION_INIT(ano_invalido, -1841);
  PRAGMA EXCEPTION_INIT(hora_invalida, -1850);
  PRAGMA EXCEPTION_INIT(minuto_invalido, -1851);
  PRAGMA EXCEPTION_INIT(segundo_invalido, -1852);
  PRAGMA EXCEPTION_INIT(data_invalida, -1839);
  v_data DATE;

BEGIN

  BEGIN
    v_data := TO_DATE('32/01/2012', 'DD/MM/YYYY');
  EXCEPTION
    WHEN dia_invalido THEN
      DBMS_OUTPUT.PUT_LINE('Dia inválido.');
  END;

  BEGIN
    v_data := TO_DATE('30/13/2012', 'DD/MM/YYYY');
  EXCEPTION
    WHEN mes_invalido THEN
      DBMS_OUTPUT.PUT_LINE('Mês inválido.');
  END;

  BEGIN
    v_data := TO_DATE('10/01/0000', 'DD/MM/YYYY');
  EXCEPTION
    WHEN ano_invalido THEN
      DBMS_OUTPUT.PUT_LINE('Ano inválido.');
  END;

  BEGIN
    v_data := to_date('25:00:00', 'HH24:MI:SS');
  EXCEPTION
    WHEN hora_invalida THEN
      DBMS_OUTPUT.PUT_LINE('Hora inválida.');
  END;

  BEGIN
    v_data := TO_DATE('23:60:00', 'HH24:MI:SS');
  EXCEPTION
    WHEN minuto_invalido THEN
      DBMS_OUTPUT.PUT_LINE('Minuto inválido.');
  END;

  begin
    v_data := TO_DATE('23:00:60', 'HH24:MI:SS');
  EXCEPTION
    WHEN segundo_invalido THEN
      DBMS_OUTPUT.PUT_LINE('Segundo inválido.');
  END;

  BEGIN
    v_data := to_date('29/02/2011', 'DD/MM/YYYY');
  EXCEPTION
    WHEN data_invalida THEN
      DBMS_OUTPUT.PUT_LINE('Data inválido.');
  END;

  BEGIN
    RAISE erro_exemplo;
    -- SQLCODE: 1
    -- SQLERRM: User-Defined Exception
  EXCEPTION
    WHEN erro_exemplo THEN
      DBMS_OUTPUT.PUT_LINE('Erro de exemplo.');
  END;

  RAISE erro_exemplo;

EXCEPTION
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('SQLCODE: ' || SQLCODE);
    DBMS_OUTPUT.PUT_LINE('SQLERRM: ' || SQLERRM);
END;
/
~~~

Gerando erro de aplicação em procedimentos armazenados:

~~~sql
-- Deve ser de -20999 a -20000
BEGIN
  RAISE_APPLICATION_ERROR(-20201, 'Mensagem de erro.');
END;
/
~~~



# Scripts SQL*Plus:

## Uso do &

Colocar &nome_variavel faz com que o valor para nome_variavel seja solicitado ao usuário na execução do comando/script. O conteúdo da variável será substituído literalmente no comando, sendo necessário usar aspas ('') para strings, por exemplo. Mesmo condições podem ser seubstituídas. 
Exemplo:

~~~sql
SELECT * FROM tabela WHERE &condicao
~~~

Alternar entre exibição do comando SQL antes/após substituição de variáveis.

~~~sql
SET VERIFY ON/OFF
~~~

Exibe/oculta comando SQL na execução de script.

~~~sql
SET ECHO ON/OFF
~~~

Recebe valor para a varíavel indicada fornecido pelo usuário.

~~~sql
ACCEPT &nome_variavel
~~~

Recebe um valor em um formato específico.

~~~sql
ACCEPT &data DATE FORMAT 'DD/MM/YYYY' -
PROMPT 'Entre com a data (DD/MM/AAAA) :'
~~~

Recebe um valor em um formato específico com valor padrão.

~~~sql
ACCEPT vv_number NUMBER FORMAT 990.00 DEFAULT -1 PROMPT "Type a number [-1]: "
~~~

Exibe uma string literal para o usuário.

~~~sql
PROMPT 'literal'
~~~

Remove definição da variável

~~~sql
UNDEFINE nome_variavel
~~~

Cria uma definição de variável. OBS: Estas variáveis são do ambiente SQL Plus / SQL Developer e são substituídas no comando antes de sua execução.

~~~sql
DEFINE nome_variavel = valor_variavel
~~~

Executa um script.

~~~sql
START caminho\script.sql
-- ou --
@ caminho\script.sql
~~~

O diretório padrão em que o SQL*Plus busca os scripts está definido na variável de ambiente do sistema SQLPATH. É possível ter vários diretórios configurados nesta variável separados por ;
No SQL Developer é possível definir o caminho padrão em Ferramentas > Preferências > Banco de dados > Planilha > Selecionar caminho default para procurar scripts.

Formata a exibição de uma coluna na saída de um comando de consulta com no máximo 30 caracteres alfanuméricos.

~~~sql
COLUMN nome_da_coluna FORMAT A30 HEADING 'Nome da Coluna'
~~~

Formata coluna para exibição com casas decimais e separador de milhar.

~~~sql
COLUMN nome_da_coluna FORMAT $99,999.99 HEADING 'Nome da Coluna'
~~~

Limpa formatação da coluna.

~~~sql
COLUMN nome_da_coluna CLEAR
~~~

Habilita o uso do DBMS_OUTPUT no SQL*Plus e Oracle SQL Developer:

~~~sql
SET SERVEROUTPUT ON FORMAT WRAPPED
~~~

Faz uma quebra pela alteração do valor em determinada coluna. Resultado deve estar ordenado pela coluna para funcionar corretamente.

~~~sql
BREAK ON nome_da_coluna
BREAK ON nome_da_coluna SKIP n
BREAK ON nome_da_coluna SKIP PAGE
BREAK ON nome_da_coluna SKIP PAGE ON outra_coluna SKIP 1
~~~

Remover quebras:

~~~sql
CLEAR BREAKS
~~~

Computar totais:

~~~sql
COMPUTE funcao LABEL rotulo OF nome_da_coluna outra_coluna ... ON coluna_de_quebra
-- onde funcao = SUM, MINIMUM, MAXIMUM, AVG, SID, VARIANCE, COUNT, MEMBER
~~~

Exemplo:

~~~sql
COMPUTE SUM OF salary ON department_id
~~~

Mais informações: <http://docs.oracle.com/cd/B19306_01/server.102/b14357/ch6.htm>

Também é possível computar para todas as linhas ex:

~~~sql
COMPUTE SUM OF valor ON REPORT
~~~

Remover totais:

~~~sql
CLEAR COMPUTES
~~~



## Variáveis do SQL*PLUS

Uso: 

~~~sql
VAR[IABLE] [ <variable> [ NUMBER | CHAR | CHAR (n [CHAR|BYTE]) |
                    VARCHAR2 (n [CHAR|BYTE]) | NCHAR | NCHAR (n) |
                    NVARCHAR2 (n) | CLOB | NCLOB | BLOB | BFILE
                    REFCURSOR | BINARY_FLOAT | BINARY_DOUBLE ] ]
~~~

Definir e usar uma variável:

~~~sql
VARIABLE hoje VARCHAR2(100)
EXECUTE SELECT SYSDATE INTO :hoje FROM DUAL;
PRINT hoje
~~~



Grava as configurações atuais do SQL*PLUS:

~~~sql
STORE SET original_settings REPLACE
~~~

Restaura as configurações gravadas por STORE SET:

~~~sql
@original_settings
~~~



## Listar comandos do SQL*PLUS

~~~sql
help index

Enter Help [topic] for help.

 @             COPY         PAUSE                    SHUTDOWN
 @@            DEFINE       PRINT                    SPOOL
 /             DEL          PROMPT                   SQLPLUS
 ACCEPT        DESCRIBE     QUIT                     START
 APPEND        DISCONNECT   RECOVER                  STARTUP
 ARCHIVE LOG   EDIT         REMARK                   STORE
 ATTRIBUTE     EXECUTE      REPFOOTER                TIMING
 BREAK         EXIT         REPHEADER                TTITLE
 BTITLE        GET          RESERVED WORDS (SQL)     UNDEFINE
 CHANGE        HELP         RESERVED WORDS (PL/SQL)  VARIABLE
 CLEAR         HOST         RUN                      WHENEVER OSERROR
 COLUMN        INPUT        SAVE                     WHENEVER SQLERROR
 COMPUTE       LIST         SET                      XQUERY
 CONNECT       PASSWORD     SHOW
~~~

Comando     | Descrição
------------|-------------------
@           | Executa script no diretório atual ou que esteja em um dos caminhos descritos na variável de ambiente SQLPATH (pode haver mais de um separado por ;)
@@          | Executa script que esteja no mesmo diretório que o script atual (aceita caminhos relativos)
/           | Executa o buffer atual
ACCEPT      | Recebe valor interativamente do usuário
APPEND      | Adiciona texto ao final da linha atual do buffer. Ex: A ORDER BY 1
ARCHIVE LOG | Mostra informações do REDO LOG (DBA)
ATTRIBUTE   | Formatação de colunas de um objeto (TYPE)
BREAK       | Define quebras para formatação de relatório. Ex: BREAK ON tipo SKIP 1 DUPLICATES
BTITLE      | Define texto a ser impresso 
CHANGE      | Substitui texto no buffer atual. Ex: C /WEHRE/WHERE
CLEAR       | BREAKS/BUFFER/COLUMNS/COMPUTES/SCREEN/SQL/TIMING
COLUMN      | Formata atributos de exibição de uma coluna. Ex: COL nome FORMAT A90 HEADING 'Nome do cliente'
COMPUTE     | Em combinação com BREAK calcula totalizadores. Ex: COMP SUM LABEL valor_total OF valor ON tipo
CONNECT     | Conecta a um banco de dados. Ex: CONN usuario/senha@banco
COPY        | Copia tabelas de um banco de dados para outro usando uma query. Opções (CREATE, REPLACE, INSERT, APPEND) Ex: COPY FROM usuario/senha@bancoA TO usuario/senha@bancoB CREATE tabela_destino USING SELECT * FROM tabela_origem
DEFINE      | Define variável de substituição (usada com & e &&). Ex: DEFINE nome_tabela cliente / SELECT COUNT(*) FROM &nome_tabela;
DEL         | Exclui linha do buffer
DESCRIBE    | Descreve estrutura do objeto (tabela, view, procedimento, função, pacote, etc.)
DISCONNECT  | Desconecta da seção atual
EDIT        | Abre editor externo com o conteúdo do buffer
EXECUTE     | Executa um comando PL/SQL
EXIT        | Sai do SQL*PLUS
GET         | Lê o conteúdo de um script no buffer.
HELP        | Exibe ajuda para um comando SQL*PLUS ou a lista de comandos disponíveis. Ex: HELP INDEX, HELP BREAK
HOST        | Executa um comando do sistema operacional sem sair do SQL*PLUS. Sem parâmetros, abre um shell (EXIT volta para o SQL*PLUS). x: HOST dir c:\temp
$           | Equivalente a HOST mas não substitui variáveis (&) na linha de comando. Ex: $cls
INPUT       | Insere linha de texto no buffer na posição atual. EX: I WHERE x = y
LIST        | Lista o conteúdo do buffer
PASSWORD    | Permite alterar a senha sem exibí-la no console. (Alternativa ao: ALTER USER xxx IDENTIFIED BY yyy)
PAUSE       | Aguarda usuário pressionar ENTER. Ex: PAUSE Pressione ENTER para continuar.
PRINT       | Exibe variáveis de ligação (BIND). (começam com :)
PROMPT      | Exibe mensagem
QUIT        | Sai do SQL*PLUS
RECOVER     | Usado para restaurar backup 




## Scripts de conexão

Conexão ao banco

~~~sql
CONNECT usuario@banco/senha
~~~

Formatação da saída

~~~sql
SET LINES 1000 PAGES 200 WRAP OFF DEFINE ON LINESIZE 3000
~~~

- `PAGES[IZE]`: Número de linhas por página na saída de uma consulta. Inclui o cabeçalho mais o espaço entre página (3 linhas).
- `WRAP ON/OFF`: Define se linhas serão truncadas ou se haverá quebra de linha se largura da saída exceder o LINESIZE.
- `LINESIZE`: Tamanho da linha da saída da consulta. Se exceder o tamanho, a linha será truncada ou quebrada para a outra linha, dependendo do estado de WRAP OFF/ON.

Altera o esquema em uso na sessão atual

~~~sql
ALTER SESSION SET CURRENT_SCHEMA = usuario_esquema;
~~~

Altera formato de data e hora na saída das consultas

~~~sql
ALTER SESSION SET NLS_DATE_FORMAT = 'DD/MM/YYYY HH24:MI:SS';
ALTER SESSION SET NLS_DATE_FORMAT = 'DD/MM/YYYY HH24:MI:SS';
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYYMMDD';
~~~

Outros parâmetros úteis relacionados a idiomas:

~~~sql
ALTER SESSION SET
  NLS_LANGUAGE            = 'BRAZILIAN PORTUGUESE'
  NLS_TERRITORY           = 'BRAZIL'
  NLS_CURRENCY            = 'R$'
  NLS_ISO_CURRENCY        = 'BRAZIL'
  NLS_NUMERIC_CHARACTERS  = ',.'
  NLS_DATE_LANGUAGE       = 'BRAZILIAN PORTUGUESE'
  NLS_SORT                = 'WEST_EUROPEAN'
  NLS_TIME_FORMAT         = 'HH24:MI:SSXFF'
  NLS_TIMESTAMP_FORMAT    = 'DD/MM/RR HH24:MI:SSXFF'
  NLS_TIME_TZ_FORMAT      = 'HH24:MI:SSXFF TZR'
  NLS_TIMESTAMP_TZ_FORMAT = 'DD/MM/RR HH24:MI:SSXFF TZR'
  NLS_DUAL_CURRENCY       = 'Cr$'
  NLS_CALENDAR            = 'GREGORIAN'
  NLS_DATE_FORMAT         = 'DD/MM/YYYY HH24:MI:SS'
  NLS_COMP                = 'BINARY'
  NLS_LENGTH_SEMANTICS    = 'BYTE'
  NLS_NCHAR_CONV_EXCP     = 'FALSE'
;
~~~

Verificando outros parâmetros:

~~~sql
SELECT parameter, value
FROM nls_session_parameters
WHERE parameter LIKE '%FORMAT';

PARAMETER                      VALUE
------------------------------ ----------------------------------------
NLS_DATE_FORMAT                DD/MM/YYYY HH24:MI:SS
NLS_TIME_FORMAT                HH24:MI:SSXFF
NLS_TIMESTAMP_FORMAT           DD/MM/RR HH24:MI:SSXFF
NLS_TIME_TZ_FORMAT             HH24:MI:SSXFF TZR
NLS_TIMESTAMP_TZ_FORMAT        DD/MM/RR HH24:MI:SSXFF TZR
~~~

Altera timezone / fuso horário:

~~~sql
ALTER SESSION SET TIME_ZONE = '-08:00';            -- Fuso horário GMT -08:00
ALTER SESSION SET TIME_ZONE = 'BRT' /*ou*/ 'BRST'; -- Fuso horário de Brasília sem e com horário de verão
ALTER SESSION SET TIME_ZONE = LOCAL;               -- Fuso horário da estação conectada ao servidor
ALTER SESSION SET TIME_ZONE = DBTIMEZONE;          -- Fuso horário do servidor
~~~

Altera prompt exibido no SQL*Plus

~~~sql
SET PROMPT 'USUARIO@CONEXAO>'
~~~

Exibe tempo decorrido na execução do comando

~~~sql
SET TIMING ON
~~~

Retorna o número de registros retornados por uma consulta.

~~~sql
SET FEED[BACK] ON/OFF
~~~

Remove espaços em branco no final da linha. Não é suportado no SQL*Plus.

~~~sql
SET TRIMSPOOL ON
~~~

Formata a saída sem utilizar TABs (evita problema com desalinhamento de campos)

~~~sql
SET TAB OFF
~~~

Alterar o separador de colunas

~~~sql
SET COLSEP ' '
SET COLSEP ';'
~~~

Alterar ou desligar a linha que separa os cabeçalhos (não pode ser espaço ou alfanumérico

~~~sql
SET UNDERLINE '='
SET UNDERLINE '-'
SET UNDERLINE OFF
~~~

Executa uma linha de comando no SO da máquina local

~~~sql
HOST comando
ou
$ comando
~~~

O comando SET altera o valor de uma variável de sistema enquanto o comando SHOW mostra o valor atual da mesma.


# Utilidades

## Calcular tempo de execução

~~~sql
DECLARE
  v_inicio  TIMESTAMP; -- Tipo de dados que permite uma maior precisão nas frações de segundo
  v_fim     TIMESTAMP;
  v_duracao INTERVAL DAY(9) TO SECOND(6); -- Tipo de dado no resultado do cálculo da diferença de 2 timestamps
BEGIN
  v_inicio  := SYSTIMESTAMP;


  -- Código cujo tempo de execução será calculado


  v_fim     := SYSTIMESTAMP;
  v_duracao := v_fim - v_inicio;
  DBMS_OUTPUT.PUT_LINE('Início:  ' || TO_CHAR(v_inicio,  'HH24:MI:SS.FF'));
  DBMS_OUTPUT.PUT_LINE('Término: ' || TO_CHAR(v_fim,     'HH24:MI:SS.FF'));
  DBMS_OUTPUT.PUT_LINE('Duração: ' || TO_CHAR(v_duracao));
END;
/
~~~

## PROFILER

Pré-requisitos:

- Ter acesso à package dbms_profiler
- Ter criadas as tabelas do profiler no schema local ($ORACLE_HOME/rdbms/admin/proftab.sql)

Exemplo de utilização:

~~~sql
ALTER session set TIMED_STATISTICS = true;

declare
  x integer;
  q integer := 0;
BEGIN
  x:=dbms_profiler.start_profiler('Test Profiler');
  for reg in ( select * from table(fnc_arquivo_0104(70954)) ) loop
    q := q + 1;
  end loop;
  dbms_output.put_line(q);
  x:=dbms_profiler.flush_data;
  x:=dbms_profiler.stop_profiler;
end;
/
~~~

Execuções:

~~~sql
SELECT a.runid,
       substr(b.run_comment, 1, 30) as run_comment,
       decode(a.unit_name, '', '<anonymous>',
             substr(a.unit_name,1, 30)) as object_name,
       a.total_time,
       TO_CHAR(a.total_time/1000000, '99999.99') as msec,
       TO_CHAR(100*a.total_time/b.run_total_time, '999.9') as pct
from plsql_profiler_units a, plsql_profiler_runs b
where a.runid=b.runid
order by a.runid asc;
~~~

Tempo gasto por unidade

~~~sql
SELECT u.UNIT_NUMBER
     , u.UNIT_TYPE
     , u.UNIT_OWNER
     , u.UNIT_NAME
     , sum(d.TOTAL_TIME) / 1000000000 as segundos
     , count(*) AS linhas
from plsql_profiler_units u, plsql_profiler_data d
where u.runid = d.runid
  and u.unit_number = d.unit_number
  and u.runid = &1
group by u.UNIT_NUMBER
     , u.UNIT_TYPE
     , u.UNIT_OWNER
     , u.UNIT_NAME
     , u.runid
order by segundos;
~~~

Tempo gasto por linha:

~~~sql
SELECT u.UNIT_NAME
     , d.LINE#
     , d.TOTAL_TIME / 1000000000 as segundos
     , s.text
from plsql_profiler_units u, plsql_profiler_data d, user_source s
where u.runid = d.runid
  and u.unit_number = d.unit_number
  and u.runid = &1
  and u.UNIT_NAME = '&2'
  and u.unit_name = s.name
  and d.line# = s.line
order by segundos;
undef 1
undef 2
~~~


## Função-Tabela

Uma função que retorna uma tabela definida como resultado.

O tipo de retorno pode ser criado em uma package também.

~~~sql
CREATE OR REPLACE TYPE r_tabuada IS OBJECT (
  fator1  NUMBER(2),
  fator2  NUMBER(2),
  produto NUMBER(4)
);
/

CREATE OR REPLACE TYPE t_tabuada IS TABLE OF r_tabuada;
/

CREATE OR REPLACE FUNCTION fnc_tabuada (p_numero IN NUMBER)
RETURN t_tabuada
PIPELINED
IS
  reg r_tabuada;
BEGIN
  FOR mult IN 1..10 LOOP
    reg.fator1  := p_numero;
    reg.fator2  := mult;
    reg.produto := p_numero * mult;
    PIPE ROW(reg);
  END LOOP;
  RETURN;
END;
/
SHOW ERRORS
~~~

Obtendo o resultado:

~~~sql
SELECT * FROM TABLE(fnc_tabuada(3));
~~~



## Packages

Definição da especificação da package. Todos os itens definidos são públicos:

~~~sql
CREATE OR REPLACE PACKAGE pkg_nome
IS
  -- itens públicos: tipos, variáveis, etc
  v_nome VARCHAR2(50);
  -- declaração de procedures e functions públicas (somente cabeçalho)
  FUNCTION abc (p IN NUMBER) RETURN NUMBER;
  PROCEDURE def (p IN NUMBER);
END pkg_nome;
/
~~~

Definição do corpo da package, onde ficam itens privados e o corpo dos subprogramas públicos ou privados

~~~sql
CREATE OR REPLACE PACKAGE BODY pkg_nome
IS
  -- itens privados: tipos, variáveis, etc
  TYPE x ...;

  v_data DATE;

  -- corpo de procedures e functions
  FUNCTION abc (p IN NUMBER)
  RETURN NUMBER
  IS
  BEGIN
    ...
  END;

  PROCEDURE def (p IN NUMBER)
  IS
  BEGIN
    ...
  END;

END pkg_nome;
/
~~~

Acesso a itens de um package

~~~sql
DECLARE
  v_num NUMBER;
BEGIN
  pkg_nome.v_nome := 'teste';
  pkg_nome.def(1);
  v_num := pkg_nome.abc(2);
END;
/
~~~

Vantagens:
- Cursores e variáveis são mantidas na mesma sessão.
- Manter informações e lógica oculta.
- Modularidade.
- Economia de memória (somente uma cópia do pacote para todos os usuários).



## Triggers

Por transação:

- Ocorre para cada transação, não por número de linhas afetadas
- Não pode referenciar valor de colunas

~~~sql
CREATE OR REPLACE TRIGGER trg_nome_trigger
  BEFORE INSERT OR DELETE OR UPDATE
  ON nome_da_tabela
BEGIN
  ...
END;
/
~~~

Por linha:

- Para triggers disparados a cada linha afetada
- As colunas podem ser acessadas através de :OLD.nome_coluna e :NEW.nome_coluna

~~~sql
CREATE OR REPLACE TRIGGER trg_nome_trigger
  BEFORE INSERT OR UPDATE OR DELETE
  ON nome_da_tabela
  REFERENCING NEW AS NEW OLD AS OLD
  FOR EACH ROW
BEGIN
  ...
  IF INSERTING THEN
    -- ações exclusivamente se estiver inserindo
  END IF;

  IF UPDATING OR DELETING THEN
    -- ações se estiver atualizando ou apagando
  END IF;
  ...
END;
/
~~~

- A execução do trigger pode ser antes ou após o comando (BEFORE/AFTER)
- O trigger BEFORE pode cancelar a execução do comando, o AFTER não



## Gravação e leitura de arquivos no servidor Oracle

1. Executar como DBA:

~~~sql
ALTER SYSTEM SET UTL_FILE_DIR = 'C:\Temp', 'C:\Temp\In', 'C:\Temp\Out' SCOPE=SPFILE;
~~~

2. Reiniciar o banco:

    stopdb.bat
    startdb.bat

3. Verificar parâmetro:

~~~sql
SHOW PARAMETER UTL_FILE_DIR

NAME           TYPE        VALUE
-------------- ----------- ----------------------------------
utl_file_dir   string      C:\Temp, C:\Temp\In, C:\Temp\Out
~~~

4. Criar package UTL_FILE caso não esteja criada (como DBA). Exemplo:

    @C:\oracle\xe\app\oracle\product\11.2.0\server\rdbms\admin\utlfile.sql


Exemplo de gravação de arquivo

~~~sql
SET SERVEROUTPUT ON

DECLARE
  arquivo_saida UTL_FILE.FILE_TYPE;
BEGIN
  arquivo_saida := UTL_FILE.FOPEN('C:\Temp\Out', 'arquivo_saida.txt', 'W');
  FOR cont IN 1..10 LOOP
    UTL_FILE.PUT_LINE(arquivo_saida, 'Teste de arquivo de saída: ' || cont);
  END LOOP;
  UTL_FILE.FCLOSE(arquivo_saida);
EXCEPTION
  WHEN UTL_FILE.INVALID_PATH THEN
    DBMS_OUTPUT.PUT_LINE('Diretório de saída inválido.');
    UTL_FILE.FCLOSE(arquivo_saida);
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Problemas na geração do arquivo: ' || SQLERRM);
    UTL_FILE.FCLOSE(arquivo_saida);
END;
/
~~~

Exemplo de leitura de arquivo

~~~sql
SET SERVEROUTPUT ON

DECLARE
  arquivo_leitura UTL_FILE.FILE_TYPE;
  linha VARCHAR2(1000);
  cont NUMBER;
BEGIN
  arquivo_leitura := UTL_FILE.FOPEN('C:\Temp\In', 'arquivo_leitura.txt', 'R');
  cont := 0;
  LOOP
    BEGIN
      UTL_FILE.GET_LINE(arquivo_leitura, linha);
      cont := cont + 1;
      DBMS_OUTPUT.PUT_LINE(linha);
    EXCEPTION
      WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Linhas lidas: ' || cont);
        EXIT;
    END;
  END LOOP;
  UTL_FILE.FCLOSE(arquivo_leitura);
EXCEPTION
  WHEN UTL_FILE.INVALID_PATH THEN
    DBMS_OUTPUT.PUT_LINE('Diretório de entrada inválido.');
    UTL_FILE.FCLOSE(arquivo_leitura);
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Problemas na leitura do arquivo: ' || SQLERRM);
    UTL_FILE.FCLOSE(arquivo_leitura);
END;
/
~~~

> **ATENÇÃO:** No caso do erro abaixo:

    ORA-06512: em "SYS.UTL_FILE", line 536
    ORA-29283: operação de arquivo inválida

Verificar:

1. Verificar o nome exato do arquivo, inclusive a extensão!
2. Verificar se o caminho acessado está na relação de diretórios permitidos. (SHOW PARAMETER UTL_FILE_DIR)
3. Verificar se há permissão para acessar ao arquivo.




## Inserção em múltiplas tabelas simultaneamente

~~~sql
INSERT ALL
INTO tabela1 (c1, c2, c3) VALUES (x1, x2, x3 * 10)
INTO tabela2 (c1, c2) VALUES (x1, x2 + x3)
SELECT x1, (a + b + c) x2, x3
FROM tabela_origem
WHERE ...;
~~~

- As tabelas de inserção (tabela1, tabela2) podem ser iguais ou diferentes. O select de origem pode retornar 1 ou mais linhas.

Inserção condicional:

~~~sql
INSERT FIRST /*ou*/ ALL
  WHEN condicao1 THEN
    INTO tabela1 (c1, c2, c3) VALUES (v1, v2, v3)
  WHEN condicao2 THEN
    INTO tabela2 (c1, c2) VALUES (v1, v2)
    INTO tabela3 (c1, c3) VALUES (v1, v3)
SELECT v1, v2, v3
FROM tabela_origem
WHERE ...;
~~~

- A condição FIRST indica que somente a primeira condição verdadeira será executada.
- A condição ALL indica que todas as condições avaliadas como verdadeiras serão executadas.

Mesclando inserção incondicional e condicional

~~~sql
INSERT ALL
  WHEN 1=1 THEN -- Sempre será inserido
    INTO tabela1 (c1, c2, c3) VALUES (v1, v2, v3)
  WHEN condicao2 THEN
    INTO tabela2 (c1, c2) VALUES (v1, v2)
  WHEN condicao3 THEN
    INTO tabela3 (c1, c3) VALUES (v1, v3)
SELECT v1, v2, v3
FROM tabela_origem
WHERE ...;
~~~



Expressões regulares
--------------------

Retorna verdadeiro se a expressão regular 'expressao' estiver presente no texto. 

~~~sql
REGEXP_LIKE(texto, 'expressao')
~~~

Procura pela expressão regular 'expressao' no texto e retorna a posição em que foi encontrada. (0 se não encontrar)

~~~sql
REGEXP_INSTR(textp, 'expressao')
~~~

Procura pela expressão regular 'expressao' no texto e retorna a substring que combinar com a expressão.

~~~sql
REGEXP_SUBSTR(textp, 'expressao')
~~~

Procura pela expressão regular 'expressao' no texto e troca pelo texto substituto.

~~~sql
REGEXP_REPLACE(textp, 'expressao', substituto)
~~~

Pega somente primeiro e último nome:

~~~sql
REGEXP_REPLACE(nome, '^(\w+) .* (\w+)$', '\1 \2')
~~~






Trace
-----

Para fazer uma análise do plano de execução de uma query (sem rodar a query!):

~~~sql
set autotrace traceonly explain
SELECT * from dual;
set autotrace off
~~~

Exemplo de uma saída:

~~~
Plano de Execução
----------------------------------------------------------

----------------------------------------------------------
| Id  | Operation         | Name | Rows  | Bytes | Cost  |
----------------------------------------------------------
|   0 | SELECT STATEMENT  |      |     1 |     2 |     2 |
|   1 |  TABLE ACCESS FULL| DUAL |     1 |     2 |     2 |
----------------------------------------------------------

Note
-----
   - 'PLAN_TABLE' is old version
~~~

Tempos de execução e estatísticas:

~~~sql
SELECT * FROM v$sql WHERE sql_text LIKE UPPER('SELECT ...%');
~~~




Database link
-------------

~~~sql
create database link banco_a connect to USUARIO identified by SENHA using 'banco_a';
create database link banco_b connect to USUARIO identified by SENHA using 'banco_b';
~~~

Source/DDL de objetos
---------------------

~~~sql
SELECT DBMS_METADATA.GET_DDL('TABLE','PERSON') from DUAL;
~~~

Pivot table
-----------

Exemplo:

~~~sql
SELECT * FROM
(
  SELECT id_tipocar, status_cartao FROM cartao WHERE rownum <= 100
)
PIVOT
(
  COUNT(*) FOR status_cartao IN ('A', 'B', 'C', 'D', 'E', 'R', 'X', 'U', 'S', 'V', 'T', 'P', 'O', 'Y')
)
ORDER BY id_tipocar;
~~~

~~~sql
SELECT * from (
   select times_purchased as "Puchase Frequency", state_code
   from customers t
)
pivot
(
   count(state_code)
   for state_code in ('NY' as "New York",'CT' "Connecticut",'NJ' as "New Jersey",'FL' as "Florida",'MO' as "Missouri")
)
order by 1;
~~~

~~~sql
SELECT *
from
(
  select to_char(d.data_procto, 'MM') as mes, d.id_crb_evento, e.descricao, e.sinal, d.valor
  from crb_detalhe d, crb_evento e
  where d.id_crb_evento = e.id_crb_evento
    and d.data_procto between to_date('01/01/2015 00:00:00', 'dd/mm/yyyy hh24:mi:ss')
                          and to_date('31/12/2015 23:59:59', 'dd/mm/yyyy hh24:mi:ss')
)
pivot
(
   sum(valor)
   for mes in
   (
     '01' as "Jan", '02' as "Fev", '03' as "Mar", '04' as "Abr", '05' as "Mai", '06' as "Jun",
     '07' as "Jul", '08' as "Ago", '09' as "Set", '10' as "Out", '11' as "Nov", '12' as "Dez"
   )
)
order by sinal, descricao;
~~~



## Chaves primárias compostas

~~~sql
SELECT uc.table_name, constraint_name, listagg(ucc.column_name, ', ') within group (order by ucc.position)
from user_constraints uc
inner join user_cons_columns ucc using (constraint_name)
where uc.constraint_type = 'P'
group by uc.table_name, constraint_name
having count(*) > 1;
~~~

## Chaves primárias que não são numéricas

~~~sql
SELECT uc.constraint_name, utc.table_name, utc.column_name, utc.data_type
from user_constraints uc
inner join user_cons_columns ucc on uc.constraint_name = ucc.constraint_name
inner join user_tab_columns utc on ucc.table_name = utc.table_name and ucc.column_name = utc.column_name
where uc.constraint_type = 'P'
and utc.data_type not in ('NUMBER', 'INTEGER', 'SMALLINT')
order by 1, 2, 3, 4;
~~~




EXECUTE IMMEDIATE
-----------------

Exemplo:

~~~sql
execute immediate ' select 1 '
                || ' from all_objects@' || reg.db_link || ' o2 '
                || ' where o2.owner       = :param1 '
                || '   and o2.object_name = :param2 '
                || '   and o2.timestamp   > :param3 '
into v_dummy
using reg.table_owner, reg.table_name, reg.timestamp_local;
~~~

Importante: O **nome** dos binds não importa. A **ordem** dos parâmetros em using deve ser a mesma das variáveis de BIND usadas na query.



CUSTOM EXCEPTION
----------------

~~~sql
DECLARE

  ex_saldo_total    EXCEPTION;

  PRAGMA EXCEPTION_INIT(ex_saldo_total,    -20101);

BEGIN

  IF ... THEN
    RAISE ex_saldo_total;
  END IF;

EXCEPTION
  WHEN ex_saldo_total THEN
    ...
END;
~~~



## Administração (DBA)

Conectar como DBA:

~~~sql
CONNECT sys/senha@banco AS SYSDBA
~~~

Exportar todos os objetos do banco sem os dados:

~~~sql
EXP.EXE login/password@TNSNAME file=entire_db.dmp owner=(scott, my_user, user2) rows=n grants=y triggers=y
~~~



Ao criar um índice ocorre o seguinte erro pois a tabela está em uso:

    ORA-00054: o recurso está ocupado e é obtido com o NOWAIT especificado ou o timeout expirou

Este comando faz com que a criação do índice aguarde a tabela ficar disponível (60 segundos) para poder criar o índice:

    alter session set DDL_LOCK_TIMEOUT = 60;




## Procedimento "Kamikaze"
  
Procedimento que executa uma vez e se auto-destrói

~~~sql
CREATE or replace procedure kamikaze
is
  job pls_integer;
BEGIN
  dbms_job.submit(job, 'begin execute immediate ''drop procedure kamikaze''; end;', sysdate + interval '10' second);
  commit;
end;
/
~~~

Agenda a execução do procedimento uma única vez (e sai da fila de jobs após executado)


~~~sql
declare
  job pls_integer;
BEGIN
  dbms_job.submit(job, 'kamikaze;', sysdate + interval '10' second);
  commit;
end;
/
~~~



## MERGE INTO ...

Faz inserção/atualização de registros de uma tabela para outra. Exemplos:

~~~sql
MERGE INTO sistema.loja_grupo a
  USING (SELECT * FROM loja_grupo) b
  ON (a.id_loja_grupo = b.id_loja_grupo)
WHEN MATCHED THEN 
  UPDATE SET a.id_tipo_grupo  = b.id_tipo_grupo
            , a.descricao      = b.descricao
WHEN NOT MATCHED THEN
  INSERT (a.id_loja_grupo, a.id_tipo_grupo, a.descricao)
  VALUES (b.id_loja_grupo, b.id_tipo_grupo, b.descricao);
~~~

~~~sql
MERGE INTO sistema.loja_grupo_detalhe a
  USING (SELECT * FROM loja_grupo_detalhe) b
  ON (a.id_loja_grupo = b.id_loja_grupo AND a.id_loja = b.id_loja)
WHEN MATCHED THEN 
  UPDATE SET a.incluir_filiais = b.incluir_filiais
WHEN NOT MATCHED THEN
  INSERT (a.id_loja_grupo, a.id_loja, a.incluir_filiais) 
  VALUES (b.id_loja_grupo, b.id_loja, b.incluir_filiais);
~~~



## Usando aspas no estilo "PERL" (Oracle 10g +):

~~~sql
BEGIN
  dbms_output.put_line(q'!Teste usando !! para delimitar string, permitindo usar ' no meio da string.!');
  dbms_output.put_line(q'[Teste usando [] para delimitar string, permitindo usar ' no meio da string.]');
  dbms_output.put_line(q'{Teste usando {} para delimitar string, permitindo usar ' no meio da string.}');
  dbms_output.put_line(q'(Teste usando () para delimitar string, permitindo usar ' no meio da string.)');
  dbms_output.put_line(q'<Teste usando <> para delimitar string, permitindo usar ' no meio da string.>');
  
  dbms_output.put_line(q'!Teste 
  
    com 'múltiplas'

      linhas...
        !');
  -- '
end;
/
~~~



## Operações em massa (BULK)

Exemplos BULK COLLECT / FETCH / LIMIT / INSERT / etc.

~~~sql
CREATE TABLE tmp_arq_conteudo_copia (id_arq_conteudo NUMBER(4), descricao VARCHAR2(50));

DELETE FROM tmp_arq_conteudo_copia;

DECLARE

  CURSOR cur_teste IS
    SELECT id_arq_conteudo, descricao
      FROM arq_conteudo
     ORDER BY 1;

  TYPE t_tabela IS TABLE OF cur_teste%ROWTYPE;
  v_tabela t_tabela;

BEGIN

  dbms_output.put_line('*******************************************');
  dbms_output.put_line('*** Obtem todos os registros de uma vez ***');
  dbms_output.put_line('*******************************************');

  OPEN cur_teste;
  FETCH cur_teste BULK COLLECT INTO v_tabela;
  CLOSE cur_teste;

  dbms_output.put_line('Processando registros: ' || v_tabela.COUNT);

  FOR indice IN 1..v_tabela.COUNT
  LOOP
    dbms_output.put_line(lpad(v_tabela(indice).id_arq_conteudo, 6, '0') || ' -> ' || v_tabela(indice).descricao);
  END LOOP;

  dbms_output.put_line('*************************');
  dbms_output.put_line('*** Inserção em massa ***');
  dbms_output.put_line('*************************');


  FORALL indice IN 1..v_tabela.COUNT
  INSERT INTO tmp_arq_conteudo_copia (id_arq_conteudo, descricao) VALUES (v_tabela(indice).id_arq_conteudo, v_tabela(indice).descricao);

  dbms_output.put_line('********************************');
  dbms_output.put_line('*** Obtem registros em lotes ***');
  dbms_output.put_line('********************************');

  OPEN cur_teste;

  LOOP
    FETCH cur_teste BULK COLLECT INTO v_tabela LIMIT 10;
    EXIT WHEN v_tabela.COUNT = 0;

    dbms_output.put_line('Processando próximos: ' || v_tabela.COUNT);

    FOR indice IN 1..v_tabela.COUNT
    LOOP
      dbms_output.put_line(lpad(v_tabela(indice).id_arq_conteudo, 6, '0') || ' -> ' || v_tabela(indice).descricao);
    END LOOP;

  END LOOP;

  CLOSE cur_teste;

END;
/
~~~


## Desvio usando exception 

~~~sql
DECLARE
  erro_cliente     EXCEPTION;
  
  PRAGMA EXCEPTION_INIT(erro_cliente, -20999);

BEGIN

  FOR i IN 1..3 LOOP
  
    BEGIN

      dbms_output.put_line('Tudo normal ' || i);
      
      IF i = 2 THEN
        RAISE erro_cliente;
      END IF;
      
      dbms_output.put_line('Tuuuuudo normal ' || i);

    EXCEPTION
      WHEN erro_cliente THEN
        dbms_output.put_line('EITA!!!!!!! => ' || SQLCODE);    
    END;
    
  END LOOP;
  
END;
/
~~~

## Desvio usando goto... /ô>

~~~sql
BEGIN

  FOR i IN 1..3 LOOP
  
    dbms_output.put_line('Tudo normal ' || i);
    
    IF i = 2 THEN
      GOTO erro_cliente;
    END IF;
    
    dbms_output.put_line('Tuuuuudo normal ' || i);
    
    GOTO ok;
  <<erro_cliente>>
    dbms_output.put_line('EITA!!!!!!! => ' || SQLCODE);    
  <<ok>>
    null;
  END LOOP;
  
END;
/
~~~


## Exportar privilégios

"Exportar" privilégios aplicados em uma base para gerar script a ser executado em outra.

Um privilégio por linha (Ex: Oracle 9)

~~~sql
SELECT 'GRANT ' || privilege || ' ON ' || owner || '.' || table_name || ' TO ' || grantee || ';'
  FROM dba_tab_privs
 WHERE grantee = 'USUARIO'
   AND owner = 'PROPRIETARIO';
~~~

Agrupado por objeto (Ex: Oracle 10 ou maior)

~~~sql
SELECT 'GRANT ' || LISTAGG(privilege, ',') WITHIN GROUP (ORDER BY privilege) || ' ON ' || owner || '.' || table_name || ' TO ' || grantee || ';'
  FROM dba_tab_privs
 WHERE grantee = 'USUARIO'
   AND owner = 'PROPRIETARIO'
 GROUP BY owner, table_name, grantee;
~~~ 

## XML

Transformando XML em uma tabela:

~~~sql
SELECT * from table(xmlsequence(xmltype('<ids><id>2</id><id>3</id><id>4</id></ids>').extract('ids/*')));
~~~

~~~sql
SELECT to_number(extractvalue(value(x), 'cliente/id')) as id
     , substr(extractvalue(value(x), 'cliente/nome'), 1, 50) as nome
  from table(xmlsequence(xmltype(
          '<clientes><cliente><id>1</id><nome>João</nome></cliente>' || 
          '<cliente><id>2</id><nome>Maria</nome></cliente></clientes>').extract('clientes/*'))
       ) x;
~~~

## Criptografia e Hash

Calcular Hash MD5 de uma string

~~~sql
exec dbms_output.put_line(rawtohex(dbms_obfuscation_toolkit.md5(input => utl_raw.cast_to_raw('6278929999999999'))));

exec dbms_output.put_line(rawtohex(dbms_crypto.hash(src => utl_raw.cast_to_raw('6278929999999999'), typ => dbms_crypto.hash_md5)));
~~~

## "PLS-00907: cannot load library unit"

Como SYS:

~~~sql
alter system flush shared_pool;
~~~


# SQL LOADER

Primeiro a tabela deve ser criada e deve estar vazia. Exemplo:

~~~sql
CREATE table tmp_crivo (
  cpf   varchar2(11) not null primary key,
  renda number(10,2) not null
);
~~~

## Exemplos do comando para importação:

    sqlldr usuario/senha@servidor control=CONTROLE.ctl bad=ERROS.bad discard=IGNORADO.dsc log=MENSAGENS.log

As diretivas acima são bad (linhas com erro), discard (linhas não processadas), log (mensagens e erros na importação)

## Exemplos do arquivo de controle (*.ctl):

### Arquivo simples somente com um campo

~~~
load data
 infile 'C:\Temp\CPFs_ja_importados.txt'
 into table tmp_cpf_vcom
 FIELDS TERMINATED BY "," OPTIONALLY ENCLOSED BY '"'
 TRAILING NULLCOLS
 ( cpf )
~~~

### Arquivo csv com vários campos, sem formatação especifica

~~~
load data
 infile 'C:\Temp\cartoes_cvv_gold.csv'
 into table tmp_22188
 FIELDS TERMINATED BY "," OPTIONALLY ENCLOSED BY '"'
 TRAILING NULLCOLS
 ( ciccli, nome, id_cartao_antigo, layout, titularidade, id_cliente_conjunto )
~~~

### Arquivo csv com formatação específica em cada campo

~~~
load data
 infile 'C:\temp\baixas_vcom_20130502.csv'
 into table tmp_baixas_vcom
 FIELDS TERMINATED BY "," OPTIONALLY ENCLOSED BY '"'
 TRAILING NULLCOLS
 ( nome_cred             "trim(:nome_cred)"
 , nome_fili_distr       "trim(:nome_fili_distr)"
 , sigla
 , cod_rec
 , contrato_tit
 , numero_parc
 , tipo_pagamento
 , vcto_parc             "to_date(:vcto_parc,   'YYYY-MM-DD HH24:MI:SS    ')"
 , dt_pgto_rec           "to_date(:dt_pgto_rec, 'YYYY-MM-DD HH24:MI:SS    ')"
 , atraso
 , valor_atualizado      "to_number(replace(:valor_atualizado,    '.', ''))"
 , valor_recebido_rec    "to_number(replace(:valor_recebido_rec,  '.', ''))"
 , percentual_comissao   "to_number(replace(:percentual_comissao, '.', ''))"
 , comissao              "to_number(replace(:comissao,            '.', ''))"
 )
~~~

### Arquivo csv com 2 campos

- Um deles é numérico na origem mas deve ser formatado como char ao inserir na tabela

~~~
load data
 infile 'C:\Temp\crivo_renda.csv'
 into table usuario.tmp_crivo
 FIELDS TERMINATED BY ";" OPTIONALLY ENCLOSED BY '"'
 TRAILING NULLCOLS
 ( cpf                   "to_char(:cpf, 'fm00000000000')"
 , renda                 "to_number(:renda)"
 )
~~~

### Arquivos posicionais

- Exemplo de importação de base dos correios (DNE)

`dne_localidade.ctl`:

~~~
OPTIONS( bindsize=10240000, READSIZE=10240000, rows=5000000 )
LOAD DATA
  INFILE 'C:\DNE_GU\GU\DNE_GU_LOCALIDADES.TXT'
  BADFILE 'C:\Importa_cep\BAD\DNE_GU_LOCALIDADES.BAD'
  DISCARDFILE 'C:\Importa_cep\DSC\DNE_GU_LOCALIDADES.DSC'
  APPEND
  INTO TABLE dne_localidade
    WHEN (001:001) = 'D'
  (dne_loc_nu     position(012:019) INTEGER EXTERNAL,
   nome           position(020:091) char NULLIF nome=BLANKS "replace(:nome,'''','´')",
   uf             position(004:005) char NULLIF uf=BLANKS,
   tipo           position(136:136) char NULLIF tipo=BLANKS,
   nome_abreviado position(100:135) char NULLIF nome_abreviado=BLANKS     "replace(:nome_abreviado,'''','´')",
   cep_unico      position(092:099) INTEGER EXTERNAL
  )
~~~
  
`dne_uf.ctl`:

~~~
OPTIONS( bindsize=10240000, READSIZE=10240000, rows=5000 )
LOAD DATA
  INFILE 'C:\DNE_GU\GU\DNE_GU_UNIDADES_FEDERACAO.TXT'
  BADFILE 'C:\Importa_cep\BAD\DNE_GU_UNIDADES_FEDERACAO.BAD'
  DISCARDFILE 'C:\Importa_cep\DSC\DNE_GU_UNIDADES_FEDERACAO.DSC'
  APPEND
  INTO TABLE dne_UF
    WHEN (001:001) = 'D'
  (uf   position(004:005) char NULLIF uf=BLANKS,
   nome position(010:030) char NULLIF nome=BLANKS
  )
~~~

`dne_bairro.ctl`:

~~~
OPTIONS( bindsize=10240000, READSIZE=10240000, rows=5000000 )
LOAD DATA
  INFILE 'C:\DNE_GU\GU\DNE_GU_BAIRROS.TXT'
  BADFILE 'C:\Importa_cep\BAD\DNE_GU_BAIRROS.BAD'
  DISCARDFILE 'C:\Importa_cep\DSC\DNE_GU_BAIRROS.DSC'
  APPEND
  INTO TABLE dne_bairro
    WHEN (001:001) = 'D'
  (dne_bai_nu       position(095:102) INTEGER EXTERNAL NULLIF dne_bai_nu=BLANKS,
   dne_loc_nu       position(010:017) INTEGER EXTERNAL NULLIF dne_loc_nu=BLANKS,
   nome             position(103:174) char NULLIF nome=BLANKS,
   nome_abreviado   position(175:210) char NULLIF nome_abreviado=BLANKS
  )
~~~

### Exemplo complexo

- Múltiplos arquivos de entrada
- Múltiplas tabelas de destino (com relação de mestre-detalhe)
- Campos posicionais de vários formatos

~~~
load data
infile 'c:\temp\cobranca\chefaly\2009\r0063001.cob'
infile 'c:\temp\cobranca\chefaly\2009\r0063002.cob'
infile 'c:\temp\cobranca\chefaly\2009\r0063003.cob'
append
trailing nullcols

into table chefaly_arquivo
when (1:2) = 'HD'
(
  id_chefaly_arquivo  "seq_id_chefaly_arquivo.nextval",
  id_cobradora        position(3  :  6),
  data_arquivo        position(157:164)  "to_date(:data_arquivo, 'DDMMYYYY')",
  numero_arquivo      position(165:172)  "to_number(:numero_arquivo)"
)

into table chefaly_faixa
when (1:2) = 'FX'
(
  id_chefaly_arquivo  "seq_id_chefaly_arquivo.currval",
  faixa_ini           position(3: 6)     "to_number(:faixa_ini)",
  faixa_fim           position(7:10)     "to_number(:faixa_fim)"
)

into table chefaly_totais
when (1:2) = 'TR'
(
  id_chefaly_arquivo  "seq_id_chefaly_arquivo.currval",
  qtd_clientes        position( 3:12)    "to_number(:qtd_clientes)",
  qtd_dividas         position(13:22)    "to_number(:qtd_dividas)",
  total_arquivo       position(23:39)    "to_number(:total_arquivo)/100"
)

into table chefaly_detalhe
when (1:2) = 'DV'
(
  id_chefaly_arquivo  "seq_id_chefaly_arquivo.currval",
  cod_operacao        position( 3: 3),
  ciccli              position( 4:14),
  id_divida           position(15:24),
  tipo_divida         position(25:25),
  dataven             position(26:33)    "to_date(:dataven, 'DDMMYYYY')",
  datapag             position(34:41)    "to_date(:datapag, 'DDMMYYYY')",
  valor               position(42:58)    "to_number(:valor)/100",
  encargos            position(59:75)    "to_number(:encargos)/100"
)
~~~

# Oracle XE

## Resumo da instalação inicial

- Pasta de Destino: C:\oraclexe\
- Oracle Home: C:\oraclexe\app\oracle\product\11.2.0\server\
- Oracle Base: C:\oraclexe\
- Porta para o 'Oracle Database Listener': 1521
- Porta para o 'Oracle Services for Microsoft Transaction Server': 2030
- Porta para o 'Oracle HTTP Listener': 8080

## Ativando o usuário de exemplo HR

~~~sql
ALTER USER hr ACCOUNT UNLOCK IDENTIFIED BY hr;
~~~

