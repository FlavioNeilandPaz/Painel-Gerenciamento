# Painel Admin CRUD

 - https://adminbro.com/tutorial-installation-instructions.html

 ## AMBIENTE 

 Você vai precisar de:

 1. Editor de Código (Eu uso o VScode)
 2. Node.js
 3. Mongo DB
 4. Terminal

 ## Iniciando com o AdminBro

 1. Crie a Estrutura
    
    ```sh
    mkdir project
    cd project
    touch admin.js
    ```

2. Instale as dependências

    ```sh
    npm install admin-bro @admin-bro/express express-formidable
    ```

3. admin.js
    ```js    
    //==============================================
    // Admin Bro
    const AdminBro = require('admin-bro') //Constante para receber a dependência AdminBro
    const AdminBroExpress = require('@admin-bro/express')

    const adminBro = new AdminBro({ //Objeto AdminBro
        databases: [], //Quais as bases de dados utilizadas
        rootPath: '/admin', //Qual o caminho a ser acessado
    })

    const router = AdminBroExpress.buildRouter(adminBro) //vamos constuir uma rota estilo express, passando o nosso objeto adminbro

    // ==============================================
    // SERVER
    const express = require("express"); //constante express
    const server = express(); //constante server, objeto de servidor

    server
        .use(adminBro.options.rootPath, router) //a configuração esta criando uma rota
        .listen(5500, () => console.log("Server started"));
    ```

4. Start Server

    ```sh
    node admin.js
    ```

## Adcionando Database

1. Instale as dependências

    ```sh
    npm i @admin-bro/mongoose mongoose
    ```
2. Update admin.js

    ```js

    const mongoose = require ("mongoose");

    const ProjectSchema = mongoose.Schema({
        title: {
            type:String
            require: true,            
        },
        description: String,
        completed: Boolean,
        created_at: { type: Date, default: Date.now},
    });

    const Project = mongoose.model("Project", ProjectSchema);


    const AdminBro = require('admin-bro')
    const AdminBroExpress = require('@admin-bro/express')
    const AdminBroMongoose = require('@admin-bro/mongoose')


    AdminBro.registerAdapter(AdminBroMongoose)

    const adminBroOptions = new AdminBro({
        resources: [Project],
            rootPath: '/admin'
    })

    const router = AdminBroExpress.buildRouter(adminBroOptions)

    const express =  require("express");
    const server = express();

    server
        .use(adminBroOptions.options.rootPath, router)


    const run = async () => {
        await  mongoose.connect("mongodb://localhost/adminbroapp", {
            useNewUrlParser: true,
            useUnifiedTopology: true
        });

        await.server.listen(5500, () => console.log("Server Started"));
    }

    run()

