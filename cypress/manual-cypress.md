<h2 align="center">
  Automatização de testes web: configurando ambiente Cypress + Mochawesome
</h2>

<p>
  O Cypress utiliza linguagem Javascript com algumas bibliotecas para sintaxe de comportamento
  (Mocha, Chai, Sinon).
  Você precisará ter o Node instalado na máquina para conseguir rodar as automatizações.
  Podemos rodar o Cypress em modo ‘headless’ (sem abrir o navegador visualmente) ou abrindo com o módulo para desktop.
  Também é possível gerar relatórios com a instalação de bibliotecas adicionais.
</p>

<img src="./images/cypress1.png">
Fonte: cypress.io

Passo a passo inicial:
<ol>
  <li>
    Caso você esteja começando o projeto do zero precisará criar o arquivo ‘package.json’.
    Utilizando npm, abra o terminal na raiz do projeto e digite:
  </li>
  ```bash
  npm init
  ```
  <li>
    Instale o Cypress como dependência de desenvolvimento via terminal:
  </li>
  ```bash
  npm --save-dev cypress
  ```
  <li>
    Você também precisará instalar o Cypress para desktop para usufruir do <i>selector playground</i>
    (vai te ajudar a muito a localizar os elementos!):
  </li>
  Link para download: <a href="https://download.cypress.io/desktop" target="_blank" rel="noopener noreferrer"></a>

  <li>
    Para rodar o Cypress pela primeira vez e instalar a estrutura de pastas e algumas specs exemplos de teste, você pode
    digitar comando no terminal:
  </li>
  ```bash
  npx cypress open
  ```

  <li>
    Para gerar relatórios, utilizo o mochawesome, um dos reporters recomendados na documentação oficial.
    Ao rodar o comando do mochawesome, além de podermos gravar a execução em mp4, serão criados vários JSON (um para
    cada spec de teste).
    Para uni-los, utilizo o mochawesome-merge, que criará um arquivo 'output.json'.
    Para facilitar a visualização, utilizo o mochawesome-report-generator para transformar o 'output.json' em um arquivo
    .html.
  </li>
  ```bash
  npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator
  ```
</ol>

E aí você me pergunta: Ah, mas é só isso??
E eu te respondo: não!
Para facilitar a sua vida, vou passar as configurações que utilizo para TODOS os projetos…
(Atenção! Esses não são os arquivos completos, é apenas a parte que referência os comandos e instalações feitas acima!)

<img src="./images/cypress2.png">
Configuração final de uma pasta contendo Cypress

<img src="./images/cypress3.png">
cypress.json

<img src="./images/cypress4.png">
package.json

Explicando linha a linha do ‘package.json’:
Obs: o nome do comando (a parte antes dos dois pontos) pode ser personalizada por você.
>> "cy:open": "cypress open"
Ao comandar ‘npm run cy:open’ no terminal você abre o módulo desktop do Cypress para rodar suas specs de teste.

>> "cy:run": "cypress run"
Ao comandar ‘npm run cy:run’ no terminal você roda as specs de teste no formato ‘headless’, sem abrir o módulo desktop —
geralmente utilizando o navegador Electron, gravando as execuções em ‘cypress/videos/…’ .

>> "report:cleanup": "rm -fr cypress/report/"
e
>> "video:cleanup": "rm -fr cypress/videos/"
Ao comandar ‘npm run report:cleanup’ e ‘npm run video:cleanup’ no terminal você limpa o conteúdo da pasta
‘cypress/report/…’ e da ‘cypress/videos/…’ . Normalmente rodo esse comando quando preciso rodar os testes novamente e
não quero me confundir com os relatórios/vídeos gerados anteriormente.

>> "test:e2e": "cypress run --headless --browser chrome --no-exit"
Ao comandar ‘npm run test:e2e’ no terminal você roda todos os testes no Chrome, sem abrir visualmente o navegador e sem
fechá-lo a cada mudança de arquivo de teste. Gerará os relatórios em .json separado por arquivos de teste, na pasta
‘cypress/report/mochawesome-report’

>> "report:merge": "mochawesome-merge cypress/report/mochawesome-report/*.json > cypress/report/output.json"
Ao comandar ‘npm run report:merge’ no terminal você unifica todos os testes .json em ‘cypress/report/output.json’.

>> "report:generate": "marge cypress/report/output.json --reportDir ./ --inline"
Ao comandar ‘npm run report:generate’ no terminal você transforma o output.json em output.html
(‘cypress/report/output.html’)