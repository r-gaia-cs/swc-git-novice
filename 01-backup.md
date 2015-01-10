---
layout: page
title: Introdução ao Controle de Versão com Git
subtitle: Uma Melhor Solução de Backup
---
> ## Objetivos {.objectives}
>
> *   Explicar os passos de inicialização e configuração necessários para
>     cada máquina e aqueles necessários para cada repositório.
> *   Passa pelo ciclo de modificação, adição e commit para um ou mais arquivos e
>     explicar onde a informação é salva em cada estágio.
> *   Identificar e utilizar o número de versão atribuído pelo Git.
> *   Comparar o arquivo atual com versões antigas.
> *   Restaurar versões antigas de arquivos.
> *   Configurar Git para ignorar arquivos específicos, e explicar porque em
> *   alguns casos é útil fazer isso.

Nós vamos começar explorando como o controle de versão pode ser utilizado para
manter o registro do que e de quando uma pessoa fez algo. Mesmo se você não
estiver colaborando com outros, controle de versão é muito melhor que:

<div>
  <a href="http://www.phdcomics.com"><img src="fig/phd101212s.gif" alt="Piled Higher and Deeper by Jorge Cham, http://www.phdcomics.com" /></a>
  <p>"Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com</p>
</div>

### Configurando

Na primeira vez que utilizamos Git em uma máquina, precisamos configurar algumas
coisas. A seguir encontra-se o que Drácula fez para configurar seu novo
notebook:

~~~ {.bash}
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
$ git config --global color.ui "auto"
$ git config --global core.editor "nano"
~~~

(Por favor, utilize seu nome e endereço de email ao invés do de Drácula e por
favor certifique-se que você escolheu um editor disponível no seu sistema, como
o `notepad` se estiver utilizando Windows).

Os commandos do Git são escritos como `git verbo`, onde `verbo` é o que
desejamos fazer. No caso anterior, estamos dizendo para o Git:

*   nosso nome e endereço de email,
*   para colorir a saída,
*   qual o nosso editor de texto favorito, e
*   que queremos utilizar essas informações globalmente (i.e., para todo
*   projeto).

Os quatro comandos anteriores só precisam ser executados uma vez: a opção (em
inglês denominada de *flag*) ``--global`` diz para o Git utilizar as
configurações para todo projeto na máquina atual.

> ## Proxy {.callout}
>
> Em alguns casos você precisa utilizar um proxy para se conectar a internet.
> Se esse for o seu caso você precisa informar o Git sobre o proxy:
>
> ~~~ {.bash}
> $ git config --global http.proxy proxy-url
> $ git config --global https.proxy proxy-url
> ~~~
>
> To disable the proxy, use
>
> ~~~ {.bash}
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~

### Criando um repositório

Uma vez que Git está configurado, podemos começar a utilizá-lo.
Vamos criar um diretório para nosso trabalho:

~~~ {.bash}
$ mkdir planetas
$ cd planetas
~~~

