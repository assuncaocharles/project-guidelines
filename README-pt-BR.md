[中文版](./README-zh.md)
| [日本語版](./README-ja.md)
| [한국어](./README-ko.md)
| [ENGLISH](./README.md)

[<img src="./images/elsewhen-logo.png" width="180" height="180">](http://elsewhen.co/)

# Padrões de Projeto &middot; [![PRs são bem vindos](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

> Enquanto desenvolver um novo projeto é apenas diversão para você, manter esse projeto pode ser um dos piores pesadelos para outra pessoa.
> Isso aqui é uma lista das padrões que encontramos, coletamos e escrevos que (para nós) funciona realmente bem com a maioria dos projetos JavaScript aqui na [elsewhen](http://elsewhen.co).
> Se você quer compartilhar alguma prática que considera importante ou acha que alguma das coisas descritas aqui deve ser removida, [Sinta se a vontade para nos dizer](http://makeapullrequest.com).

🔥 [Confira](https://github.com/elsewhencode/react-redux-saucepan) nosso [react redux projeto base](https://github.com/elsewhencode/react-redux-saucepan) em Flow com hot reloading e server-side rendering.

<hr>

- [Git](#git)
  - [Algumas regras do Git](#some-git-rules)
  - [Git workflow](#git-workflow)
  - [Escrevendo boas mensagens de commit](#writing-good-commit-messages)
- [Documentação](#documentation)
- [Ambientes](#environments)
  - [Ambiente de dev consistente](#consistent-dev-environments)
  - [dependências consistentes](#consistent-dependencies)
- [dependências](#dependencies)
- [Testes](#testing)
- [Estrutuaras e nomes](#structure-and-naming)
- [Estilo de código](#code-style)
  - [Alguns padrões de estilo de código](#code-style-check)
  - [Forçando os padrões de códigos definidos](#enforcing-code-style-standards)
- [Logging](#logging)
- [API](#api)
  - [Design da API](#api-design)
  - [Segurança da API](#api-security)
  - [Documentação da API](#api-documentation)
- [Licensas](#licensing)

<a name="git"></a>

## 1. Git

![Git](/images/branching.png)
<a name="some-git-rules"></a>

### 1.1 Algumas regras do Git

Essas são algumas regras do Git para manter em mente:

- Trabalhe em uma feature branch.

  _Por que?:_

  > Porque desse jeito todo o código é criado isolado em uma branch específica ao invés de poluir a branch principal com trabalho em progresso. Isso vai permitir você abrir vários pull requets sem confusão. Você pode continuar com uma branch em progresso sem correr o risco de quebrar a branch principal com código instável. [Leia mais sobre...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)

- Sempre comece uma nova branch a partir da `develop`

  _Por que?_

  > Desse jeito você pode garantir que o código na master vai estar sempre pronto para fazer build sem problemas e poderá ser usado a qualquer momento para fazer releases (isso pode ser exagero para alguns projetos).

- Nunca push direto na `develop` ou `master`. Sempre faça Pull Requests.

  _Por que?_

  > Isso permite outros membros do time saber que você terminou uma feature. Também possibilita code review e dicussões sobre o código que está prestes a ser introduzido no code base.

- Atualize sua `develop` local e faça rebase interativo antes de subir sua feature e abrir um Pull Request.

  _Por que?_

  > Rebase vai fazer um merge do branch destino do pull request e aplicar os commits que você tem localmente no topo da história sem criar um commit de merge (assumindo que não tem conflitos). Como resultado você tem uma história limpa no seu repositório. [Leia mais sobre ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

- Resolva os conflitos enquanto faz o rebase e antes de abrir o Pull Request.
- Delete feature branches, local e remoto, depois de realizar o merge.

  _Por que?_

  > Vai reduzir sua lista de branches removendo branches mortas. Vai granteir que você apenas faça o merge de uma branch uma única vez. Feature branches só devem existir enquanto o código ainda está em progresso.

- Antes de fazer um Pull Request, tenha certeza que sua feature branch está fazendo build corretamente e passando em todos os testes (incluindo os padrões de estilo de código).

  _Por que?_

  > Você está prestes a colocar seu código em uma branch estável. Se sua feature branch faz algum teste falahar, a chance é alta de que você vai quebrar o build na branch destino. Você também precisa conferir o code style antes de fazer um Pull Request. Isso contribui para legibilidade e reduz a chance de algum problema de formatação is para o code base com as outras alterações.

- Faça uso desse [this](./.gitignore) `.gitignore`.

  _Por que:_

  > É uma lista que já contém arquivos de sistemas que não devem ser enviados para o seu repositório remoto. E também exclui pastas de configuração e os arquivos comumente usado por editores e obviamente, também, pastas de dependência.

- Proteja (Bloqueie) a `develop` e `master`.

  _Por que?_

  > Protege suas branchs que devem, em teoria, estarem prontas para irem para produção de receberem códigos e mudanças irreversíveis. Leia mais sobre... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) e [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

<a name="git-workflow"></a>

### 1.2 Git workflow

Devido a maioria dos motivos listados acima, nos usamos [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) com [Interactive Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) e alguns pontos do [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (nomeação e ter uma develop branch). Os principais passos são:

- Em um projecto novo, inicialize o git na pasta do projeto. **Para qualquer features/changes ignore esse passo**.

  ```sh
  cd <pasta do projeto>
  git init
  ```

- Checkout para uma nova branch feature/bug-fix.
  ```sh
  git checkout -b <branchname>
  ```
- Faça as alterações.

  ```sh
  git add <arquivo1> <arquivo2> ...
  git commit
  ```

  _Por que?_

  > `git add <arquivo1> <arquivo2> ...` - Você deve add apenas arquivos com mudanças pequenas e concisas.

  > `git commit` Abrirá o editor, o que permite você separar o titulo da mensagem.

  > Leia mais sobre na _seção 1.3_.

  _Dica:_

  > Você poderia usar `git add -p`, o que te daria a chance de revisar todas as mudanças introduzidas, uma a uma, e decidir se inclui ou não naquele commit.

- Sincronize com as ultimas alterações no repositório remoto.
  ```sh
  git checkout develop
  git pull
  ```
  _Por que?_
  > Isso vai permitir que você lide com os conflitos na sua máquina local enquanto você faz o rebase (posteriormente) ao invés de criar um pull request com conflitos.

- Atualize sua feature branch com as ultimas alterações da develop usando rebase iterativo.
  ```sh
  git checkout <branchname>
  git rebase -i --autosquash develop
  ```

  _Por que?_
  > Você pode usar --autosquash para comprimir todos os seus commits em um único commit. Ninguém quer commits de desenvolvimento de uma feature na develop. [Leia mais sobre...](https://robots.thoughtbot.com/autosquashing-git-commits)

- Se você não tem conflitos, pule esse passo. Se você tem conflitos, [resolva-os](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) e continue onrebase.
  ```sh
  git add <file1> <file2> ...
  git rebase --continue
  ```

- Push sua branch. Rebase vai alterar a história, então você precisa usar `-f` para forçar a mudança no branch remoto. Se tem mais alguém trabalhando na mesma branch, use o comando `--force-with-lease`.
  ```sh
  git push -f
  ```

  _Por que?_
  > Quando você faz rebase, você está mudando a história na sua feature branch. Então o git ira rejeitar seu `git push`. Para passar por isso você precisa usar -f ou --force flag. [Leia mais sobre...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)

- Abra um Pull Request.
- Pull request deve ser aceito, mergiado e fechado por quem estiver revisando.
- Delete seu branch local se tiver terminado.

  ```sh
  git branch -d <nome do branch>
  ```

  Para remover todos os branchs que não existem no repositório remoto:

  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>

### 1.3 Escrevendo boas mensagens de commit 

Ter um bom padrão para criar commits e se atentar a ele faz com que trabalhar com Git e colaborar com outros seja muito mais fácil. Aqui estão algumas boas práticas ([fonte](https://chris.beams.io/posts/git-commit/#seven-rules)):

- Separe o assunto e a mensagem com uma nova linha entre eles.

  _Por que?_

  > Git é inteligente o suficiente para identificar a primeira linha do seu commit como um resumo. Na verdade, se você tentar shortlog, ao invés de git log, você vai ver uma longa lista de mensagens de commits, com apenas o id e o resumo do commit.

- Máximo de 50 caracteres para o assunto e 72 para a mensagem.

  _Por que?_

  > Commits devem ser objetivos e claros, não é o momento para ser verboso. [Leia mais sobre...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

- Capitalize a linha do assunto.
- Não use um ponto para finalizar a linha do assunto.
- Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) na linha do assunto.

  _Por que?_

  > É melhor que o commit diga o que vai acontecer no projeto depois daquele commit do que o que o que aconteceu dentro do commit em si. [Lei mais sobre...](https://news.ycombinator.com/item?id=2079612)

* Use a mensagem para explicar **o que** e **porque** ao invés de **como**.

<a name="documentation"></a>

## 2. Documentação

![Documentation](/images/documentation.png)

- Use esse [template](./README.sample.md) para `README.md`, sinta-se a vontade para adicionar seções que achar necessárias.
- Para projetos com mais de um repositório adicione todos os respctivos links nos `README.md` de todos os projetos.
- Mantenha o `README.md` enquanto o projeto evolui.
- Comente seu código. Tente sempre deixar claro o que uma grande parte do código tem a intenção de fazer.
- Se existe alguma referência em relação a forma como você resolveu o problema ou uma discussão em aberto, adicione os links.
- Não use comentários como desculpa para fazer um código ruim. Matenha seu código limpo.
- Não use código limpo como uma desculpa para não fazer nenhum comentário.
- Matenha apenas os comentários relevantes enquanto o código evolui.

<a name="environments"></a>

## 3. Ambientes

![Environments](/images/laptop.png)

- Defina ambientes de `desenvolvimento`, `testes` e `produção` separados.

  _Por que?_

  > Diferentes informações, dados, tokens, APIs, portas etc... podem ter que ser diferentes em cada ambiente. Você provavelmente vai querer isolar seu ambiente de `desenvolvimento` para fazer chamadas fake para a API que retornará dados previsíveis, tornando tanto os testes automatizados quanto os manuais muito mais fácil. Ou você pode querer ativar o Google Analytics apenas em `produção` e etc... [Leia mais sobre...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)

* Carregue suas configurações específicas de deploy de variáveis de ambiente e nunca as adicione no seu codebase como constantes, [veja aqui um exemplo](./config.sample.js).

  _Por que?_

  > Você terá tokens, senhas e outras informações sigilosas nessa configuração. Sua configuração deve ser corretamente separada da sua aplicação como se seu codebase pudesse se tornar público a qualquer momento.

  _Como?_

  > Arquivos `.env` para manter suas variáveis e então adicione-o ao `.gitignore` para ser excluído. Ao invés, commit um `.env.example` que servirá de modelo para outros desenvolvedores. Para produção, você deve setar suas variáveis no jeito padrão. [Leia mais sobre...](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

* É recomendável validar suas variáveis de ambiente antes de inicializar sua aplicação. [De uma olhada nesse exemplo](./configWithTest.sample.js) usando `joi` para validar os valores.

  _Por que?_

  > Pode salvar todos de horas de "dor de cabeça".

<a name="consistent-dev-environments"></a>

### 3.1 Consistente ambientes de dev:

- Defina sua versão do node em `engines` no `package.json`.

  _Por que?_

  > Permite que todos saibem em qual versão o projeto funciona. [Leia mais sobre...](https://docs.npmjs.com/files/package.json#engines)

- Adicionamente, use `nvm` e crie um arquivo `.nvmrc` na raíz do seu projeto. Não se esqueça de menciona-lo na sua documentação.

  _Por que?_

  > Qualque pessoa que usar `nvm` pode apenas rodar `nvm use` para trocar para a versão correta. [leia mais sobre...](https://github.com/creationix/nvm)

- É uma boa ideia criar um script `preinstall` para conferir as versões do node e do npm.

  _Por que?_

  > Algumas dependências podem falhar quando instaladas por versões mais recentes do NPM.

- Use Docker se puder.

  _Por que?_

  > Te dará um ambiente estável durante todo o workflow. Sem muita necessidade de lidar com dependências e configurações. [leia mais sobre...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)

- Use local modules ao invés de modules instalados globalmente.

  _Por que?_

  > Você estará compartilhando suas dependências com os outros ao invés de esperar que eles a tenham instalado globalmente.

<a name="consistent-dependencies"></a>

### 3.2 Dependências consistentes:

- Garanta que seus colegas de equipe obtenham exatamente a mesma versão de dependências que você.

  _Por que?_

  > Porque você quer que se código tenha o mesmo comportamento em qualquer máquina de desenvolvimento [leia mais sobre...](https://medium.com/@kentcdodds/why-semver-ranges-are-literally-the-worst-817cdcb09277)

  _Como?_

  > Use `package-lock.json` a partir do `npm@5`

  _E se eu não tenho npm@5?_

  > Uma alternativa pode ser o `Yarn` e não se esqueça de mencionar o seu uso no `README.md`. Seu lock file e o `package.json` devem manter as mesmas versões após cada atualização. [leia mais sobre...](https://yarnpkg.com/en/)

  _E se eu não gosto do nome `Yarn`?_

  > Que pena. Para versões antigas do `npm`, use `—save --save-exact` quando instalando novas dependências e criando um `npm-shrinkwrap.json` antes de publicar. [Leia mais sobre...](https://docs.npmjs.com/files/package-locks)

<a name="dependencies"></a>

## 4. Dependencies

![Github](/images/modules.png)

- Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
- See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)

  _Why:_

  > You may include an unused library in your code and increase the production bundle size. Find unused dependencies and get rid of them.

- Before using a dependency, check its download statistics to see if it is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)

  _Why:_

  > More usage mostly means more contributors, which usually means better maintenance, and all of these result in quickly discovered bugs and quickly developed fixes.

- Before using a dependency, check to see if it has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

  _Why:_

  > Having loads of contributors won't be as effective if maintainers don't merge fixes and patches quickly enough.

- If a less known dependency is needed, discuss it with the team before using it.
- Always make sure your app works with the latest version of its dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

  _Why:_

  > Dependency updates sometimes contain breaking changes. Always check their release notes when updates show up. Update your dependencies one by one, that makes troubleshooting easier if anything goes wrong. Use a cool tool such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

- Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).

<a name="testing"></a>

## 5. Testing

![Testing](/images/testing.png)

- Have a `test` mode environment if needed.

  _Why:_

  > While sometimes end to end testing in `production` mode might seem enough, there are some exceptions: One example is you may not want to enable analytical information on a 'production' mode and pollute someone's dashboard with test data. The other example is that your API may have rate limits in `production` and blocks your test calls after a certain amount of requests.

- Place your test files next to the tested modules using `*.test.js` or `*.spec.js` naming convention, like `moduleName.spec.js`.

  _Why:_

  > You don't want to dig through a folder structure to find a unit test. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

* Put your additional test files into a separate test folder to avoid confusion.

  _Why:_

  > Some test files don't particularly relate to any specific implementation file. You have to put it in a folder that is most likely to be found by other developers: `__test__` folder. This name: `__test__` is also standard now and gets picked up by most JavaScript testing frameworks.

* Write testable code, avoid side effects, extract side effects, write pure functions

  _Why:_

  > You want to test a business logic as separate units. You have to "minimize the impact of randomness and nondeterministic processes on the reliability of your code". [read more...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)

  > A pure function is a function that always returns the same output for the same input. Conversely, an impure function is one that may have side effects or depends on conditions from the outside to produce a value. That makes it less predictable. [read more...](https://hackernoon.com/structure-your-javascript-code-for-testability-9bc93d9c72dc)

* Use a static type checker

  _Why:_

  > Sometimes you may need a Static type checker. It brings a certain level of reliability to your code. [read more...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)

- Run tests locally before making any pull requests to `develop`.

  _Why:_

  > You don't want to be the one who caused production-ready branch build to fail. Run your tests after your `rebase` and before pushing your feature-branch to a remote repository.

- Document your tests including instructions in the relevant section of your `README.md` file.

  _Why:_

  > It's a handy note you leave behind for other developers or DevOps experts or QA or anyone who gets lucky enough to work on your code.

<a name="structure-and-naming"></a>

## 6. Structure and Naming

![Structure and Naming](/images/folder-tree.png)

- Organize your files around product features / pages / components, not roles. Also, place your test files next to their implementation.


    **Bad**

    ```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
    ```

    **Good**

    ```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
    ```

    _Why:_
    > Instead of a long list of files, you will create small modules that encapsulate one responsibility including its test and so on. It gets much easier to navigate through and things can be found at a glance.

- Put your additional test files to a separate test folder to avoid confusion.

  _Why:_

  > It is a time saver for other developers or DevOps experts in your team.

- Use a `./config` folder and don't make different config files for different environments.

  _Why:_

  > When you break down a config file for different purposes (database, API and so on); putting them in a folder with a very recognizable name such as `config` makes sense. Just remember not to make different config files for different environments. It doesn't scale cleanly, as more deploys of the app are created, new environment names are necessary.
  > Values to be used in config files should be provided by environment variables. [read more...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)

* Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts.

  _Why:_

  > It's very likely you may end up with more than one script, production build, development build, database feeders, database synchronization and so on.

- Place your build output in a `./build` folder. Add `build/` to `.gitignore`.

  _Why:_

  > Name it what you like, `dist` is also cool. But make sure that keep it consistent with your team. What gets in there is most likely generated (bundled, compiled, transpiled) or moved there. What you can generate, your teammates should be able to generate too, so there is no point committing them into your remote repository. Unless you specifically want to.

<a name="code-style"></a>

## 7. Code style

![Code style](/images/code-style.png)

<a name="code-style-check"></a>

### 7.1 Some code style guidelines

- Use stage-2 and higher JavaScript (modern) syntax for new projects. For old project stay consistent with existing syntax unless you intend to modernise the project.

  _Why:_

  > This is all up to you. We use transpilers to use advantages of new syntax. stage-2 is more likely to eventually become part of the spec with only minor revisions.

- Include code style check in your build process.

  _Why:_

  > Breaking your build is one way of enforcing code style to your code. It prevents you from taking it less seriously. Do it for both client and server-side code. [read more...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

- Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) to enforce code style.

  _Why:_

  > We simply prefer `eslint`, you don't have to. It has more rules supported, the ability to configure the rules, and ability to add custom rules.

- We use [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Use the javascript style guide required by the project or your team.

- We use [Flow type style check rules for ESLint](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).

  _Why:_

  > Flow introduces few syntaxes that also need to follow certain code style and be checked.

- Use `.eslintignore` to exclude files or folders from code style checks.

  _Why:_

  > You don't have to pollute your code with `eslint-disable` comments whenever you need to exclude a couple of files from style checking.

- Remove any of your `eslint` disable comments before making a Pull Request.

  _Why:_

  > It's normal to disable style check while working on a code block to focus more on the logic. Just remember to remove those `eslint-disable` comments and follow the rules.

- Depending on the size of the task use `//TODO:` comments or open a ticket.

  _Why:_

  > So then you can remind yourself and others about a small task (like refactoring a function or updating a comment). For larger tasks use `//TODO(#3456)` which is enforced by a lint rule and the number is an open ticket.

* Always comment and keep them relevant as code changes. Remove commented blocks of code.

  _Why:_

  > Your code should be as readable as possible, you should get rid of anything distracting. If you refactored a function, don't just comment out the old one, remove it.

* Avoid irrelevant or funny comments, logs or naming.

  _Why:_

  > While your build process may(should) get rid of them, sometimes your source code may get handed over to another company/client and they may not share the same banter.

* Make your names search-able with meaningful distinctions avoid shortened names. For functions use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.

  _Why:_

  > It makes it more natural to read the source code.

* Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below.

  _Why:_

  > It makes it more natural to read the source code.

<a name="enforcing-code-style-standards"></a>

### 7.2 Enforcing code style standards

- Use a [.editorconfig](http://editorconfig.org/) file which helps developers define and maintain consistent coding styles between different editors and IDEs on the project.

  _Why:_

  > The EditorConfig project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. EditorConfig files are easily readable and they work nicely with version control systems.

- Have your editor notify you about code style errors. Use [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) and [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) with your existing ESLint configuration. [read more...](https://github.com/prettier/eslint-config-prettier#installation)

- Consider using Git hooks.

  _Why:_

  > Git hooks greatly increase a developer's productivity. Make changes, commit and push to staging or production environments without the fear of breaking builds. [read more...](http://githooks.com/)

- Use Prettier with a precommit hook.

  _Why:_

  > While `prettier` itself can be very powerful, it's not very productive to run it simply as an npm task alone each time to format code. This is where `lint-staged` (and `husky`) come into play. Read more on configuring `lint-staged` [here](https://github.com/okonet/lint-staged#configuration) and on configuring `husky` [here](https://github.com/typicode/husky).

<a name="logging"></a>

## 8. Logging

![Logging](/images/logging.png)

- Avoid client-side console logs in production

  _Why:_

  > Even though your build process can (should) get rid of them, make sure that your code style checker warns you about leftover console logs.

- Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or
  [node-bunyan](https://github.com/trentm/node-bunyan)).

      _Why:_
      > It makes your troubleshooting less unpleasant with colorization, timestamps, log to a file in addition to the console or even logging to a file that rotates daily. [read more...](https://blog.risingstack.com/node-js-logging-tutorial/)

<a name="api"></a>

## 9. API

<a name="api-design"></a>

![API](/images/api.png)

### 9.1 API design

_Why:_

> Because we try to enforce development of sanely constructed RESTful interfaces, which team members and clients can consume simply and consistently.

_Why:_

> Lack of consistency and simplicity can massively increase integration and maintenance costs. Which is why `API design` is included in this document.

- We mostly follow resource-oriented design. It has three main factors: resources, collection, and URLs.

  - A resource has data, gets nested, and there are methods that operate against it.
  - A group of resources is called a collection.
  - URL identifies the online location of resource or collection.

  _Why:_

  > This is a very well-known design to developers (your main API consumers). Apart from readability and ease of use, it allows us to write generic libraries and connectors without even knowing what the API is about.

- use kebab-case for URLs.
- use camelCase for parameters in the query string or resource fields.
- use plural kebab-case for resource names in URLs.

- Always use a plural nouns for naming a url pointing to a collection: `/users`.

  _Why:_

  > Basically, it reads better and keeps URLs consistent. [read more...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)

- In the source code convert plurals to variables and properties with a List suffix.

  _Why_:

  > Plural is nice in the URL but in the source code, it’s just too subtle and error-prone.

- Always use a singular concept that starts with a collection and ends to an identifier:

  ```
  /students/245743
  /airports/kjfk
  ```

- Avoid URLs like this:

  ```
  GET /blogs/:blogId/posts/:postId/summary
  ```

  _Why:_

  > This is not pointing to a resource but to a property instead. You can pass the property as a parameter to trim your response.

- Keep verbs out of your resource URLs.

  _Why:_

  > Because if you use a verb for each resource operation you soon will have a huge list of URLs and no consistent pattern which makes it difficult for developers to learn. Plus we use verbs for something else.

- Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result. These **are not** CRUD (create, retrieve, update, and delete) operations:

  ```
  /translate?text=Hallo
  ```

  _Why:_

  > Because for CRUD we use HTTP methods on `resource` or `collection` URLs. The verbs we were talking about are actually `Controllers`. You usually don't develop many of these. [read more...](https://byrondover.github.io/post/restful-api-guidelines/#controller)

- The request body or response type is JSON then please follow `camelCase` for `JSON` property names to maintain the consistency.

  _Why:_

  > This is a JavaScript project guideline, where the programming language for generating and parsing JSON is assumed to be JavaScript.

- Even though a resource is a singular concept that is similar to an object instance or database record, you should not use your `table_name` for a resource name and `column_name` resource property.

  _Why:_

  > Because your intention is to expose Resources, not your database schema details.

- Again, only use nouns in your URL when naming your resources and don’t try to explain their functionality.

  _Why:_

  > Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` . Also avoid sending resource operations as a parameter.

- Explain the CRUD functionalities using HTTP methods:

  _How:_

  > `GET`: To retrieve a representation of a resource.

  > `POST`: To create new resources and sub-resources.

  > `PUT`: To update existing resources.

  > `PATCH`: To update existing resources. It only updates the fields that were supplied, leaving the others alone.

  > `DELETE`: To delete existing resources.

* For nested resources, use the relation between them in the URL. For instance, using `id` to relate an employee to a company.

  _Why:_

  > This is a natural way to make resources explorable.

  _How:_

  > `GET /schools/2/students` , should get the list of all students from school 2.

  > `GET /schools/2/students/31` , should get the details of student 31, which belongs to school 2.

  > `DELETE /schools/2/students/31` , should delete student 31, which belongs to school 2.

  > `PUT /schools/2/students/31` , should update info of student 31, Use PUT on resource-URL only, not collection.

  > `POST /schools` , should create a new school and return the details of the new school created. Use POST on collection-URLs.

* Use a simple ordinal number for a version with a `v` prefix (v1, v2). Move it all the way to the left in the URL so that it has the highest scope:

  ```
  http://api.domain.com/v1/schools/3/students
  ```

  _Why:_

  > When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening. [read more...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)

- Response messages must be self-descriptive. A good error message response might look something like this:

  ```json
  {
    "code": 1234,
    "message": "Something bad happened",
    "description": "More details"
  }
  ```

  or for validation errors:

  ```json
  {
    "code": 2314,
    "message": "Validation Failed",
    "errors": [
      {
        "code": 1233,
        "field": "email",
        "message": "Invalid email"
      },
      {
        "code": 1234,
        "field": "password",
        "message": "No password provided"
      }
    ]
  }
  ```

  _Why:_

  > developers depend on well-designed errors at the critical times when they are troubleshooting and resolving issues after the applications they've built using your APIs are in the hands of their users.


    _Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only the password was incorrect._

- Use these status codes to send with your response to describe whether **everything worked**,
  The **client app did something wrong** or The **API did something wrong**.

  _Which ones:_ > `200 OK` response represents success for `GET`, `PUT` or `POST` requests.

      > `201 Created` for when a new instance is created. Creating a new instance, using `POST` method returns `201` status code.

      > `204 No Content` response represents success but there is no content to be sent in the response. Use it when `DELETE` operation succeeds.

      > `304 Not Modified` response is to minimize information transfer when the recipient already has cached representations.

      > `400 Bad Request` for when the request was not processed, as the server could not understand what the client is asking for.

      > `401 Unauthorized` for when the request lacks valid credentials and it should re-request with the required credentials.

      > `403 Forbidden` means the server understood the request but refuses to authorize it.

      > `404 Not Found` indicates that the requested resource was not found.

      > `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.

      _Why:_
      > Most API providers use a small subset HTTP status codes. For example, the Google GData API uses only 10 status codes, Netflix uses 9, and Digg, only 8. Of course, these responses contain a body with additional information.There are over 70 HTTP status codes. However, most developers don't have all 70 memorized. So if you choose status codes that are not very common you will force application developers away from building their apps and over to wikipedia to figure out what you're trying to tell them. [read more...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)

* Provide total numbers of resources in your response.
* Accept `limit` and `offset` parameters.

* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource. Use a fields query parameter that takes a comma separated list of fields to include:
  ```
  GET /student?fields=id,name,age,class
  ```
* Pagination, filtering, and sorting don’t need to be supported from start for all resources. Document those resources that offer filtering and sorting.

<a name="api-security"></a>

### 9.2 API security

These are some basic security best practices:

- Don't use basic authentication unless over a secure connection (HTTPS). Authentication tokens must not be transmitted in the URL: `GET /users/123?token=asdf....`

  _Why:_

  > Because Token, or user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. [read more...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

- Tokens must be transmitted using the Authorization header on every request: `Authorization: Bearer xxxxxx, Extra yyyyy`.

- Authorization Code should be short-lived.

- Reject any non-TLS requests by not responding to any HTTP request to avoid any insecure data exchange. Respond to HTTP requests by `403 Forbidden`.

- Consider using Rate Limiting.

  _Why:_

  > To protect your APIs from bot threats that call your API thousands of times per hour. You should consider implementing rate limit early on.

- Setting HTTP headers appropriately can help to lock down and secure your web application. [read more...](https://github.com/helmetjs/helmet)

- Your API should convert the received data to their canonical form or reject them. Return 400 Bad Request with details about any errors from bad or missing data.

- All the data exchanged with the REST API must be validated by the API.

- Serialize your JSON.

  _Why:_

  > A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser... or, if you're using node.js, on the server. It's vital that you use a proper JSON serializer to encode user-supplied data properly to prevent the execution of user-supplied input on the browser.

- Validate the content-type and mostly use `application/*json` (Content-Type header).

  _Why:_

  > For instance, accepting the `application/x-www-form-urlencoded` mime type allows the attacker to create a form and trigger a simple POST request. The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `4XX` response.

- Check the API Security Checklist Project. [read more...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>

### 9.3 API documentation

- Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
- Describe API authentication methods with a code sample.
- Explaining The URL Structure (path only, no root URL) including The request type (Method).

For each endpoint explain:

- URL Params If URL Params exist, specify them in accordance with name mentioned in URL section:

  ```
  Required: id=[integer]
  Optional: photo_id=[alphanumeric]
  ```

- If the request type is POST, provide working examples. URL Params rules apply here too. Separate the section into Optional and Required.

- Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect:

  ```
  Code: 200
  Content: { id : 12 }
  ```

- Error Response, Most endpoints have many ways to fail. From unauthorized access to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
  ```json
  {
    "code": 403,
    "message": "Authentication failed",
    "description": "Invalid username or password"
  }
  ```

* Use API design tools, There are lots of open source tools for good documentation such as [API Blueprint](https://apiblueprint.org/) and [Swagger](https://swagger.io/).

<a name="licensing"></a>

## 10. Licensing

![Licensing](/images/licensing.png)

Make sure you use resources that you have the rights to use. If you use libraries, remember to look for MIT, Apache or BSD but if you modify them, then take a look at the license details. Copyrighted images and videos may cause legal problems.

---

Sources:
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials),
[Apigee](https://apigee.com/about/blog),
[Wishtack](https://blog.wishtack.com)

Icons by [icons8](https://icons8.com/)
