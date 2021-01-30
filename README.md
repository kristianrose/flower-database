# Bem-vindo ao banco de dados Flower Classe V 1.0 
A classe FlowerDB é uma classe construtora de query simples em PHP para aumentar sua produtividade, você não pode imaginar quanto tempo vai economizar se estiver usando essa classe! .
`Caso encontrei consulta = query`
## Recursos
* Totalmente seguro:
Esta classe de banco de dados usa instruções preparadas por PDO para fornecer altos níveis de proteção contra ataques de injeção de SQL
* Fácil uso:
A sintaxe é muito simples e há muitas maneiras de fazer a mesma query, então você pode usar da maneira que quiser;)
* Bem documentado :
Tudo o que você quer saber sobre essa aula está aqui e muito bem organizado, para que você possa encontrar facilmente.

## Uso
Depois de baixar a classe [aqui] (https://raw.githubusercontent.com/mareimorsy/DB/master/DB.php) salve-o em seu diretório raiz e abra-o para ajustar as configurações básicas para sua conexão de banco de dados como host , nome do banco de dados, nome de usuário e senha do banco de dados. E você também pode definir facilmente seu ambiente de desenvolvimento atual para `development` or `production`.
```php
//ambiente de desenvolvimento atualambiente de desenvolvimento atual
"env" => "development",
//Localhost
"development" => [
					"host" => "localhost",
					"database" => "teste",
					"username" => "root",
					"password" => ""
				 ],
//Servidor
"production"  => [
					"host" => "",
					"database" => "",
					"username" => "",
					"password" => ""
				 ]
```
Para usar a classe, basta incluí-la em seus arquivos de projeto como este
```php
include 'DB.php';
```
Então você tem que instanciar a classe assim
```php
$db = DB::getInstance();
```
Agora, `$db` objeto é uma nova instância da classe DB, vamos usar este objeto para lidar com nosso banco de dados, e você pode criar muitos objetos como quiser (não se preocupe com as conexões porque estou usando o padrão de design Singleton, então sempre que você criar novos objetos ele retorna a mesma conexão) . 
### Inserir valores em uma tabela
use o método `insert()` para inserir valores em uma tabela, e leva 2 parâmetros: o primeiro é `$table_name` e o segundo é um array associativo` $fields [] `então a chave desse array é o nome da coluna na tabela e o valor dessa matriz é o valor que você deseja inserir nessa coluna.
```php
$db->insert('tabela',
	[
		'primeiro_nome' => 'Kristian',
		'ultimo_nome' => 'Rose',
		'idade'	=> 19
	]);
```
Para ver a query SQL que foi executada, use o método `getSQL()` como este:
```php
echo $db->getSQL();
```
Output :
```sql
INSERT INTO `tabela` (`primeiro_nome`, `ultimo_nome`, `idade`) VALUES (?, ?, ?)
```
#### Obtenha o último ID inserido:
Você pode obter o último ID inserido usando o método `lastId()`, ou pode obter o retorno do método `insert()` como este:
```php
$lastID = $db->insert('tabela',
	[
		'primeiro_nome' => 'Kristian',
		'ultimo_nome' => 'Rose',
		'idade'	=> 19
	]);
echo $lastID;
```
E aqui está como usar `lastId()` depois de usar o método `update ()`:
```php
echo $db->lastId();
```
###Atualizar valores da tabela
Para atualizar a tabela, use o método `update()` que contém 3 parâmetros: o primeiro é o nome da tabela, o segundo é um array associativo dos valores da tabela que você deseja atualizar e o terceiro parâmetro é opcional, você pode usá-lo para indicar a condição de atualização como a cláusula WHERE no SQL.
A classe DB fornece tantas maneiras de fazer as mesmas consultas, por exemplo: o terceiro parâmetro no método `update()` você pode fazer um dos seguintes métodos:
####Passando o id
Você pode passar o `$id` como um terceiro parâmetro e a classe DB entenderá que há um campo na tabela chamado`id` e você deseja atualizar o registro que seu id é o valor de `$id` assim:
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Cute',
		'ultimo_nome' => 'Boy',
		'idade'	=> 22
	],1);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ?