e dizemos para fazer do diretório um
[repositório](../../gloss.html#repository)&mdash;um lugar onde Git irá
armazenar as versões anteriores de nossos arquivos:

~~~ {.bash}
$ git init
~~~

Se utilizarmos `ls` para mostrar o conteúdo do diretório irá parecer que nada
foi feito:

~~~ {.bash}
$ ls
~~~

Mas se adicionarmos a opção `-a` para mostrar todos os arquivos, iremos ver que
Git criou um diretório oculto denominado `.git`:

~~~ {.bash}
$ ls -a
~~~
~~~ {.output}
.	..	.git
~~~

Git armazena informações sobre o projeto nesse subdiretório especial. Se
deletarmos ele iremos perder o histórico do projeto.

Podemos verificar que a configuração foi feita com sucesso requisitando o estado
do nosso projeto para o Git:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~

### Monitorando Alterações nos Arquivos

Vamos criar um arquivo chamado `marte.txt` que contém algumas notas sobre a
sustentabilidade de uma base no planeta vermelho. (Iremos utilizar um editor
chamado `nano` para editar o arquivo mas você pode utilizar o editor de sua
preferência. Em particular, ele não precisa ser o mesmo editor informado para o
Git).

~~~ {.bash}
$ nano marte.txt
~~~

Escreva o texto abaixo no arquivo `marte.txt`:

~~~
Frio e seco, mas tudo é da minha cor favorita.
~~~

`marte.txt` agora contem a seguinte linha:

~~~ {.bash}
$ ls
~~~
~~~ {.output}
marte.txt
~~~
~~~ {.bash}
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
~~~

Se verificarmos o estado do nosso projeto novamente, Git irá informar que ele
encontrou um novo arquivo:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	marte.txt
nothing added to commit but untracked files present (use "git add" to track)
~~~

A mensagem "untracked files" significa que existe um arquivo no diretório que
Git não está monitorando. Iremos dizer para o Git que ele deve fazê-lo
utilizando `git add`:

~~~ {.bash}
$ git add marte.txt
~~~

e então verificamos alteração na mensagem de estado:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   marte.txt
#
~~~

Git agora sabe que ele deve monitorar o arquivo `marte.txt` mas ele ainda não
salvou nenhuma mudança para a posterioridade como um commit. Para fazer isso
precisamos executar mais um comando:

~~~ {.bash}
$ git commit -m "Começando a pensar em Marte"
~~~
~~~ {.output}
[master (root-commit) f22b25e] Começando a pensar em Marte
 1 file changed, 1 insertion(+)
 create mode 100644 marte.txt
~~~

Quando executamos `git commit`, Git pega todas as mudanças que informamos
precisar ser salvas quando utilizamos `git add` e armazena uma cópia permanente
dentro do diretório especial `.git`. Essa cópia permanente é denominada
[revisão](../../gloss.html#revision) é brevemente identificada por `f22b25e`.
(Sua revisão pode ter um identificador diferente.)

Utilizamos a opção `-m` (de "mensagem") para salvar um comentário que irá nos
ajudar a lembrar depois o que fizemos e porque. Se apenas executarmos `git
commit` sem a opção `-m`, Git irá iniciar `nano` (ou o editor que tivermos
configurado no início) para que possamos escrever um comentário longo.

Se executarmos `git status` agora:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
nothing to commit, working directory clean
~~~

Git está dizendo que tudo está atualizado. Se desejarmos saber o que foi feito
recentemente podemos pedir ao Git que mostre o histórico do projeto utilizando
`git log`:

~~~ {.bash}
$ git log
~~~
~~~ {.output}
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Começando a pensar em Marte
~~~

`git log` lista todas as revisões salvas em um repositório na ordem cronológica
reversa. Essa lista inclui, para cada revisão, o identificador completo da
revisão (que inicia com os mesmos caracteres que o identificador curto impresso
pelo comando `git commit` anteriormente), o autor da revisão, quando ela foi
criada e o comentário dado à revisão quando ela foi criada.

> ## Onde estão minhas mudanças? {.callout}
>
> Se executarmos `ls` agora continuamos a encontrar apenas um arquivo chamado
> `marte.txt`. Isso deve-se ao fato do Git salvar as informações com
> histórico dos arquivos no diretório especial denominado `.git` mencionado
> anteriormente tal que nosso sitema de arquivos não fique cheio (e nós
> acidentalmente editemos ou removemos uma versão anterior.

### Alterando arquivos

Agora suponha adicionou algumas informações ao arquivo. (Novamente, editaremos o
arquivo utilizando o `nano` e utilizaremos o commando `cat` para mostrar o
conteúdo do arquivo; você pode utilizar outro editor e não precisa do comando
`cat`.)

~~~ {.bash}
$ nano marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
~~~

Quando executamos o comando `git status`, ele irá informar que um arquivo sendo
monitorado foi alterado:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   marte.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~

A última linha,
"no changes added to commit",
é importante e nos avisa que nenhuma das mudanças feitas será salvo na próxima
revisão. Embora tenhamos alterado o arquivo não informamos ao Git que queremos
salvar essas mudanças (que iremos fazer utilizando `git add`).
Para verificar as alterações nos arquivos utilizamos `git diff`, que irá mostrar
a diferença entre o estado atual dos arquivo e a última revisão salva:

~~~ {.bash}
$ git diff
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1 +1,2 @@
 Frio e seco, mas tudo é da minha cor favorita.
+Duas luas pode ser um problema para o Lobisomem.
~~~

A saída parecer criptografada porque na verdade é uma série de comandos dizendo
para programas como editores de texto e `patch` como reconstruir um arquivo
partindo do outro. Podemos quebrar essa saída em algumas partes:

1.  A primeira linha informa que Git utilizou o comando `diff` para comparar a
    versão antiga com a nova.
2.  A segunda linha informa exatamente quais
    [revisões](../../gloss.html#revision) Git está comparando:
    `df0654a` e `315bf3a` são identificadores únicos gerados pelo computador
    para essas duas revisões.
3.  As linhas restantes mostram o que realmente mudou e as linhas
    correspondentes. Em particular, o sinal `+` na primeira coluna indica onde
    adicionamos novas linhas.

Vamos salvar nossas mudanças:

~~~ {.bash}
$ git commit -m "Preocupações decorrentes das luas de Marte"
~~~
~~~ {.output}
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   marte.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~

Hoops: Git não salvou uma nova revisão porque esquecemos de utilizar o comando
`git add` primeiro. Vamos corrigir isso:

~~~ {.bash}
$ git add marte.txt
$ git commit -m "Preocupações decorrentes das luas de Marte"
~~~
~~~ {.output}
[master 34961b1] Preocupações decorrentes das luas de Marte
 1 file changed, 1 insertion(+)
~~~

Git insiste que adicionemos os arquivos ao grupo a ser salvo antes de realmente
criarmos uma nova revisão porque podemos não querer incluir todas as mudanças de
uma vez. Por exemplo, suponha que estejamos adicionando algumas citações ao
trabalho de nosso orientador na nossa tese. Pode ser que desejamos ter uma
versão em que adicionamos as citações e as referências bibliográficas mas
**não** desejamos incluir as mudanças na conclusão uma vez que ainda não
terminamos esta.

Para que isso seja possível, Git possui uma área temporária especial (em inglês
denominada *staging area*) onde ele mantem o registro das alterações que foram
adicionadas ao [conjunto](../../gloss.html#change-set) a ser utilizado para o
próximo commit (que ainda não foi feito). `git add` coloca as modificações nessa
área e `git commit` move a informação dessa área para o armazenamento de longo
termo na forma de um commit.

<img src="fig/git-staging-area.svg" alt="The Git Staging Area" />

Vamos verificar como nossas mudanças são transmitidas do nosso editor para a
área temporária e posteriormente para o armazenamento de longo termo. Primeiro,
precisamos adicionar uma nova linha ao arquivo:

~~~ {.bash}
$ nano marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
~~~
~~~ {.bash}
$ git diff
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1,2 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
 Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~

Até agora, tudo bem: adicionamos uma nova linha no final do arquivo
(identificada com o sinal `+` na primeira coluna). Agora vamos colocar essa
mudança na área temporária e verificar o que `git diff` informa:

~~~ {.bash}
$ git add marte.txt
$ git diff
~~~

Não existe saída pois até onde o Git consegue informar não existe diferença
entre o que foi pedido para salvar permanentemente e o arquivos no repositório.
Entretanto, se pedirmos:

~~~ {.bash}
$ git diff --staged
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1,2 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
 Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~

será mostrado a diferença entre o último commit e as mudanças na área
temporária. Vamos salvar nossa mudança:

~~~ {.bash}
$ git commit -m "Pensamentos sobre o clima"
~~~
~~~ {.output}
[master 005937f] Pensamentos sobre o clima
 1 file changed, 1 insertion(+)
~~~

e verificar o estado do repositório:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
nothing to commit, working directory clean
~~~

e também no histórico do que foi feito até agora:

~~~ {.bash}
$ git log
~~~
~~~ {.output}
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Pensamentos sobre o clima

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Preocupações decorrentes das luas de Marte

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Começando a pensar em Marte
~~~

### Explorando o histórico

Se desejarmos ver o que alteramos, podemos utilizar `git diff` novamente, mas
referindo-se a versões antigas utilizando a notação `HEAD~1`, `HEAD~2` e assim
por diante:

~~~ {.bash}
$ git diff HEAD~1 marte.txt
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1,2 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
 Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~
~~~ {.bash}
$ git diff HEAD~2 marte.txt
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
+Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~

Dessa forma, criamos uma sequência de revisões. A revisão mais recente nessa
sequência é referenciada por `HEAD` e podemos referenciar revisões anteriores
utilizando a notação com `~`, tal que `HEAD~1` (pronuncia-se "*head minus one*")
significa a revisão anterior, enquanto `HEAD~123` retorna 123 revisões do ponto
em que estamos agora.

Podemos também referenciar revisões anteriores utilizando a longa string de
dígitos e letras impressas por `git log`. Essa longa string é única para as
revisões e "única" realmente significa única: todo conjunto de mudanças em um
conjunto de arquivos em cada máquina possui um identificador único de 40
caracteres. O nosso primeiro commit possui como identificado
f22b25e3233b4645dabd0d81e651fe074bd8e73b.
Então vamos tentar:

~~~ {.bash}
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b marte.txt
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
+Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~

A resposta do Git está correta mas digitar 40 caracteres aleatórios é
inconveniente e por isso Git permite você digitar apenas os primeiros
caracteres:

~~~ {.bash}
$ git diff f22b25e marte.txt
~~~
~~~ {.output}
diff --git a/marte.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/marte.txt
+++ b/marte.txt
@@ -1 +1,3 @@
 Frio e seco, mas tudo é da minha cor favorita.
+Duas luas pode ser um problema para o Lobisomem.
+Mas a Múmia irá apreciar a falta de humidade.
~~~

### Recuperando versões anteriores

Até agora aprendemos como salvar alterações nos arquivos e verificar as
alterações realizadas. Como podemos recuperar um arquivo de uma versão antiga?
Vamos supor que acidentalmente sobre escrevemos um de nossos arquivos.

~~~ {.bash}
$ nano marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Teremos que produzir oxigênio para nosso consumo.
~~~

`git status` irá informar que os arquivo foi alterado e que as alterações não
foram salvas na área temporária:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   marte.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~

Podemos desfazer as mudanças utilizando o comando `git checkout`:
We can put things back the way they were by using `git checkout`:

~~~ {.bash}
$ git checkout HEAD marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
~~~

Como você pode adivinhar pelo verbo utilizado, `git checkout` *checks out*
(i.e., restaura) uma versão anterior do arquivo. Nesse caso, estamos dizendo
para o Git que queremos recuperar a versão do arquivo presente em `HEAD`, que
corresponde a última versão salva. Se você você resolver resolver voltar para
uma versão mais antiga você deve utilizar o identificado da respectiva versão:

~~~ {.bash}
$ git checkout f22b25e marte.txt
~~~

É importante lembrar que
devemos utilizar o identificador da revisão
*anterior* ao estado que desejamos desfazer.
Um erro comum é utilizar o identificador da
revisão na qual as alterações indesejadas foram feitas.
No exemplo abaixo, queremos recuperar o estado anterior
ao commit mais recente (`HEAD~1`), cuja identificação é `f22b25e`:

<img src="fig/git-checkout.svg" alt="Git Checkout" />

O diagrama a seguir ilustra como o histórico de um arquivo deve ser
(indo para antes de `HEAD`, a versão mais recente salva):

<img src="fig/git-when-revisions-updated.svg" alt="When Git Updates Revision Numbers" />

> ## Simplificando o Caso Comum {.callout}
>
> Se você tiver lido cuidadosamente a saída do comando `git status` você terá
> notado que ele encontra a seguinte dica:
>
> ~~~ {.bash}
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
>
> Como ela diz, `git checkout` irá restaurar os arquivos para o estado salvo em
> `HEAD`. O traço duplo, `--`, é necessário para separar o nome do arquivo a ser
> recuperado do comando propriamente dito: sem o traço duplo, Git tentará
> utilizar o nome do arquivo como o identificador da revisão.

O fato de que os arquivos pode ser recuperados um por um tende a mudar a forma
como as pessoas organizam seu trabalho. Se todo o trabalho consiste de um grande
documento, será difícil (mas não impossível) de desfazer alguma mudança sem
também desfazer outras, por exemplo desfazer as alterações na introdução sem
também desfazer as alterações feitas na conclusão. Se a introdução e conclusão
estiverem salvas em arquivos separados será muito mais fácil desfazer apenas as
alterações em um dos arquivos.

### Ignorando Coisas

Se tivermos arquivos que não desejamos serem monitorados pelo Git, por exemplo
arquivos de backup criados pelo nosso editor ou arquivos intermediários criados
durante a análise de dados. Vamos criar um exemplo bobo:

~~~ {.bash}
$ mkdir resultados
$ touch a.dat b.dat c.dat resultados/a.out resultados/b.out
~~~

e verificar o que o Git diz:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	a.dat
#	b.dat
#	c.dat
#	resultados/
nothing added to commit but untracked files present (use "git add" to track)
~~~

Colocando esses arquivos sob controle de versão é um desperdício de memória em
disco. Algo pior é que ter eles listados toda vez pode reduzir nossa atenção
para as mudanças que realmente importam. Vamos então dizer para o Git ignorar
alguns arquivos.

Fazemos isso criando um arquivo denominado `.gitignore` no diretório raiz do
nosso projeto.

~~~ {.bash}
$ nano .gitignore
$ cat .gitignore
~~~
~~~ {.output}
*.dat
resultados/
~~~

A primeira expressão no arquivo `.gitignore` diz para o Git ignorar todos os
arquivos que terminam com `.dat` e a segunda expressão para ele ignorar todos os
arquivos dentro do diretório `resultados`. (Se algum desses arquivos já está
sendo monitorado pelo Git ele continuará sendo-o).

Uma vez que criamos esse arquivo, a saída do comando `git status` é muito mais
limpa.

~~~ {.bash}
$ git status
~~~
~~~ {.output}
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	.gitignore
nothing added to commit but untracked files present (use "git add" to track)
~~~

A única alteração que Git nota é a criação do arquivo `.gitignore`. Inicialmente
você pode pensar que você não quer monitorar esse arquivo mas todas as pessoas
que fizerem uso do repositório provavelmente irão querer ignorar os mesmos
arquivos que ignoramos. Por esse motivo, vamos adicionar o arquivo `.gitignore`
ao nosso controle de versão:

~~~ {.bash}
$ git add .gitignore
$ git commit -m "Adicionado gitignore"
$ git status
~~~
~~~ {.output}
# On branch master
nothing to commit, working directory clean
~~~

Como um bônus, utilizar `.gitignore` irá ajudar nos a evitar de acidentadamente
adicionar arquivos indesejados.

~~~ {.bash}
$ git add a.dat
~~~
~~~ {.output}
The following paths are ignored by one of your .gitignore files:
a.dat
Use -f if you really want to add them.
fatal: no files added
~~~

Se realmente desejarmos desobedecer nossas configurações presentes no
`.gitignore` precisamos informar isso ao Git utilizando `git add -f`. Também
podemos verificar o status dos arquivos ignorados utilizando:

~~~ {.bash}
$ git status --ignored
~~~
~~~ {.output}
# On branch master
# Ignored files:
#  (use "git add -f <file>..." to include in what will be committed)
#
#        a.dat
#        b.dat
#        c.dat
#        resultados/

nothing to commit, working directory clean
~~~

> ## Repositório `bio` {.challenge}
>
> Crie um novo repositório Git no seu computador chamado `bio`. Escreva uma versão
> curta da sua bibliografia com três linhas no arquivo `me.txt`, salva suas
> mudanças. Depois, modifique uma das linhas e adicione uma quarta linha, mostre
> a alteração feita e desfaça-a.

> ## Onde posso criar meu repositório? {.challenge}
>
> A seguinte sequencia de comandos cira um repositório Git dentro de outro:
>
> ~~~ {.bash}
> cd           # retorna para sua pasta de usuário
> mkdir alpha  # cria um novo repositório
> cd alpha     # muda o diretório atual para o diretório recém criado
> git init     # transforma o diretório recém criado em um repositório Git
> mkdir beta   # cria um subdiretório
> cd beta      # muda o diretório atual para o subdiretório recém criado
> git init     # transforma o subdiretório em um repositório Git
> ~~~
>
> Por que utilizar um repositório Git dentro de outro é uma péssima idéia?
