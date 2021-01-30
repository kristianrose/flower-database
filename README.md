# Bem-vindo ao banco de dados Flower Classe V 1.0 
A classe FlowerDB é uma classe construtora de query simples em PHP para aumentar sua produtividade, você não pode imaginar quanto tempo vai economizar se estiver usando essa classe! .
## Recursos
* Totalmente seguro:
Esta classe de banco de dados usa instruções preparadas por PDO para fornecer altos níveis de proteção contra ataques de injeção de SQL
* Fácil uso:
A sintaxe é muito simples e há muitas maneiras de fazer a mesma query, então você pode usar da maneira que quiser;)
* Bem documentado :
Tudo o que você quer saber sobre essa aula está aqui e muito bem organizado, para que você possa encontrar facilmente.

## Uso
Depois de baixar a classe [aqui](https://raw.githubusercontent.com/kristianrose/flower-database/main/DB.php) salve-o em seu diretório raiz e abra-o para ajustar as configurações básicas para sua conexão de banco de dados como host , nome do banco de dados, nome de usuário e senha do banco de dados. E você também pode definir facilmente seu ambiente de desenvolvimento atual para `development` or `production`.
```php
//ambiente de desenvolvimento atualambiente de desenvolvimento atual
"env" => "development",
//Localhost
"development" => [
					"host" => "localhost",
					"database" => "test",
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
### Atualizar valores da tabela
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
#### Usando `where()` com `Delete()` :
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
### Seleção
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
Para imprimir o resultado of `Qget()` como JSON, apenas use `json_encode($Qget_result);` como isso :
```php
$users = $db->table("users")->Qget();
echo json_encode($users);
``` 
#### `select()` Método:
Se você quiser selecionar uma(s) coluna(s) específica(s), use o método `select ()`, ele mantém os nomes das colunas como um parâmetro de string separado por `,` assim: 
```php
$rows = $db->table('tabela')->select('primeiro_nome, ultimo_nome')->get();
```
SQL Query :
```sql
SELECT `primeiro_nome`, `ultimo_nome` FROM `mytable`
```
#### `limit()` Método:
O método `limit()` facilita a codificação de resultados de páginas múltiplas ou paginação, e é muito útil em tabelas grandes. Retornar um grande número de registros pode afetar o desempenho. Leva dois parâmetros, o primeiro é usado para especificar o número de registros a serem retornados. E o segundo é opcional para passar o deslocamento. E você pode usá-lo assim: 
```php
$rows = $db->table('tabela')->limit(10)->get();
```
SQL Query :
```sql
SELECT * FROM `tabela` LIMIT 10
```
Ele retornará os primeiros 10 registros.
```php
$rows = $db->table('tabela')->limit(10, 20)->get();
```
SQL Query :
```sql
SELECT * FROM `tabela` LIMIT 10 OFFSET 20
```
It will return only 10 records, start on record 21 (OFFSET 20).
#### Paginação fácil com o método `paginat()`:
Agora, depois de usar o método `paginate ()`, a paginação nunca foi tão fácil !. Você pode usar o método `paginate ()` com todos os métodos de seleção como `table()` e `select()` em vez de `get()`, leva dois parâmetros: o primeiro é o número da página começando em 1 como inteiro e o segundo é usado para especificar o número de registros para retornar `paginate($page, $limit)` e você pode usá-lo exatamente como o método `get()` e aqui está um exemplo de como você pode usá-lo:
```php
$rows = $db->table('tabela')->paginate(2, 5);
```
Isso significa que queremos retornar apenas 5 registros da segunda página e ela retornará apenas 5 registros, começando no registro 6 até 10. Para obter mais informações sobre o que está acontecendo nos bastidores, use o método `PaginationInfo()` para mais detalhes como este:
```php
print_r( $db->paginationInfo() );
```
Resultado : 
```plain
Array
(
    [previousPage] => 1
    [currentPage] => 2
    [nextPage] => 3
    [lastPage] => 5
)
```
Ele retornará uma matriz associativa de informações úteis que você pode precisar saber, como a página atual, anterior, seguinte e última. E se não houver página anterior ou seguinte, seu valor seria nulo.
#### Método `Qpaginate()`:
O método `Qpaginate ()` funciona exatamente como o método `paginate ()`, mas sem toda a funcionalidade `FlowerCollecton`, como imprimir o resultado como JSON e outros métodos como` toArray() `,` toJSON() `,` first() `, `último()` e `item()`. se você realmente se preocupa com o desempenho, `Qget()` é o que você precisa usar. E você pode usá-lo assim:
```php
$rows = $db->table('tabela')->paginate(2, 5);
```
####Using `where()` and `orWhere()` with selection : 
You can use `where()` or `orWhere()` methods with selection like this : 
```php
$rows = $db->table('tabela')->where(1)->get();
```
SQL Query :
```sql
SELECT * FROM `tabela` WHERE `tabela`.`id` = ?
```
#### Ordenar o conjunto de resultados
você pode usar o método `orderBy()` para ordenar o conjunto de resultados por um nome de coluna, `orderBy ($column_name, $order)` leva dois parâmetros, o primeiro é o nome da coluna como string e o segundo é opcional e aceita apenas dois valores `ASC`, que é o valor padrão para ordenar o conjunto de resultados em ordem crescente, ou` DESC` para ordenar o conjunto de resultados em ordem decrescente como esta:
```php
$rows = $db->table('tabela')->orderBy('id', 'DESC')->get();
```
Para ordenar o conjunto de resultados em ordem decrescente por id.
E você pode usar mais do que orderBy juntos, assim:
```php
$rows = $db->table('tabela')
           ->orderBy('id', 'DESC')
	   ->orderBy('idade', 'ASC')
	   ->get();
```
e claro, como você usa `orderBy()` com `get()`, você também pode usá-lo com os métodos `paginate()`, `limit()`, `Qget()` e `Qpaginate()`.
#### Contar linhas selecionadas
Use o método `getCount ()` para obter o número total de linhas retornadas da última consulta. e você pode usá-lo após a seleção como este:
```php
echo $db->getCount();
```
### Usando Raw Queries:
Aposto que você perguntou e se eu quisesse executar mais consultas resolvidas?
é por isso que criei o método `query()`, ele contém três parâmetros o primeiro é a consulta SQL como uma string, e o segundo é opcional e é para os valores que você deseja passar para a consulta como um array. E aqui está como você pode usar o método `query()`:
```php
$sql = "SELECT * FROM tabela WHERE id = ?";
$rows = $db->query($sql, [1]);
```
SQL Query :
```sql
SELECT * FROM tabela WHERE id = 1
```
se você quiser se livrar de todos os `FlowerCollection` funcionalmente, apenas passe true como um terceiro parâmetro como este:
```php
$sql = "SELECT * FROM tabela WHERE id = ?";
$rows = $db->query($sql, [1], true);
```
