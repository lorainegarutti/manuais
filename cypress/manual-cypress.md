<h2>
  Automatização de testes web: configurando ambiente Cypress + Mochawesome
</h2>

<p>
  O Cypress utiliza linguagem Javascript com algumas bibliotecas para sintaxe de comportamento
  (Mocha, Chai, Sinon).
  <br />
  Você precisará ter o Node instalado na máquina para conseguir rodar as automatizações.
  <br />
  Podemos rodar o Cypress em modo ‘headless’ (sem abrir o navegador visualmente) ou abrindo com o módulo para desktop.
  <br />
  Também é possível gerar relatórios com a instalação de bibliotecas adicionais.
</p>

<h3>Passo a passo inicial:</h3>

<p>
  1. Caso você esteja começando o projeto do zero precisará criar o arquivo ‘package.json’.
  Utilizando npm, abra o terminal na raiz do projeto e digite:

  ```bash
  npm init
  ```
</p>

<p>
  2. Instale o Cypress como dependência de desenvolvimento via terminal:

  ```bash
  npm --save-dev cypress
  ```
</p>

<p>
  3. Você também precisará instalar o Cypress para desktop para usufruir do <i>selector playground</i>
  (vai te ajudar a muito a localizar os elementos!):
  <br />
<h5>Link para download: <a href="https://download.cypress.io/desktop" target="_blank" rel="noopener noreferrer">Cypress
    versão para desktop</a>
</h5>
</p>

<p>
  4. Para rodar o Cypress pela primeira vez e instalar a estrutura de pastas e algumas specs exemplos de teste, você
  pode
  digitar comando no terminal:

  ```bash
  npx cypress open
  ```
</p>

<p>
  5. Para gerar relatórios, utilizo o mochawesome, um dos reporters recomendados pela documentação oficial.
  <br />
  Ao rodar o comando do mochawesome, além de podermos gravar a execução em mp4, serão criados vários JSON (um para cada
  spec de teste). Para uni-los, utilizo o mochawesome-merge, que criará um arquivo 'output.json'.
  <br />
  Para facilitar a visualização, utilizo o mochawesome-report-generator para transformar o 'output.json' em um arquivo
  .html.

  ```bash
  npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator
  ```
</p>

<h3>
  E depois de rodar esses comandos, a estrutura de pastas será assim:
</h3>

<p>
  <img src="./images/cypress2.png">
</p>

<h3>
  Ajuste as configurações do Reporter no arquivo 'cypress.json' (na raiz do projeto)
</h3>

<p>
  <img src="./images/cypress3.png">
</p>

<h3>
  Configure scripts para facilitar os comandos do Reporter no arquivo 'package.json' (na raiz do projeto)
</h3>

<p>
  <img src="./images/cypress4.png">
</p>

<h3>
  Explicando cada linha do ‘package.json’
</h3>

<li>
  Para abrir o módulo desktop do Cypress:

  ```bash
  "cy:open": "cypress open"
  ```
</li>

<li>
  Para rodar as specs de teste no formato 'headless' (usa Electron por default):

  ```bash
  "cy:run": "cypress run"
  ```
</li>

<li>
  Para apagar as pastas de relatórios e vídeos antigos:

  ```bash
  "report:cleanup": "rm -fr cypress/report/"
  ```

  ```bash
  "video:cleanup": "rm -fr cypress/videos/"
  ```
</li>

<li>
  Para rodar os testes no Chrome, em modo 'headless', e gerar os relatórios na extensão '.json' na pasta ‘cypress/report/mochawesome-report’:

  ```bash
  "test:e2e": "cypress run --headless --browser chrome"
  ```
</li>

<li>
  Para unificar todos os relatórios de teste no 'cypress/report/output.json’:

  ```bash
  "report:merge": "mochawesome-merge cypress/report/mochawesome-report/*.json > cypress/report/output.json"
  ```
</li>

<li>
  Para transformar o relatório em extensão html (‘cypress/report/output.html’):

  ```bash
  "report:generate": "marge cypress/report/output.json --reportDir ./ --inline"
  ```
</li>