```
mas, e se o nome da coluna não fosse id?
#### Passando o nome da coluna e valor
você pode passar um array de dois itens para o método `update` como um terceiro parâmetro: o primeiro item do array é o nome da coluna e o segundo item é o valor da coluna. O método `update()` na classe DB entenderá que você deseja atualizar a tabela onde o nome da coluna é igual ao valor. Como isso :
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'e',
		'ultimo_nome' => 'Girl',
		'idade'	=> 16
	],['id',1]);
```
SQL Query :
```sql
UPDATE `mytable` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ?
```
mas, e se precisarmos usar outro operador?
#### Passando nome da coluna, operador e valor
Você pode passar um array de três itens para o método `update()` como um terceiro parâmetro. O primeiro item do array é o nome da coluna como string, o segundo é o operador como string e o terceiro item é o valor, como isso :
```php
$db->update('tabela',
	[ 
		'primeiro_nome' => 'e',
		'ultimo_nome' => 'Boy',
		'idade'	=> 17
	],['idade','>',17]);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`idade` > ?
```
você também pode fazer a mesma query por apenas 2 itens na array, como este:
```php
$db->update('mytable',
	[ 
		'primeiro_nome' => 'Kris',
		'ultimo_nome' => 'Brabo',
		'idade'	=> 19
	],['idade >= ',19]);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE idade >= ?
```
mas, e se quisermos adicionar mais de uma condição where?
#### passando mais de uma condição where
Você pode passar um array de arrays (array aninhado) como um terceiro parâmetro para o método `update()`, cada array contém três itens: o nome da coluna como uma string, o operador e o valor. O segundo e o terceiro itens são opcionais, então você pode passar apenas o id como um array, ou você pode passar um array de dois itens: o nome da coluna e o valor. E aqui estão alguns exemplos de passagem de uma array:
##### Exemplo 1 : 
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Vanderson',
		'ultimo_nome' => 'Oliveira',
		'idade'	=> 19
	],[ [1] ]);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ?
```
##### Exemplo 2 : 
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Miury',
		'ultimo_nome' => 'Lourenço',
		'idade'	=> 14
	],[ ['idade',18], [1] ]);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`idade` = ? AND `tabela`.`id` = ?
```
##### Exemplo 3 : 
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Chinêd',
		'ultimo_nome' => 'Alev',
		'idade'	=> 21
	],[ ['idade','>=', 18], [1] ]);
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`idade` >= ? AND `tabela`.`id` = ?
```
Ou você pode fazer `[ ['idade >= ', 21], [1] ]` para obter o mesmo resultado.
### Outra maneira de atualizar usando o método `where()`
O método `where()` contém três parâmetros, o segundo e o terceiro são opcionais, se você passou apenas um parâmetro, o método `where()` entenderá que existe um campo chamado id e você deseja atualizar a tabela onde seu id é igual a aquele parâmetro como este:
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Yamashita',
		'ultimo_nome' => 'Fofoo',
		'idade'	=> 14
	])->where(1)->exec();
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ?
```
Usamos o método `exec()` para executar a consulta, o que significa que você pode usar o método `getSQL()` para verificar a query antes de executá-la sem `exec()`.
Você pode usar mais de um método `where()` da mesma forma:
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'S0n1x',
		'ultimo_nome' => 'Rose',
		'idade'	=> 35
	])->where(1)->where('primeiro_nome','S0n1x')->exec();
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ? AND `tabela`.`primeiro_nome` = ?
```
Como você pode ver, se você fornecer o método where com 2 parâmetros, ele entenderá que você deseja atualizar a tabela onde o nome da coluna é o primeiro parâmetro onde é igual ao valor do segundo parâmetro. E também se você notou que o segundo onde se torna 'AND' na query.
```php
$db->update('tabela',
	[
		'primeiro_nome' => 'Alice',
		'ultimo_nome' => 'Ham',
		'idade'	=> 30
	])->where(1)->where('idade','>',20)->exec();
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`id` = ? AND `tabela`.`idade` > ?
```
Agora, e se quiséssemos adicionar OR à nossa cláusula where?
### Como usar o método `orWhere()`?
`orWhere()` atua exatamente como o método `where()` e leva os mesmos parâmetros, é como 'OR' em SQL e você pode usar os dois métodos juntos desta forma:
```php
$db->update('mytable',
	[
		'primeiro_nome' => 'Kris',
		'ultimo_nome' => 'Oli',
		'idade'	=> 21
	])->where('idade','<=',20)->orWhere(1)->exec();
