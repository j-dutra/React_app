# Criando um pequeno projeto com 'react', Webpack, e babel

## O que é React?

React é um framework/biblioteca mantido pelo Facebook. Sua principal finalidade está na criação de interfaces gráficas fáceis de serem mantidas e reutilizadas. 

## O que é JSX?

JSX (Javascript Syntax Extension): uma sintaxe semelhante ao XML e que permite escrever código parecido com HTML ( Não é HTML! ). O JSX não é reconhecido pelo browser e, por isso, é necessário “traduzí-lo” para que o mesmo o reconheça. Dessa forma, entra em ação o “Babel”.

## O que é o Babel?

Babel é um compilador JavaScript, uma ferramenta que “transpila” ( mistura de traduzir e compilar) o código para versões anteriores do Javascript compatíveis com os browsers, ou seja converte código escrito em sintaxes mais recentes do JavaScript (ECMAScript 2015+), que podem ser incompatíveis com diferentes browsers, para uma sintaxe compatível.

## O que é o webpack?

O webpack é um empacotador de módulos ( module bundler ) para aplicações modernas JavaScript. Esta ferramenta permite agrupar, em um único arquivo, módulos presentes em um projeto. Este arquivo único pode ser minificado para redução de seu tamanho ou não. Em relação a sua funcionalidade, o webpack recebe um arquivo ‘.js’ de entrada ( o entry point ). A partir deste arquivo, é criado um “grafo de dependências” ( dependency graph ), isto é, todos os módulos que tiverem sido importados pelo arquivo de entrada, direta ou indiretamente, são identifacados e, por fim, agrupados. Dessa forma, para configurar o webpack você precisará mencionar o local do arquivo de entrada ( entry point ), e o local do arquivo de saída ( output ). 


## Iniciando um pequeno projeto

### Requisitos para iniciar um projeto React.

    $ node --version && npm –-version

### Crie e acesse o diretório do projeto:

    $ mkdir <react-app> && cd <react-app>

### Instalando as dependências

    $ npm init -y

cria o arquivo “package.json” para automação de seus scripts e organização de seu projeto

### instalar as dependências, pacotes e bibliotecas sobre os quais o projeto será construído.

    $ npm i -D webpack webpack-cli webpack-dev-server html-loader html-webpack-plugin

    $ npm i -D react react-dom

    $ npm i -D @babel/core @babel/preset-env @babel/preset-react babel-loader


### Resumidamente, o que estes pacotes significam para o projeto:

    “webpack” = Pacote genérico do webpack.

    “webpack-cli” = Linha de comando do webpack (será utilizado para iniciar o projeto).

    “webpack-dev-server” = Ele atualiza automaticamente a aplicação após mudanças nos arquivos. Em outras palavras, ele é um facilitador de vidas :)

    “react” = Pacote genérico do React.

    “react-dom” = Pacote que tem a função de intermediar o que será renderizado (componentes) e o DOM

    “@babel/core” = Núcleo do compilador Babel

    “@babel/preset-env” =Torna o código mais recente (ECMAScript 2015+) compatível.

    “@babel/preset-react” = Converte JSX em JavaScript compatível.

    “babel-loader” = Plugin do webpack que “transpila” o código.

###  Configurando o arquivo “.babelrc”

Após a instalação das dependências, crie na raiz de seu projeto o arquivo ‘.babelrc’. Este é o arquivo de configuração do babel. Configure-o da seguinte forma:

    {
        "presets": ["@babel/preset-env", "@babel/preset-react"]
    }

### Configurando o arquivo “webpack.config.js”

Você deve configurar o webpack também! Para isso, crie na raiz de seu projeto o arquivo de configuração “webpack.config.js”. Lembra que, lá em cima, eu disse que o webpack precisa de um arquivo de entrada (entry point) e um arquivo de saída (output)? Esses detalhes são configurados no “webpack.config.js”, assim como os loaders (babel-loader e html-loader) e os plugins necessários (‘html-webpack-plugin’).

