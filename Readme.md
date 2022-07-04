# App Help-Desk 
## Primeiros passos pra criaçao da logica e aplicação

1. Criaçao do form via bootstrap com class card
2. validaçao do login via action que chamando o 'valida_login.php' e sua logica 

* Usando atributo 'name' passa receber o valor contido nos campos do form, as informaçoes serão encapsuldas pelo browser e serão encaminhadas pro servidor/navegador a partir da propria url como parametro - add '?' na url especificando os parametros q serao recebidos

3. Usando variavel superglobal $_GET - essa super global é um array associativo, (query string). O array é formado pelos dados recebidos pelo HTTP GET $_GET
    __________________________________________________________________________
    *Varias variaveis são superglobais, sendo assim, variaveis q nao precisam de declaraçao e estao disponiveis em todo escopo para todo script, nao há necessidade de fazer **global $variavel**, sendo chamada por '$_' seguido da variavel em letras maiusculas, variaveis superglobais $GLOBALS $_SERVER $_GET, $_POST, $_FILES,$_COOKIE, $_SESSION, $_REQUEST, $_ENV*
    __________________________________________________________________________
    ps: a forma de receber os parametros via usuario, e enviando para um servidor e encapsulando, retornando como array/html, é oq transforma um site estatico em sites dinamicos, deixando assim facil ediçao e manutençao sem verbalizar tags e exigencia de conhecimento html pra produçao de sites como por exemplo de noticias .  

* Metodo superglobal $_POST anexando os dados do formulario dentro da propria requisiçao, retirando da url - usando metodo 'method = "post"' - alterando o metodo $_GET para $_POST  

    __________________________________________________________________________
    Um array associativo de variáveis passados para o script atual via método HTTP POST quando utilizado application/x-www-form-urlencoded ou multipart/form-data como valor do cabeçalho HTTP Content-Type na requisição.
    __________________________________________________________________________

## Autenticando o Usuario *valida_login.php*
**Criando um "banco de dados" advinto de um array criado, por ser um projeto de aprendizado, nao sera usado nenhum banco de dados e os usuarios serão pré-cadastrado via array**

*Verifica se email e senha correspondem a um usuario valido*
* Se sim, informa que o usuario é autendicado
* Se não, retorna para o index.php e apresenta o erro de usuario ou senha 
* Criando uma variavel que recebe retorno 'false' e verifica se a autenticaçao foi realizada - *$usuario_autenticado = false*
* Usando 'foreach' para percorrer o array recebido nos metodos action/name/$_POST () - foreach($usuario_app as $user) -> foreach precisa de 1 condiçao pre estabelicida e um array para comparar. 
* Usando 'if' para comparar se o email/senha digitado é igual ao email/senha recebido no $_POST - e caso seja verdadeiro a variavel *'$usuario_autenticado = true'*
* Usando 'else' para estabelecer uma msg de error e um retorno pra pagina de login com a funçao nativa 'header' 

    *header = header() é usado para enviar um cabeçalho HTTP bruto*
* Usando funçao nativa 'isset()' essa funçao vai verificar se um determinado indice de um determinado array, esta setado ou definido, verificar a existencia desse indice antes de tentar utilizar.
* Verificar via isset() se o indice login ta setado na superglobal $_GET e se o mesmo indice dentro da superglobal $_GET possui valor igual 'erro'.



____

## Protegendo Paginas restristas da aplicaçao com SESSION - *valida_login.php*
### Utilizando a variavel de sessão PHP
*Em um site ou sistema web, a sessão é importante quando se quer mais segurança na página ou quando se quer ter um controle de usuário. Também alguns programadores utilizam-se deste recurso para guardar informações e também pode-se montar um carrinho de compra de um site de vendas, pois assim vão armazenando-se os itens ou produtos e só no final é que os dados são jogados no banco de dados.*

___
”A variável de sessão PHP é usada para armazenar informações sobre, ou alterar as configurações do sistema ou site para uma sessão de usuário. As variáveis ​​de sessão armazenam informações sobre um único usuário e estão disponíveis para todas as páginas em um único aplicativo”.
___