```
SQL Query :
```sql
UPDATE `tabela` SET `primeiro_nome` = ?, `ultimo_nome` = ?, `idade` = ? WHERE `tabela`.`idade` <= ? OR `tabela`.`id` = ?
```
E você também pode passar um array de cláusulas where para `where()` ou `orWhere()` método como este:
```php
->where([ ['primeiro_nome', 'Flower'], ['idade >=', 18], [1] ])->exec();
```
SQL seria assim:
```sql
WHERE `primeiro_nome` = ? AND idade >= ? AND id = ?
```
Você também pode usar uma combinação dos métodos `where()` e `orWhere()` com uma única cláusula ou com um grupo de cláusulas where como este:
```php
->where([ ['primeiro_nome', 'Kristian'], ['idade >=', 18]])->where(1)->orWhere([ [5], ['ultimo_nome', 'Rose'] ])->exec();
```
SQL seria assim:
```sql
WHERE `primeiro_nome` = ? AND idade >= ? AND `id` = ? OR `id` = ? Or `ultimo_nome` = ?
```
Como você pode notar que você pode usar `where()` e `orWhere()` não apenas com o método `upadte()`, mas também com outros métodos de consulta como `delete()`, `update()` e `table() `.
### Excluir valores da tabela
use o método `delete()` para deletar linhas da tabela, ele contém 2 parâmetros, o primeiro é o nome da tabela e o segundo é opcional, ele atua exatamente como o terceiro parâmetro no método `update()` então, você pode passar apenas o id como valor inteiro, você pode passar um array do nome do campo e o valor, você pode passar um array do nome do campo e parâmetro e valor, você pode passar um array de arrays das cláusulas where. E aqui estão alguns exemplos de como usar o método `delete()`:
#### Exemplo 1 : 
```php
$db->delete('tabela',1);
```
SQL Query :
```sql
DELETE FROM `tabela` WHERE `tabela`.`id` = ?
```
#### Exemplo 2 : 
```php
$db->delete('tabela', ['primeiro_nome', 'Kristian']);
```
SQL Query :
```sql
DELETE FROM `tabela` WHERE `tabela`.`primeiro_nome` = ?
```
#### Exemplo 3 : 
```php
$db->delete('tabela', ['idade', '<', 18]);
```
SQL Query :
```sql
DELETE FROM `tabela` WHERE `tabela`.`idade` < ?
```
#### Exemplo 4 : 
```php
$db->delete('tabela', [ ['idade', '<', 18], [1] ]);
```
SQL Query :
```sql
DELETE FROM `tabela` WHERE `tabela`.`idade` < ? AND `tabela`.`id` = ?
```
####Using `where()` with `Delete()` :
Você pode usar `where()` e `orWhere()` com `delete()` assim:
```php
$db->delete('tabela')->where(1)->exec();
```
SQL Query :
```sql
DELETE FROM `tabela` WHERE `tabela`.`id` = ?
```
Para excluir todas as linhas da tabela:
```php
$db->delete('tabela')->exec();
```
SQL Query :
```sql
DELETE FROM `tabela`
```
###Seleção
Use o método `get()` para recuperar dados da tabela, mas você tem que definir a tabela primeiro usando o método `table()`, que leva o nome da tabela como único parâmetro como este:
```php
$rows = $db->table('tabela')->get();
```
SQL Query :
```sql
SELECT * FROM `tabela`
```
Ele retorna uma coleção chamada "FlowerCollection" você pode pensar nela como um array de objetos, cada objeto representa uma linha da tabela, então você pode usar `foreach` para lançar o array `$rows` e obter cada linha separadamente desta forma :
 ```php
foreach ($rows as $row) {
	echo "$row->primeiro_nome<br>";
}
```
Resultado : 

```plain
Kristian - Rose 
Cute - Boy 
e - Girl 
```
e você pode aplicar métodos em "FlowerCollection" tipo `first()`, `last()`, `toArray()`, `toJSON()`, `item()` e `list()` assim:
 ```php
