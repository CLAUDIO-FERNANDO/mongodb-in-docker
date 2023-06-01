# mongodb-in-docker
MongoDB - docker - docker-compose


 Vou explicar o código em detalhes para que você possa entender melhor cada parte:


version: '3.8'
```
A versão do formato do arquivo do Docker Compose que estamos utilizando.

---------------------------------------------------------------------------------------------------------------------
networks:
  mongo-network:
    driver: bridge
```
Definição de uma rede chamada `mongo-network` com o driver de rede `bridge`. Essa rede será usada para conectar os serviços `mongo-express` e `mongodb`.
---------------------------------------------------------------------------------------------------------------------

services:
  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    networks:
      - mongo-network
    environment:
      - ME_CONFIG_MONGODB_SERVER=mongodb
      - ME_CONFIG_MONGODB_ADMINUSERNAME=mongouser
      - ME_CONFIG_MONGODB_ADMINPASSWORD=mongopwd
    depends_on:
      - mongodb
```
Definição do serviço `mongo-express`:

- Utilizamos a imagem `mongo-express`, que é a imagem oficial do Mongo Express.
- Mapeamos a porta `8081` do host para a porta `8081` do contêiner, permitindo acessar o Mongo Express no host através da porta `8081`.
- Conectamos o serviço à rede `mongo-network`.
- Definimos as variáveis de ambiente necessárias para configurar a conexão com o MongoDB, incluindo o servidor (`ME_CONFIG_MONGODB_SERVER`), nome de usuário (`ME_CONFIG_MONGODB_ADMINUSERNAME`) e senha (`ME_CONFIG_MONGODB_ADMINPASSWORD`).
- Especificamos que o serviço depende do serviço `mongodb`, garantindo que o MongoDB esteja rodando antes de iniciar o Mongo Express.

---------------------------------------------------------------------------------------------------------------------

  mongodb:
    image: mongo:4.4.3
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    networks:
      - mongo-network
    environment:
      - MONGO_INITDB_ROOT_USERNAME=mongouser
      - MONGO_INITDB_ROOT_PASSWORD=mongopwd
```
Definição do serviço `mongodb`:

- Utilizamos a imagem `mongo:4.4.3`, que é a imagem oficial do MongoDB na versão 4.4.3.
- Mapeamos a porta `27017` do host para a porta `27017` do contêiner, permitindo acessar o MongoDB no host através da porta `27017`.
- Criamos um volume nomeado `mongo_data` e o mapeamos para o diretório `/data/db` dentro do contêiner, garantindo que os dados do MongoDB sejam persistentes mesmo após reinicializações.
- Conectamos o serviço à rede `mongo-network`.
- Definimos as variáveis de ambiente necessárias para configurar a inicialização do MongoDB, incluindo o nome de usuário do superusuário (`MONGO_INITDB_ROOT_USERNAME`) e a senha do superusuário (`MONGO_INITDB_ROOT_PASSWORD`).

---------------------------------------------------------------------------------------------------------------------
volumes:
  mongo_data:
```
Declaração do volume nomeado `mongo_data` que será usado para persistir os dados do MongoDB.

Espero que essa explicação detalhada ajude você a entender melhor o código do Docker Compose. 