---
layout: page
title: Introdução ao Controle de Versão com Git
subtitle: Colaborando
---
> ## Objetivos {.objectives}
>
> *   Explicar o que são repositórios remotos e por que eles são úteis.
> *   Explicar o que acontece quando um repositório remoto é clonado.
> *   Explicar o que acontece quando mudanças são obtidas e enviadas para um
>     repositório remoto.

Controle de versão começa a fazer falta quando começamos a colaborar com outras
pessoas. Já temos quase todas as ferramentas necessárias para fazer isso, só
falta aprendermos como copiar alterações de um repositório para outro.

Sistemas como Git permite trocar alterações entre dois repositórios quaisquer.
Na prática, entretanto, é mais fácil utilizar uma cópia como hub central e
mantê-lo em um servidor conectado à internet do que no notebook de alguém.
Grande parte dos programadores utilizam serviços de hospedagem como
[GitHub](https://github.com) ou [BitBucket](https://bitbucket.org) para
armazenar esses cópias. Vamos explorar os pros e os contras dessa abordagem no
final dessa lição.

Vamos começar por compartilhar as mudanças que fizemos no nosso projeto com o
mundo. Autentique-se no GitHub e clique no ícone no canto superior direito para
criar um novo repositório denominado `planetas`:

<img src="fig/github-create-repo-01.png" alt="Criando um novo repositório no GitHub (Passo 1)" />

Nomeie o seu repositório de "planetas" e selecione "Create Repository":

<img src="fig/github-create-repo-02.png" alt="Criando um novo repositório no GitHub (Passo 2)" />

Assim que o repositório for criado, GitHub irá mostrar uma página com uma URL e
algumas informações de como configurar seu repositório local:

<img src="fig/github-create-repo-03.png" alt="Criando um novo repositório no GitHub (Passo 3)" />

Esse procedimento realiza os seguintes comandos nos servidores do GitHub:

~~~ {.bash}
$ mkdir planetas
$ cd planetas
$ git init
~~~

Nosso repositório local ainda contem nosso trabalho anterior no arquivo
`marte.txt` mas o repositório remoto no GitHub não contem nenhum arquivo:

<img src="fig/git-freshly-made-github-repo.svg" alt="Freshly-Made GitHub Repository" />

O próximo passo é conectar esses dois repositórios. Fazemos isso transformando o
repositório no GitHub um repositório [remote](../../gloss.html#repository-remote)
para o repositório local. Na página inicial do repositório no GitHub encontra-se
a string necessária para identificá-lo:

<img src="fig/github-find-repo-string.png" alt="Where to Find Repository URL on GitHub" />

Selecione 'HTTPS' para alterar o [protocol](../../gloss.html#protocol) SSH para
HTTPS.

> ## HTTPS vs SSH {.callout}
>
> Utilizamos HTTPS porque ele não requer configurações adicionais.
> Depois do workshop você pode querer configurar acesso via SSH,
> que é um pouco seguro,
> seguindo um dos tutoriais disponibilizados por
> [GitHub](https://help.github.com/articles/generating-ssh-keys),
> [Atlassian/BitBucket](https://confluence.atlassian.com/display/BITBUCKET/Set+up+SSH+for+Git)
> e [GitLab](https://about.gitlab.com/2014/03/04/add-ssh-key-screencast/)
> (esse é um vídeo).

<img src="fig/github-change-repo-string.png" alt="Changing the Repository URL on GitHub" />

Copie essa URL do navegador, vá para seu repositório local e execute o comando:

~~~ {.bash}
$ git remote add origin https://github.com/vlad/planetas
~~~

Certifique-se de utilizar a URL do seu repositório ao invés da URL do Vlad: a
única diferença deve ser seu nome de usuário no lugar de `vlad`.

Você pode verificar que o comando anterior foi executado corretamente utilizando
`git remote -v`:

~~~ {.bash}
$ git remote -v
~~~
~~~ {.output}
origin   https://github.com/vlad/planetas.git (push)
origin   https://github.com/vlad/planetas.git (fetch)
~~~

O nome `origin` é o apelido local para seu repositório remoto: você pode
utilizar outro apelido no lugar se você desejar, mas `origin` é a opção mais
utilizada.

Uma vez que o apelido `origin` está configurado, o comando a seguir irá copiar
as alterações no repositório local para o repositório no GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 821 bytes, done.
Total 9 (delta 2), reused 0 (delta 0)
To https://github.com/vlad/planetas
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
~~~

> ## Proxy {.callout}
>
> Se a rede que você está conectado utiliza um proxy existe uma change do último
> comando ter falhado com "Could not resolve hostname" como mensagem de error.
> Para resolver esse problema você precisa informar ao Git sobre o proxy:
>
> ~~~ {.bash}
> $ git config --global http.proxy http://user:password@proxy.url
> $ git config --global https.proxy http://user:password@proxy.url
> ~~~
>
> Quando você se conectar em outra rede que não utiliza um proxy você terá que
> dizer para o Git desabilitar o uso do proxy utilizando
>
> ~~~ {.bash}
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~

> ## Gerenciadores de Senhas {.callout}
>
> Se seu sistema operacional possui um gerenciador de senha configurad,
> `git push` irá tentar utilizá-lo quando for necessário informar o usuário e
> senha. Se você desejar digitar seu usuário e senha no terminal ao invés do
> gerenciador de senha, digite
>
> ~~~ {.bash}
> $ unset SSH_ASKPASS
> ~~~
>
> Você talvez queira adicionar esse comando no final do seu `~/.bashrc`
> para que esse passe a ser o comportamento padrão.

Agora o seus repositórios locais e remotos estão no seguinte estado:

<img src="fig/github-repo-after-first-push.svg" alt="Repositório no GitHub após o primeiro push" />

> ## A opção '-u' {.callout}
>
> Normalmente você vai encontrar a opção `-u` em outras documentações
> disponíveis na internet. Ela está relacionada com conceitos apresentados na
> nossa lição intermediária e pode ser ignorada sem problemas.

Você também pode pegar as alterações no seu repositório remoto para o local:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
From https://github.com/vlad/planetas
 * branch            master     -> FETCH_HEAD
Already up-to-date.
~~~

Nesse caso, nenhuma modificação foi recebida porque os dois repositórios já
estavam sincronizados. Se alguém tivesse enviado alguma alteração para esse
repositório no GitHub, o comando anterior teria baixado as modificações para o
repositório local.

Para o próximo passo, iremos trabalhar em pares.
Escolha um dos seus repositórios no GitHub para utilizá-lo colaborativamente.

> ## Praticando sozinho {.callout}
>
> Se você estiver seguindo essa lição sozinho, antes de continuar abra um
> segundo terminal, altere o diretório corrente para outro diretório (e.g.
> `/tmp`). Esse segundo terminal irá representar seu colega que estaria
> trabalhando em outro computador. Você não precisa dar permissões para ninguém
> pois o seu "colega" vai ser você mesmo.

O dono do repositório que será utilizado precisa dar permissões de escrita para
a outra pessoa. No GitHub, selecione "Settings" no lado direito, depois
"Collaborators" e depois digite o usuário da sua dupla.

<img src="fig/github-add-collaborators.png" alt="Adding collaborators on Github" />

Quem *não* é dono do repositório que será utilizado deve mudar de diretório,
utilizando `cd`, de tal forma que `ls` não mais mostre o diretório `planetas` e
em seguida criar uma cópia do repositório que será utilizado no seu computador:

~~~ {.bash}
$ git clone https://github.com/vlad/planetas.git
~~~

Troque `vlad` pelo usuário do seu parceiro (o dono do repositório que será
utilizado).

`git clone` irá criar uma cópia local do repositório remoto.

<img src="fig/github-collaboration.svg" alt="After Creating Clone of Repository" />

Agora o novo colaborador pode fazer uma alteração na sua cópia do repositório:

~~~ {.bash}
$ cd planetas
$ nano plutao.txt
$ cat plutao.txt
~~~
~~~ {.output}
Também é um planeta!
~~~
~~~ {.bash}
$ git add plutao.txt
$ git commit -m "Algumas notas sobre Plutão"
~~~
~~~ {.output}
 1 file changed, 1 insertion(+)
create mode 100644 plutao.txt
~~~

e depois enviar as mudanças para o GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 306 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/vlad/planetas.git
   9272da5..29aba7c  master -> master
~~~

Note que não precisamos criar um repositório remoto chamado `origin`
pois Git faz isso automaticamente quando clonamos um repositório.
(Por é o motivo que utilizamos `origin` anteriormente
quando configuramos o repositório remoto manualmente).

Podemos baixar as mudanças agora disponíveis no GitHub no repositório original
em nossa máquina:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planetas
 * branch            master     -> FETCH_HEAD
Updating 9272da5..29aba7c
Fast-forward
plutão.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 plutão.txt
~~~

> ## Marcação de tempo no GitHub {.challenge}
>
> Crie um repositório no GitHub,
> clone-o,
> adicione um arquivo,
> envie essa mudança para o GitHub
> e olhe o horário, em inglês [timestamp](reference.html#timestamp),
> que as alterações foram feitas no GitHub.
> Como que o GitHub grava o tempo? E por que?
