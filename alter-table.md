# ALTERAÇÕES EM TABELAS COM SQL (ALTER TABLE)

Esta nota de aula mostra como fazer diversas alterações em uma tabela SQL que já possui dados, com foco em:

- Alterar nomes de atributos (colunas)
- Alterar tipo e tamanho de atributos
- Adicionar/remover chaves primárias e estrangeiras
- Ativar/desativar CASCADE
- Renomear a tabela

---

## 1. Renomear a Tabela

### O que quer fazer:
Alterar o nome da tabela sem perder os dados.

### Comando:
sql
RENAME TABLE nome_antigo TO nome_novo;
-- ou em PostgreSQL:
ALTER TABLE nome_antigo RENAME TO nome_novo;


### Resultado:
- Os dados permanecem intactos.
- É necessário atualizar todas as referências ao nome antigo da tabela (views, procedures, etc).

---

## 2. Renomear uma Coluna

### O que quer fazer:
Alterar o nome de um atributo (coluna) já existente.

### Comando:
sql
ALTER TABLE nome_da_tabela RENAME COLUMN nome_antigo TO nome_novo;


### Resultado:
- Os dados permanecem.
- Queries e scripts que usam o nome antigo quebram, exigindo atualização.

---

## 3. Alterar o Tipo de uma Coluna

### O que quer fazer:
Mudar o tipo de dado de uma coluna (ex: VARCHAR → INT).

### Comando:
sql
ALTER TABLE nome_da_tabela MODIFY nome_coluna NOVO_TIPO;


### Resultado:
- Se os dados forem compatíveis, a alteração ocorre normalmente.
- ⚠ Se houver dados incompatíveis, o comando falha.
- ⚠ Pode haver truncamento de dados (ex: FLOAT para INT).

---

## 📏 4. Alterar o Tamanho de uma Coluna

### O que quer fazer:
Alterar o tamanho de um campo VARCHAR, por exemplo.

### Comando:
sql
ALTER TABLE nome_da_tabela MODIFY nome_coluna VARCHAR(novo_tamanho);


### Resultado:
- Aumentar o tamanho é seguro.
- Reduzir pode causar erro se houver valores maiores que o novo limite.

---

## 5. Adicionar ou Remover Chave Primária

### O que quer fazer:
Definir ou remover uma chave primária na tabela.

### Adicionar:
sql
ALTER TABLE nome_da_tabela ADD PRIMARY KEY (coluna1[, coluna2]);


### Remover:
sql
ALTER TABLE nome_da_tabela DROP PRIMARY KEY;


### Resultado:
- Para adicionar, os dados existentes devem ser únicos e não nulos.
- Caso contrário, o comando falha.
- Remover não afeta os dados existentes, mas elimina a garantia de unicidade.

---

## 6. Adicionar ou Remover Chave Estrangeira

### O que quer fazer:
Criar ou remover um vínculo com outra tabela.

### Adicionar:
sql
ALTER TABLE nome_da_tabela
ADD CONSTRAINT nome_fk FOREIGN KEY (coluna)
REFERENCES outra_tabela(coluna_referenciada);


### Remover:
sql
ALTER TABLE nome_da_tabela DROP FOREIGN KEY nome_fk;


### Resultado:
- Ao adicionar, os dados precisam corresponder aos valores da tabela referenciada.
- Se não corresponderem, o comando falha.
- Remover a constraint não afeta os dados existentes, mas pode gerar inconsistência futura.

---

## 7. Adicionar ou Remover CASCADE

### O que quer fazer:
Definir ou remover o comportamento em cascata ao atualizar/deletar registros relacionados.

### Adicionar:
sql
ALTER TABLE nome_da_tabela
ADD CONSTRAINT nome_fk FOREIGN KEY (coluna)
REFERENCES outra_tabela(coluna)
ON DELETE CASCADE ON UPDATE CASCADE;


### Remover:
sql
ALTER TABLE nome_da_tabela DROP FOREIGN KEY nome_fk;

ALTER TABLE nome_da_tabela
ADD CONSTRAINT nome_fk FOREIGN KEY (coluna)
REFERENCES outra_tabela(coluna);


### Resultado:
- Com CASCADE, ao deletar/atualizar um registro pai, os filhos são automaticamente afetados.
- A mudança passa a valer dali pra frente; dados existentes não são modificados.
- Remover CASCADE impede que atualizações/deleções se propaguem automaticamente.