1. Iniciar com 'session_start()', fundamentalmente importante que seja antes de qualquer impressão de dados no navegador. Por padrão é colocada como primeira instruçao do cod php.
*Iniciando cm session_start(), disponibiliza a superglobal $_SESSION, um array associativo contendo variaveis da sessão disponivel, os scripts criado em determinada sessão pode ser compartilhada entre paginas restritas e o servidor e vise-versa*
2. Na condiçao 'if' que testa se o usuario é autenticado ou não, criar um $_SESSION com indice ['autenticado']. 
3. Transferir o cod que recebe essa sessão em todas paginas restritas, chamando a variavel $_SESSION['autenticado'].
4. Forçar o redirecionamento das paginas restristas, caso o indice da superglobal $_SESSION nao seja autenticada, caso a variavel nao seja autenticada ou iniciada correntamente, impedindo o acesso sem ser liberado cm login e senha - Usando a funçao nativa header() transferindo para a pagina index.php

__
## Incorporando scripts com include, include_once, require e require_once
*include, require são metodos de inclusao e requisiçao de scripts de outros codigos em outras paginas, evitando a redundancia de codigos e dificultando a manutençao, sao metodos e podem ser repetidos quantas vezes forem chamados, diferente do include_once e require_once, o php tem inteligencia de observar que o codigo ja foi incluido uma vez e nao repete o cod*
**Refactoring do projeto com require_once**

* Criando um script validador_acesso.php para recebimento do cod php de validaçao de acesso, e chamando o cod nas paginas que vao usar esse cod.
Opção do require_once, pro caso de aconteça qlqr erro na validação, ocorra um fatal error.

## Navegando entre as paginas do Help Desk

* Forçar o redirecionamento no script valida_login.php, sendo assim encaminhado caso esteja correto o login, para pagina home.php
* Encaminhar links paras imagens usada no home.php, redirecionando caso clique, para abrir_chamado.php ou consultar_chamado.php
* Transformar os buttons do script abrir_chamado.php em links que retornam para pagina home.php

# Encerrando a sessão (logoff)
Encerrar a sessao é uma forma de proteger a sessão e deixar de ficar validada por um tempo default.
* Criar uma 'ul' e um 'li' cm link 'a' que redireciona para um script de logoff.
* Criando um script loggoff.php para destruir os indices da session ou uma variavel. usando metodo session_destroy(), é destruido todos indices recuperado da superglobal $_SESSION
* Iniciar a sessão utilizando o session_destroy, depois forçar um redirecionamento para o script index.php

*Destruindo todas variaveis da sessao,usando session_destroy(); em uma unica sessao, os efeitos dessa destruiçao, ficam disponivel apos o usuario entrar novamente*

# Registrando Chamados - *abrir_chamador.php*
## **Criando os chamados arquivos txt**

*Recuperar os dados do formulario via requisição http, encaminhar para o servidor, fazendo esse envio utilizando o metodo $_GET ou $_POST, do lado do servidor, inteceptar essas informaçoes, abrir escrever, e fechar um arquivo .txt, sendo armazenada no servidor e guardadas para serem utilizada novamente*

* Usando funçao nativa fopen()

*PHP | fopen() (função abrir arquivo ou URL) A função fopen() em PHP é uma função embutida que é usada para abrir um arquivo ou URL. Ele é usado para vincular um recurso a um vapor usando um nome de arquivo específico.*
___
Parametros do *fopen* 
- 'r' - abre somente para leitura, coloca o ponteiro do arquivo no começo
- 'r+' abre para leitura e escrita
- 'w' abre somente para escrita, coloca o ponteiro do arquivo no começo e reduz o comprimento do arquivo para zero
- 'w+' Abre para leitura e escrita; coloca o ponteiro do arquivo no começo do arquivo e reduz o comprimento do arquivo para zero. 
- 'a' abre somente para escrita. coloca o ponteiro no final do arquivo
- 'a+' abre para leitura e escrita
- 'x' cria e abre o arquivo somente para leitura.  Se o arquivo já existir, a chamada a fopen() falhará, retornando false e gerando um erro de nível E_WARNING. Se o arquivo não existir, tenta criá-lo. Isto é equivalente a especificar as flags O_EXCL|O_CREAT para a chamada de sistema open(2).
- 'x+' cria e abre o arquivo para leitura e escrita Se o arquivo já existir, a chamada a fopen() falhará, retornando false e gerando um erro de nível E_WARNING. Se o arquivo não existir, tenta criá-lo. Isto é equivalente a especificar as flags O_EXCL|O_CREAT para a chamada de sistema open(2).
____

