# **Banco de Dados**

## **Criando Banco de Dados**
```sql
create database BancoTeste
```
## **Apagando Banco de Dados**
```sql
drop database BancoTeste
```
## **Usando um banco qualquer**
```sql
use BancoTeste
```
## **Criando uma tabela**
```sql
create table cidade
{
    codCidade int primary key,
    > BANCO MICROSOFT 
    descricao varchar(50) not null,
    populacao int default -1
}
```
### **Outro jeito de criar uma tabela**
> Usado normalmente para chave composta

```sql
create table processo
{
    processo int,
    descricao varchar(50) not null,
    populacao int default 0
    primary key(codCidade),
}
```

> Primary key(PK) - Define como chave primária 

> Primary key(a,b,c) - Uso de chave composta

> Varchar(n), n - número máximo de caracteres

> Default - define o valor padrão


## **Adicionando campos na tabela**
```sql
alter table processo
    add codCidade int not null,
        teste varchar(10)
```

## **Excluindo coluna da tabela**
```sql
alter table processo
    drop column teste
```

## **Adicionando uma Foreign Key(FK)**
```sql
alter table processo
    add constraint fk_processo_cidade
    foreign key(codCidade)
    references cidade(codCidade)
```
> Foreign Key - Chave estrangeira, usada para se relacionar com outra tabela

> References - Se refere a primary key da tabela que irá se relacionar

## **Criando tabela com PK e FK juntas**
```sql
create table processo
{
    processo int,
    ano Smallint,
    dataAbertura smalldatetime not null,
    descricao varchar(50) default 'Sem      descrição',
    codCidade foreign key,
    references cidade(codCidade)
        primary key(processo,ano)
}
```

## **Inserindo dados na tabela**
```sql
use BancoTeste
insert into cidade(codCidade,descricao,populacao) values (1, 'SBC', 4500)

insert into cidade(codCidade,descricao,populacao) values (2, 'Maua', 14500)
```
> Como o campo descrição é not null, se tentar adicionar uma cidade sem descrição não conseguirá

## **Alterando dados da tabela**
```sql
update cidade set descricao = 'Santo André' where codCidade = 1

update cidade set populacao = 10 where codCidade = 2
```

> set - alterar o valor

> where - filtrar o campo que deseja alterar

```sql
update Forum set codCidade = 1
```
> Atualizar todos os Foruns da cidade com código igual a 1

## **Excluindo dados da tabela**
```sql
delete from cidade where codCidade = 1

delete from cidade where codCidade = 2
```

> from - se refere a tabela que deseja realizar o comando

> Caso seja uma Fk de outra tabela não conseguirá apagar o dado

## **Selecionando todos os campos da tabela**
```sql
select * from cidade
```
> O * significa que irá selecionar todos os campos da tabela

## **Selecionando alguns campos da tabela**
```sql
select codCidade, descricao from cidade
```
> Seleciona apenas os campos código e descrição da cidade

## **Usando apelidos**
```sql
select codCidade as Cidade, descricao as Descrição from cidade
```
#### **ou**
```sql
select codCidade Cidade, descricao Descrição from cidade
```
> Pode se usar ou não o ```as```

## **Usando o operador Is NULL/ Is not NULL**
```sql
select * from Forum where codCidade = NULL

select * from Forum where codCidade = 'NULL'

select * from Forum where codCidade is NULL
```
> Exibe todos os campos dos Foruns que possuem o código da cidade nulo.

## **Operador BETWEEN**
```sql
select * from funcionarios where func_salario between 200 and 1400
```
#### **ou**
```sql
select * from funcionarios where func_salario >= 200 and func_salario <=1400
```
> Exibe os funcionários que possuem o salário entre 200 e 1400

## **Usando a clausula Like**
> Listar os funcinários que começam com as letras ```An```
```sql
select * from funcionarios where func_nome like 'an%'
```

> Listar os funcinários que terminam com as letras ```an```
```sql
select * from funcionarios where func_nome like '%an'
```

> Listar os funcinários que contenham as letras ```an``` em qualquer lugar do nome
```sql
select * from funcionarios where func_nome like '%an%'
```
> Listar todos os flavio's e flavia's do jeito "ruim"
```sql
select * from funcionarios
where func_nome like 'flavio %' or
	  func_nome like 'flavia %'
```

> Listar todos os flavio's e flavia's do jeito "bom"
```sql
select * from funcionarios
where func_nome like 'flavi_ %'
```

> Listar todos os flavio's e flavia's do melhor jeito
```sql
select * from funcionarios
where func_nome like 'flavi[ao] %'
```

## **Usando operador IN**
> Listar funcionarios de código 1, 3 ou 5

```sql
select * from funcionarios
where func_id in (1,3,5)
```
#### *ou*
```sql
select * from funcionarios
where func_id = 1 or func_id = 3 or
	  func_id = 5
```
> Listar funcionarios que não tem código 1, 3 ou 5

