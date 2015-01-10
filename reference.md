---
layout: page
title: Introdução ao Controle de Versão com Git
subtitle: Referência
---
## [Uma Melhor Solução de Backup](01-backup.html)

*   Usar `git config` para configurar
    o nome de usuário, endereço de email, editor e outras preferências em uma máquina.
*   `git init` inicializa um repositório.
*   `git status` mostra o estado de um repositório.
*   Arquivos podem ser armazenados no diretório de trabalho do projeto (que o usuário visualiza),
    na área intermediária (onde a próxima versão está sendo construída)
    e no repositório local (onde snapshots são salvos permanentemente).
*   `git add` move os arquivos para a área temporária.
*   `git commit` cria um novo snapshot da área temporária no repositório local.
*   Sempre escreva uma mensagem descritiva quando criar uma nova revisão.
*   `git diff` mostra a diferença entre revisões.
*   `git checkout` recupera uma versão anterior.
*   O arquivo `.gitignore` informa o Git quais arquivos devem ser ignorados.

## [Collaborating](02-collab.html)

*   Um repositório Git local pode ser conectado com um ou mais repositórios remotos.
*   Utilize o protocolo HTTPS para comunicar-se com um repositório remoto até que você aprenda como configurar autenticação via SSH.
*   `git push` copia as mudanças de um repositório local para um repositório remoto.
*   `git pull` copia as mudanças de um repositório remoto para um repositório local.
*   `git clone` copia um repositório remoto para criar um repositório local
    que possui um repositório remoto chamado de `origin` automaticamente configurado.

## [Conflicts](03-conflict.html)

*   Conflitos ocorrem quando duas ou mais pessoas alteram o mesmo arquivo ao mesmo tempo.
*   Um sistema de controle de versão não deixa um usuário sobre-escrever as alterações de um outro usuário cegamente.
    Ao contrário, ele destaca os conflitos para que estes possam ser resolvidos.

## [Ciência Aberta](04-open.html)

*   Trabalhos científicos são mais úteis e mais citados que trabalhos
    científicos fechados.
*   Pessoas que incorporam software GPL em seus softwares devem torná-los
    abertos utilizando a GPL;
    a maioria das outras licenças não requer isso.
*   A família de licenças Creative Commons permite que pessoas misturem
    requerimentos e restrições de atribuição,
    criação de trabalhos derivados,
    compartilhamento,
    e comercialização.
*   Pessoas que não são advogados não devem tentar escrever licenças começando do zero.
*   Projetos podem ser hospedados nos servidores da universidade,
    em um domínio pessoal,
    ou em repositórios públicos.
*   Regras relacionadas com a propriedade intelectual e armazenamento de informações sensíveis são válidas
    independentemente de onde o código e o dado são hospedados.

## Glossário

conjunto de alterações
:   Um grupo de alterações em um ou mais arquivos
    que são [salvos](#commit) em um [repositório](#repositório) sob [controle de versão](controle-de-versão)
    em uma única operação.

commit
:   Ação de salvar o estado atual de um conjunto de arquivos
    (um [conjunto de alterações](#conjunto-de-alterações))
    em um [repositório](#repositório) sob [controle de versão](#controle-de-versão).
    Se um commit contem mudanças em vários arquivos,
    todas as alterações são salvas em conjunto.

conflito
:   Uma alteração feita por um usuário do [sistema de controle de versão](#controle-de-versão)
    que é incompatível com as alterações feitas por outros usuários.
    Auxiliar usuários a [resolver](#resolver) conflitos
    é uma das maiores tarefas de um sistema de controle de versão.

HTTP
:   O [Protocolo](#protocolo) Hypertext Transfer usado para compartilhar páginas da internet
    e outros dados
    na World Wide Web.

licenças infecciosas
:   Uma licença como a [GPL](http://opensource.org/licenses/GPL-3.0)
    que obriga as pessoas que incorporam material sob essa licença em seu trabalho
    a utilizar a mesma licença ou uma com requerimentos similares.

merge
:   (um repositório):
    Reconciliar dois conjuntos de alterações em um [repositório](#repositório).

protocolo
:   Um conjunto de regras que definem como um computador se comunica com outro.
    Protocolos populares na Internet incluem [HTTP](#http) e [SSH](#ssh).

remoto
:   Um [repositório](#repositório) sob controle de versão que não o atual
    que de alguma forma é um espelho ou está conectado com o atual.

repositório
:   Uma área de armazenamento onde um sistema de [controle de versão](#controle-de-versão)
    armazena [revisões](#revisão) anteriores dos arquivos
    e informações sobre quem, quando e onde criou uma nova [revisão](#revisão).

resolver
:   Eliminar os [conflitos](#conflito) entre duas ou mais alterações incompatíveis
    em um arquivo ou um conjunto de arquivos
    que estão sendo gerenciados por um sistema de [controle de versão](#controle-de-versão).

revisão
:   Um registro do estado de um [repositório](#repositório) de [controle de versão](#controle-de-versão).

SSH
:   O [protocolo](#protocolo) Secure Shell utilizado para a comunicação segura entre computadores.

timestamp
:   O registro de quando um evento em particular ocorreu.

controle de versão
:   Uma ferramenta para gerenciar alterações em um conjunto de arquivos.
    Cada conjunto de mudanças cria uma nova [revisão](#revisão) dos arquivos;
    o sistema de controle de versão permite os usuários recuperarem versões anteriores de forma segura,
    e ajuda a gerenciar conflitos entre alterações feitas por diferente usuários.