* Transformando o array recebido pelo $_POST, em um texto pra ser encaminhado em forma de txt, retornando e concatenando os indices e armazenando dentro de uma variavel 

* Usando funçoes nativas do php, fwrite e fclose - fwrite () -> função que recebe 2 parametros, que receb como referencia arquivo aberto, e oq deve ser escrito no arquivo -fclose() -> passando a referencia do arquivo aberto e fechando txt.

* Usando uma constante PHO_EOL, armazenda o caracter de quebra de linha e indicando quando um arquivo acaba ou começa 

# Consultando Chamados - *consultar_chamado.php*

* Implementar a tela de consulta de chamados, fazer uma requisiçao do arquivo.hd, o arquivo onde estamos armazenano os dados, e ler esses arquivos, retornando uma pagina web criada de forma dinamica.
* Abrindo o arquivo cm a funçao _fopen_ e usando parametro 'r'
* Percorrer o arquivo enquanto houver registros (linhas) a serem recuperados, usando o _while_
* Usando a funçao nativa _feof — Testa pelo fim-de-arquivo (eof) em um ponteiro de arquivo_, para percorrer todas linhas do arquivo.
* Usando funçao nativa _fgets — Lê uma linha de um ponteiro de arquivo_ Retorna uma linha de ponteiro do arquivo


___
O a funçao _feof_ vai ler e retornar 'true' caso ela encontre o final do aquivo, tem q usar  operador '!' para inverter o retorno do laço, para que retorne false e entre no laço, apos ler todas as linhas e chegar no final, podemos ler e recuperar essas linhas, usando a funçao _fgets_ para ler a linha que o cursor estiver no começo da linha, ele recupera toda linha baseada em bits, ou ate chegar na quebra de linha 
___

* Guardando e recuperando os dados do arquivo.hd, criando um array para receber os dados recebidos da funçao _fgets_.
* Usando o _foreach_ no escopo da codificaçao html, para criar uma repetiçao dinamica do card.
* Usando o metodo _explode_ que divide uma string em strings, retorna uma matriz de strings, como cada substring, usando por um delimitador, no nosso caso seria o '#', transformando numa array, sendo assim possivel encaixar cada string na posiçao escolhida.
* Usando 'if' com a funçao count() para definir o limite de indices no array e parar a execução do _foreach_.
* Deixando dinamico os cards com o array recuperado do _explode_

# Aplicando controle de perfil de usuario - *valoda_login.php*

___
Controlando a visibilidade dos chamados.
Usuario administrativos visualizam todos os chamados, usuarios comuns, visualizam somente seus chamados.
___

* Criando novos usuarios autenticados para limitar o acesso de cada usuario
* Criando uma id atribuida para cada usuario como indice de array.
* Recuperando e atribuindo cada id para cada usuario, por meio de variaveis e associaçao de indice/variavel, usando a funçao $_SESSION para definir e limitar o acesso de cada usuario, por meio de id.

**registra_chamado.php** - _iniciando uma session cm session_start(), atribuindo o $_SESSION['id'] na variavel $texto, que recebe e cria o arquivo.hd_
* Ajustar os indices do card dinamico *consultar_chamado.php*
* Criar array com indices associativos para Admistrativo e Usuario
* Incluir outro elemento no array usuarios do app, definindo o perfil admistrativo ou usuario de cada um
* Usar essa informaçao de perfil do usuario, de forma global, atribuindo a uma variavel e incluir na superglobal $_SESSION, recuperando a variavel e atribuindo o valor do indice do usuario autenticado, e atribuindo a um novo indice da superglobal, assim o id do usuario ficara de forma global.
* Implementar as regras de negocio, fazendo alguns testes do perfil do usuario, sendo ele 'admistrativo' ou 'usuario', limitando o acesso do mesmo

# Segurança no back-end de aplicaçoes web 
 
 ### Proteger arquivos sigilosos da nossa aplicação 
 Criando uma pasta no nosso servidor e deixando ela fora do diretorio publico 
 c:/xampp 
    - app_help_desk (sera diretorio de arquivos e scripts sigiloso)
    - htdocs/App_Help_Desk (diretorio publico)
Usando os comandos de inclusão _require_, e chamar os arquivos sigilos, usando token '..' vc sobe um diretorio dentro do diretorio q esta. 