$users = $db->table("users")->get()->toArray();
```
Para obter usuários como uma array
```php
$users = $db->table("users")->get()->toJSON();
echo $users;
```
Para obter usuários como JSON e se você apenas ecoar o resultado, a classe Flower DB é inteligente o suficiente para entender que você deseja retornar um JSON, portanto, você pode obter o mesmo resultado em uma única linha como esta:
```php
echo $db->table("users")->get();
```
Para imprimir a tabela de usuários como JSON
```php
echo $db->table("users")->get()->first();
```
Para imprimir a primeira linha na tabela de usuários como JSON
```php
echo $db->table("users")->get()->last();
```
Para imprimir a última linha na tabela de usuários como JSON
```php
echo $db->table("users")->get()->first()->primeiro_nome;
```
Para imprimir o primeiro nome do primeiro usuário, você também pode fazer assim :
```php
$primeiro_user = $db->table("users")->get()->first();
echo $primeiro_user->primeiro_nome;
```
Ou você pode fazer assim:
```php
$primeiro = $db->table("users")->get()->first()->toArray();
echo $first_user['primeiro_nome'];
```
Se você deseja obter uma linha específica de`FlowerCollection` use `item()` método e passe a chave do item assim:
```php
echo $db->table("users")->get()->item(0);
```
imprime a primeira linha na tabela de usuários como JSON

Se você quiser obter uma coluna específica de `FlowerCollection`, como se você quiser o token dos usuários do Firebase como uma matriz, use o método` list() `e passe o nome da coluna assim:
```php
print_r( $db->table("users")->get()->list('token') );
```
imprimir todos os tokens na tabela de usuários como uma matriz
#### Método `Qget ()`:
O método `Qget()` funciona exatamente como o método get, mas sem toda a funcionalidade `FlowerCollecton`, como imprimir o resultado como JSON e outros métodos como` toArray() `,` toJSON() `,` first() `,` last () `e` item () `. se você realmente se preocupa com o desempenho, `Qget()` é o que você precisa usar. E você pode usá-lo assim:
```php
$users = $db->table("users")->Qget();
foreach ($users as $user) {
	echo $user->primeiro_nome;
}
```
To print the result of `Qget()` as JSON just use `json_encode($Qget_result);` like this :
```php
$users = $db->table("users")->Qget();
echo json_encode($users);
``` 
#### `select()` Method : 
If you want to select a specific column(s) use `select()` method, it holds column names as a string parameter separated by `,` like this : 
```php
$rows = $db->table('mytable')->select('first_name, last_name')->get();
```
SQL Query :
```sql
SELECT `first_name`, `last_name` FROM `mytable`
```
#### `limit()` Method : 
The `limit()` method makes it easy to code multi page results or pagination, and it is very useful on large tables. Returning a large number of records can impact on performance. It takes two parameters the first one is used to specify the number of records to return. And the second one is optional to pass the offset. And you can use it like this : 
```php
$rows = $db->table('mytable')->limit(10)->get();
```
SQL Query :
```sql
SELECT * FROM `mytable` LIMIT 10
```
It will return the first 10 records.
```php
$rows = $db->table('mytable')->limit(10, 20)->get();
```
SQL Query :
```sql
SELECT * FROM `mytable` LIMIT 10 OFFSET 20
```
It will return only 10 records, start on record 21 (OFFSET 20).
#### Easy pagination with `paginate()` method : 
Now after using `paginate()` method, pagination has never been easier!. You can use `paginate()` method with all selection methods like `table()` and `select()` instead of `get()`, it takes two parameters : the first one is page number starting from 1 as integer and the second one is used to specify the number of records to return `paginate($page, $limit)` and you can use it exactly like `get()` method and here is an example of how you can use it : 
```php
$rows = $db->table('mytable')->paginate(2, 5);
```
That means we want to return only 5 records from the second page and it will return only 5 records, start on record 6 up to 10. To get more information about what is going on behind the scenes, use `PaginationInfo()` method for more details like this: 
```php
print_r( $db->paginationInfo() );
```
Output : 
```plain
Array
(
    [previousPage] => 1
    [currentPage] => 2
    [nextPage] => 3
    [lastPage] => 5
)
```
It will return an associative array of useful information you might need to know like the current, previous, next and last page. And if there's no previous or next page its value would be null.
#### `Qpaginate()` Method :
`Qpaginate()` method works exactly like `paginate()` method but without all `MareiCollecton` functionality like print the result as JSON and other methods like `toArray()`, `toJSON()`, `first()`, `last()` and `item()`. if you really care about performance `Qget()` is what you need to use. And you can use it like this :
```php
$rows = $db->table('mytable')->paginate(2, 5);
```
####Using `where()` and `orWhere()` with selection : 
You can use `where()` or `orWhere()` methods with selection like this : 
```php
$rows = $db->table('mytable')->where(1)->get();
```
SQL Query :
```sql
SELECT * FROM `mytable` WHERE `mytable`.`id` = ?
```
####Order the result set
you can use `orderBy()` method to order the result set by a column name, `orderBy($column_name, $order)` takes two parameters, the first one is the column name as string and the second one is optional and it takes only two values `ASC` which is the default value to order the result set by asccending order, or `DESC` to order the result set by descending order like this :
```php
$rows = $db->table('mytable')->orderBy('id', 'DESC')->get();
```
To order the result set in descending order by id.
And you can use more than orderBy together like this :
```php
$rows = $db->table('mytable')
           ->orderBy('id', 'DESC')
	   ->orderBy('age', 'ASC')
	   ->get();
```
and ofcourse as you use `orderBy()` with `get()`, you can also use it with `paginate()`, `limit()`, `Qget()` and `Qpaginate()` methods.
####Count selected rows
Use `getCount()` method to get the total number of rows returned of the last query. and you can use it after selection like this : 
```php
echo $db->getCount();
```
###Using Raw Queries : 
I bet that you asked what if I wanted to execute more complected queries?
that's why I created `query()` method, it holds three parameters the first one is SQL query as a string, and the second one is optional and it's for the values that you wanna pass to query as an array. And here is how you can use `query()` method : 
```php
$sql = "SELECT * FROM mytable WHERE id = ?";
$rows = $db->query($sql, [1]);
```
SQL Query :
```sql
SELECT * FROM mytable WHERE id = 1
```
if you want to get rid of all `MareiCollection` functionally just pass true as a third parameter like this :
```php
$sql = "SELECT * FROM mytable WHERE id = ?";
$rows = $db->query($sql, [1], true);
```
