Automatização de testes mobile: configurando ambiente Robot + Appium Library

Esse post é para explicar como configurar o ambiente para automatizarmos testes de aplicativos mobile (linguagem nativa).
Pressupostos: que você tenha NPM e NODE instalados e configurados na sua máquina!
Faremos aqui utilizando Windows (para falar a verdade, tive MUITOS problemas com a instalação do Java no Linux — e como o Android Studio necessita do pacote JDK…).
Image for post
Exemplo de um teste .robot
Para configurar o Robot Framework…
(A documentação do Robot está neste link)
O Robot é um framework escrito em Python e é utilizado para automação de testes em geral (APIs, web, mobile). Mas fiquem calmos, ele traduz linguagem natural para Python, então não é preciso muito conhecimento técnico — apenas algumas regras que colocarei por aqui.
O Robot precisa de bibliotecas e ferramentas adicionais que ajudam a fazer a tradução através de palavras-chave. Para fazer automação de testes mobile utilizaremos a Appium Library (clique aqui para ver a documentação). A documentação dessa biblioteca diz que ainda não funciona bem com o Python3, mas não tive sucesso com o Python 2.7, então instalei o 3 e ainda não tive problemas.
A Appium Library permitirá que utilizemos palavras-chave como ‘Click Element’, ‘Close Application’, ‘Element Text Should Be’, etc..
O único pré-requisito para conseguirmos utilizar essa biblioteca é: os elementos precisam ter sido construídos (no código, mesmo, do aplicativo) com algumas propriedades específicas… Os únicos localizadores válidos para Appium + Android UIAutomator2 são: accessibility id, id, class name ou xpath — altamente desencorajado por causa da instabilidade da ‘DOM’.
Você também precisará instalar a versão desktop do Appium (clique aqui para documentação) para conseguir localizar os seletores do aplicativo mobile. E também precisará instalar via linha de comando de modo global na máquina (documentação aqui).
Para utilizar o Appium, é necessário ter uma automação de acordo com o sistema operacional que você pretende usar. Neste caso, utilizaremos o Android e o driver UIAutomator2. E para emular dos celulares Android utilizaremos o Android Studio, que sugere que você tenha instalado o pacote Java SDK.
Vamos lá…
Passo a passo:
Instalar o Python (global)
Verificar se já está instalado na máquina;
Abra o prompt de comando do Windows e digite o comando:
Image for post
Se tiver instalado no seu computador, aparecerá o número da versão.
Entrar no site oficial para o passo a passo da instalação em Windows https://python.org.br/instalacao-windows/;
Fazer o download da versão mais atual estável (recomendada) https://www.python.org/downloads/windows/;
Fazer a instalação do Python através do arquivo .exe que foi baixado;
Conferir se o Python e o pip (gerenciador de pacotes do Python) foram instalados corretamente;
Abra o prompt de comando do Windows e digite o comando:
Image for post
Image for post
Se ambos tiverem sido instalados corretamente, aparecerá o número da versão de cada um.
2. Instalar o Robot Framework
Abra o prompt de comando do Windows e digite o comando:
pip install robotframework
3. Instalar extensões para Visual Studio Code
Supondo que você já tenha o VSCode instalado na máquina, é só clicar na parte de extensões (barra lateral esquerda);
Instale a extensão oficial para rodar o Python (não precisar instalar outra IDE!!);
Image for post
E pra te auxiliar a escrever os testes, a extensão do Robot Framework Intellisense, pois ele identificará as sintaxes próprias dos arquivos ‘.resource’ e ‘.robot’;
Image for post
4. Instalar a biblioteca AppiumLibrary para ter as keywords específicas para testes mobile:
Entre na página da biblioteca no GitHub para obter o passo a passo, dúvidas, código e ver as issues abertas;
Abra o prompt de comando do Windows e digite o comando abaixo para instalar a biblioteca no seu computador:
pip install robotframework-appiumlibrary
5. Instalar o aplicativo Appium para desktop Windows:
Clique aqui para baixar o arquivo .exe do Appium (escolha a última versão disponível para Windows) e instale no seu computador.
6. Instalar o Appium globalmente:
Abra o prompt de comando do Windows e digite o comando:
npm install -g appium
7. Instalar o driver UIAutomator2 com NPM:
Abra o prompt de comando do Windows e digite o comando:
npm install appium-uiautomator2-driver
8. Instalar Android Studio / Java SDK
Primeiramente siga os passos da parte 1 e 2 deste blog:
Parte 1: https://medium.com/@lcmuniz/parte-1-instalando-o-java-no-windows-b56e5c758cdb
Parte 2: https://medium.com/@lcmuniz/parte-2-instalando-o-android-studio-no-windows-4e9f28c9a84
Após a instalação precisamos configurar as variáveis de ambiente para o ‘ANDROID_HOME’. No menu iniciar do Windows, digite ‘variáveis de ambiente’ e abrirá uma tela parecida com a imagem abaixo. Clique na sugestão do Windows;
Image for post
Na aba ‘Avançado’, clique em ‘Variáveis de ambiente’.
Image for post
Verifique se já não tem configurada as variáveis ‘ANDROID_HOME’ e ‘JAVA_HOME’. Se não tiver, clique em novo para adicionar as duas variáveis (uma por vez) com as informações dos paths desses arquivos que estão instalados no seu computador. Não serão os mesmos paths que os da imagem a seguir, mas serão parecidos…
Image for post
Ainda na mesma tela, ache a variável de sistema ‘Path’ e clique em ‘Editar’;
Image for post
Confira se alguma dessas configurações já existem. Se não existirem, é só clicar em ‘Novo’ e adicionar;
Obs: algumas pastas podem estar ocultas e você precisará configurar para conseguirem ser identificadas!
Image for post
Quando finalizar, é só clicar em ‘OK’ nesta tela e ‘OK’ na próxima;
Quando voltar pra janela ‘Propriedades do Sistema’, clique em ‘OK’;
Cada versão do Android tem seu próprio Software Development Kit e você precisa atualizar de tempos em tempos. Abra o aplicativo do Android Studio no computador e veja seus SDKs em ‘Configure’ e depois ‘SDK Manager’. Mantenho sempre a última versão da plataforma e uma mais antiga.
Image for post
Image for post
Na mesma tela, vá em ‘SDK Tools’ e confira se tem todos os pacotes instalados conforme a imagem abaixo. Após conferir, clique em ‘Apply’ e depois ‘OK’;
Image for post
Depois de seguir esses passos, você voltará para a parte inicial do Android Studio. Clique em ‘Configure’ (lateral direita na parte de baixo) e depois ‘AVD Manager’. É ele que vai conseguir abrir o emulador quando você solicitar;
Image for post
Aguarde o ‘Android Virtual Device’ abrir e clique em ‘Create Virtual Device’;
Image for post
Escolha dispositivo ‘Phone’, gosto de escolher o ‘Pixel’ pelo tamanho mediano da tela (responsividade) e depois clicar em ‘Next’;
Image for post
Escolha a versão do Android para instalar e rodar no emulador (dica: escolha sempre uma das mais recentes), clique em ‘Download’ e aceite a instalação. Quando estiver instalado, clique em ‘Next’;
Image for post
Escolha o nome do emulador e clique em ‘Finish’;
Image for post
Para rodar o emulador é só clicar no ‘Play’;
Image for post