```sql
select * from funcionarios
where func_id NOT in (1,3,7)
```
## **Usando operador DISTINCT**
> Descobrir quais setores distintos existem na tabela

> Esse operador só funciona com um campo
```sql
select distinct setor_id from funcionarios
```

## **Usando operador TOP**
> Buscar os 3 primeiros
```sql
select top 3 * from funcionarios
```

> Exibir os 4 maiores salários
```sql
select top 4 * from funcionarios order by func_salario desc
```
> order by - ordena os dados por um campo escolhido

> desc - exibe o resultado em ordem decrescente

## **Exercícios de Criação de Tabelas**
> 1-) Crie uma tabela chamada Advogado com código, nome e data de nascimento.
```sql
create table Advogado
(
    codAdvogado int primary key,
    nome varchar(30) not null,
    data_nasc smalldatetime not null,
)
```
> ```smalldatetime(YYYY-MM-DD hh:mm:ss)````

> 2-) Adicione na tabela Processo um campo que faça relacionamento com a tabela Advogado
```sql
alter table processo
    add codAdvogado int not null
        foreign key(codAdvogado) references
                    Advogado(codAdvogado)
```
> 3-) Crie uma tabela chamada Forum com código, nome, endereço, código da cidade(FK).
```sql
create table Forum(
    codForum int primary key,
    nome varchar(80),
    endereco varchar(100),
    codCidade int foreign key references
                        cidade(codCidade)
)
```
> 4-) Adicione na tabela Advogado um campo chamado numOAB.
```sql
alter table Advogado
    numOAB varchar(10) not null
```

## **Usando o comando INNER JOIN**
> Operação de junção que combina uma ou mais tabelas do banco de dados.

```sql
select * from funcionarios f inner join setores s on (f.setor_id) = (s.setor_id)
```
### **ou**
```sql
select * from funcionarios f, setor s where f.setor_id = s.setor_id
```
## **Usando o comando ORDER BY**
> Listar os funcionários por ordem alfabetica
```sql
select * from funcionarios order by func_nome
```
> Listar os funcionários por salário em ordem crescente
```sql
select * from funcionarios order by func_salario
```
> Listar os funcionários por salário em ordem descrescente
```sql
select * from funcionarios order by func_salario desc
```

> Ordenar por mais de um campo(ordena pelo primeiro campo e depois ordena pelos outros)
```sql
select * from funcionarios order by func_salario, func_nome
```

## **Usando Funções de Agrupamento**
### **COUNT**
```sql
select COUNT(*) from funcionarios where gerente_id = 1
```
> Retorna a quantidade de funcinários que possui o gerente 1

### **MAX**
```sql
select MAX(func_salario) from funcionarios
```
> Retorna o maior salário (somente o valor)

### **TOP**
```sql
select top 1 func_nome from funcionarios order by func_salario desc
```
> Retorna o nome do funcionário com maior salário

```sql
select top 1 func_nome from funcionarios order by func_salario
```
> Retorna o nome do funcionário com menor salário

```sql
select top 3 func_nome from funcionarios order by func_salario desc
```
> Retorna o nome dos 3 funcionários com maior salário

### **SUM**
```sql
select SUM(func_salario) as Salario from funcionarios
```
> Retorna a soma de todos os salários

```sql
select SUM(func_salario) Salario, COUNT(*) QtdeDeFunc from funcionarios
```
> Retorna a soma de todos os salários e também a quantidade de funcionários

## **Usando GROUP BY**
> Listar o total pago aos funcionários de cada setor
```sql
select setor_id as Setor, SUM(func_salario) Total from funcionarios group by setor_id
```
> Listar quantos funcionários possuem em cada setor separados por ano de nascimento
```sql
select  setor_id Setor, YEAR(func_dataNasc) Ano, COUNT(*) Qtde from funcionarios group by setor_id, YEAR(func_dataNasc) order by setor_id, YEAR(func_dataNasc)
```
> A função YEAR() retorna apenas o ano correspondente da data escolhida. Assim como também possui as funções MOUNTH() e DAY().

### **Having**
> Usado somente quando tiver a clausula GROUP BY, funciona basicamente igual o WHERE
```sql
select COUNT(*) qtd, setor_id from funcionarios group by setor_id having COUNT(*) >= 5
```
> Lista a quantidade de funcionários nos setores que possuem mais de 5 funcionários

## **Usando UNION**
> Usado quando se quer combinar duas colunas similares que não possuem relacionamento direto. Observação: as colunas devem ter o mesmo tipo de dado.
```sql
select func_id, func_nome from funcionarios union select setor_id, setor_nome from setores
```
### **UNION ALL**
> A principal diferença entre UNION e UNION ALL é que no caso de UNION ALL ele não elimina os valores duplicados, ou seja, aparecem mais de uma vez os mesmos valores. Enquanto no UNION isso não acontece.
```sql
select * from setores union all select * from setores
```

## **Usando Cast**