Apenas um pequeno detalhe. A partir da versão 4 do webpack, você não precisa configurar o entry point e nem o local de saída dos seus arquivos. Nesta versão, por default, o webpack busca uma pasta nomeda “src” na raiz do seu projeto. Em seguida, dentro da pasta “src”, ele busca o arquivo de entrada nomeado “index.js”. Após encontrar o “index.js”, o webpack cria uma pasta na raiz chamada “dist”, onde estarão seus arquivos de saída. É tudo automático, mas você pode mudar essas configurações padrões, se você desejar.

Antes de começar a configurar o “webpack.config.js”, crie a pasta “src” na raiz do projeto e, dentro dele, crie o arquivo “index.js”.

    $ mkdir src && touch src/index.js

Agora, crie o arquivo “webpack.config.js” também na raiz de seu projeto.

    $ touch webpack.config.js

E o configure da seguinte forma:

    const HtmlWebpackPlugin = require("html-webpack-plugin");

    module.exports = {
    module: {
        rules: [
        {
            test: /\.js$/,
            exclude: /node_modules/,
            use: { loader: "babel-loader" }
        },
        {
            test: /\.html$/,
            use: [{ loader: "html-loader" }]
        }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
        template: "./src/index.html",
        filename: "./index.html"
        })
    ]
    };

###  Criando um “Hello, World” com React

Antes de criar o primeiro componente, devemos criar um template no diretório “src” para que o ReactDOM possa renderizar o código JavaScript em conteúdo HTML. Dessa forma crie, no diretório “src”, o arquivo “index.html” da seguinte forma:

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset='utf-8'>
            <meta http-equiv='X-UA-Compatible' content='IE=edge'>
            <meta name='viewport' content='width=device-width, initial-scale=1.0'>
            <title> react-app </title>
        </head>

        <body>
            
            <div id='app'>
                <!-- Conteúdo será renderizado aqui -->
            </div>
            
        </body>
    </html>

Agora que o template foi criado, vamos criar o componente React. Para isso, crie o diretório “components” dentro do diretório “src”. Em seguida, crie o arquvio “App.js” dentro de “components”.

    $ mkdir src/components && touch src/components/App.js

Abra o arquivo “App.js” e escreva o código que se segue:

    import React, { Component } from "react";
    import ReactDOM from "react-dom";

    export default class App extends React.Component {
    render() {
        return (
        <h1> Hello, World! </h1>
        )
    }
    }

    const container = document.getElementById('app');
    ReactDOM.render(<App />, container)

Este componente criado nada mais é do que um módulo Javascript que pode ser importado por outros módulos.

O webpack é uma ferramenta que junta todos os módulos de um projeto em um único arquivo. Então, na próxima etapa, será fazer com o que o arquivo de entrada do webpack, o “index.js”, importe o componente que você acabou de criar. Para isso, abra o arquivo “index.js” e escreva o seguinte código:

    import App from "./components/App";

Agora falta só mais um detalhe: configurar o “package.json” para automatizar o início de sua aplicação. 

### Iniciando a sua aplicação

Para finalizar, abra o arquivo “package.json” que está presente na raíz de seu projeto. Nele você verá várias informações. O nosso interesse está no campo “scripts”. Neste campo, você irá adicionar comandos do terminal que irão automatizar sua aplicação. Configure este campo para que fique da seguinte forma:

    {
        "name": "ReactApp",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
            "start": "webpack-dev-server --open --mode development",
            "build": "webpack --mode production"
        },
        "keywords": [],
        "author": "",
        "license": "ISC",
        "devDependencies": {
            "@babel/core": "^7.1.0",
            "@babel/preset-env": "^7.1.0",
            "@babel/preset-react": "^7.0.0",
            "babel-loader": "^8.0.2",
            "html-loader": "^0.5.5",
            "html-webpack-plugin": "^3.2.0",
            "react": "^16.5.2",
            "react-dom": "^16.5.2",
            "webpack": "^4.19.1",
            "webpack-cli": "^3.1.0",
            "webpack-dev-server": "^3.1.8"
        }
    } 

Por fim, use o seguinte comando:

    $ npm run start

