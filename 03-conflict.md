---
layout: page
title: Introdução ao Controle de Versão com Git
subtitle: Conflitos
---
> ## Objetivos {.objectives}
>
> *   Explicar o que são conflitos e quando eles ocorrem.
> *   Resolver conflitos ocorridos durante um merge.

Logo que pessoas começam a trabalhar em paralelo alguém irá pisar no pé de outra
pessoa. Isso também pode acontecer com uma única pessoa: se trabalharmos no
mesmo arquivo do nosso notebook e do computador no laboratório, pode ser que
façamos mudanças diferentes em cada uma das cópias. Controle de versão ajuda a
gerenciarmos esses [conflitos](reference.html#conflitos) ao fornecer uma
ferramenta para [resolver](reference.html#resolver) a sobreposição de mudanças.

Para que possamos aprender como resolver conflitos precisamos, primeiro, criar
um. Atualmente o arquivo `marte.txt` corresponde, em todas as nossas cópias do
repositório `planeta`, a

~~~ {.bash}
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
~~~

Vamos adicionar uma linha na cópia no nosso diretório de usuário:

~~~ {.bash}
$ nano marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
Linha adicionada na cópia do Lobisomem.
~~~

e enviar essa mudança para o GitHub:

~~~ {.bash}
$ git add marte.txt
$ git commit -m "Adicionado linha em nossa cópia local"
~~~
~~~ {.output}
[master 5ae9631] Adicionado linha em nossa cópia local
 1 file changed, 1 insertion(+)
~~~
~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 352 bytes, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/vlad/planetas
   29aba7c..dabb4c8  master -> master
~~~

Agora vamos esperar nosso colaborador fazer uma alteração em sua cópia local
*sem* atualizar a cópia local com as últimas alterações disponíveis no GitHub:

~~~ {.bash}
$ nano marte.txt
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
Adicionado linha diferente na outra cópia.
~~~

Podemos salvar a alteração localmente:

~~~ {.bash}
$ git add marte.txt
$ git commit -m "Adicionado linha em minha cópia"
~~~
~~~ {.output}
[master 07ebc69] Adicionado linha em minha cópia
 1 file changed, 1 insertion(+)
~~~

mas o Git não irá deixar enviarmos nossas alterações para o GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
To https://github.com/vlad/planetas.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/vlad/planetas.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~

<img src="fig/conflict.svg" alt="The conflicting changes" />

Git detecta que as alterações em uma cópia local sobrepõe aquelas feitas em
outra cópia e previne de nós bagunçarmos nosso trabalho anterior. O que
precisamos fazer é pegar as mudanças no GitHub, fazer um
[merge](reference.html#merge) delas na cópia que estamos
trabalhando atualmente e depois enviá-las novamente. Vamos começar por baixar as
mudanças:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1)
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planetas
 * branch            master     -> FETCH_HEAD
Auto-merging marte.txt
CONFLICT (content): Merge conflict in marte.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~

`git pull` informa que existe um conflito e marca os conflitos nas linhas
afetadas:

~~~ {.bash}
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
<<<<<<< HEAD
Linha adicionada na cópia do Lobisomem.
=======
Adicionado linha diferente na outra cópia.
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
~~~

Nossas mudanças---aquelas em `HEAD`---é precedido por `<<<<<<<`.
Depois das nossas mudanças conflitantes, Git adiciona `=======` como um
separador das mudanças e maca o fim das alterações conflitantes baixadas do
GitHub com `>>>>>>>`. (O conjunto de letras e dígitos depois desse marcador
identifica a versão que acabamos de baixar.)

Agora depende de nós editar o arquivo para remover os marcadores e reconciliar
as alterações. Podemos fazer o que desejarmos: manter as alterações feitas nessa
cópia, manter as alterações feitas na outra cópia, escrever algo novo para
substituir ambas alterações ou removê-las. Vamos substituir ambas de modo que o
arquivo fique como:

~~~ {.bash}
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
Removemos o conflito nessa linha.
~~~

Para finalizar o merge, nos adicionamos o arquivo `marte.txt` e criamos uma
nova revisão:

~~~ {.bash}
$ git add marte.txt
$ git status
~~~
~~~ {.output}
# On branch master
# All conflicts fixed but you are still merging.
#   (use "git commit" to conclude merge)
#
# Changes to be committed:
#
#	modified:   marte.txt
#
~~~
~~~ {.bash}
$ git commit -m "Juntando alterações provenientes do GitHub"
~~~
~~~ {.output}
[master 2abf2b1] Juntando alterações provenientes do GitHub
~~~

Agora podemos enviar nossas alterações para o GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 697 bytes, done.
Total 6 (delta 2), reused 0 (delta 0)
To https://github.com/vlad/planetas.git
   dabb4c8..2abf2b1  master -> master
~~~

Git mantem o registro de quando realizamos um merge e desse modo não precisamos
corrigir novamente esse conflito quando um colaborador baixar as novas mudanças:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 2), reused 6 (delta 2)
Unpacking objects: 100% (6/6), done.
From https://github.com/vlad/planetas
 * branch            master     -> FETCH_HEAD
Updating dabb4c8..2abf2b1
Fast-forward
 marte.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

Como podemos verificar o arquivo `marte.txt` encontra na forma que salvamos após
o merge:

~~~ {.bash}
$ cat marte.txt
~~~
~~~ {.output}
Frio e seco, mas tudo é da minha cor favorita.
Duas luas pode ser um problema para o Lobisomem.
Mas a Múmia irá apreciar a falta de humidade.
Removemos o conflito nessa linha.
~~~

Não precisamos juntar as alterações novamente porque o Git sabe que alguém já
fez isso.

A habilidade de sistemas de controle de versão em resolver conflitos é uma das
razões pela qual vários usuários tendem a dividir seus programas e artigos em
vários arquivos ao invés de armazená-los em um único grande arquivo. Existe um
outro benefício: quando conflitos em um arquivo ocorrem com frequência isso é
uma indicação de que as responsabilidades não estão claras ou o trabalho precisa
ser melhor dividido.

> ## Resolvendo conflitos que você criou {.challenge}
>
> 1. Clone o repositório criado pelo seu instrutor.
> 2. Adicione um novo arquivo nele e altere um arquivo existente (seu instrutor
>    irá informar qual arquivo).
> 3. Quando requisitado pelo seu instrutor, baixe as mudanças do repositório para
>    criar um conflito e resolva-o.

> ## Conflitos em arquivos que não são texto plano {.challenge}
>
> O que o Git faz quando existe um conflito em uma imagem ou em um arquivo não
> texto que está armazenado no controle de versão?
