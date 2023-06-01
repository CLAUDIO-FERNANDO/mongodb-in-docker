Aqui está a documentação explicando cada parte do código:


----------------------------------------------/VERSÃO & NETWORK/-------------------------------------------------------------------

version: '3.8'

networks:
  mongo-network:
    driver: bridge

Essa seção define a versão do Docker Compose utilizada e, em seguida, define uma rede chamada "mongo-network" com o driver "bridge". Essa rede será usada para permitir a comunicação entre os serviços.

----------------------------------------------/VOLUME/--------------------------------------------------------------------------

volumes:
  mongo_data:

Aqui, é definido um volume chamado "mongo_data". Volumes são usados para persistir os dados de um container Docker em uma localização específica do sistema de arquivos do host.

----------------------------------------------/MONGO-EXPRESS/----------------------------------------------------------------------

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



services: Indica o início da seção de definição de serviços do Docker Compose.

mongo-express: É o nome dado ao serviço do Mongo Express definido neste trecho do código.

image: mongo-express: Especifica a imagem Docker a ser utilizada para o serviço do Mongo Express. Nesse caso, a imagem oficial do Mongo Express será baixada e utilizada.

ports: Define a configuração de mapeamento de porta entre o host e o container. Neste caso, a porta 8081 do host é mapeada para a porta 8081 do container. Isso permite que você acesse o Mongo Express no host através da porta 8081.

networks: Define a rede à qual o serviço do Mongo Express será conectado. Neste caso, o serviço está conectado à rede mongo-network, que foi definida anteriormente no arquivo de configuração.

environment: Permite configurar variáveis de ambiente dentro do container do Mongo Express. Aqui, são definidas três variáveis de ambiente:

ME_CONFIG_MONGODB_SERVER: Define o nome do servidor MongoDB ao qual o Mongo Express se conectará. Nesse caso, é definido como "mongodb", que é o nome do serviço do MongoDB que foi definido anteriormente.
ME_CONFIG_MONGODB_ADMINUSERNAME: Define o nome de usuário do administrador do MongoDB que o Mongo Express utilizará para autenticação. Neste caso, é definido como "mongouser".
ME_CONFIG_MONGODB_ADMINPASSWORD: Define a senha do administrador do MongoDB que o Mongo Express utilizará para autenticação. Neste caso, é definido como "mongopwd".
depends_on: Define a dependência do serviço do Mongo Express em relação ao serviço do MongoDB. Isso significa que o serviço do MongoDB será iniciado antes do serviço do Mongo Express.

Essa parte do código configura o serviço do Mongo Express no ambiente Docker. Ele define a imagem, as portas, a rede e as variáveis de ambiente necessárias para o Mongo Express se conectar ao servidor MongoDB e autenticar usando um nome de usuário e senha específicos. Isso permite que você acesse o Mongo Express no host através da porta 8081 e gerencie o banco de dados MongoDB por meio da interface web fornecida pelo Mongo Express.



----------------------------------------------/MONGODB/--------------------------------------------------------------------------


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


mongodb: É o nome dado ao serviço do MongoDB definido neste trecho do código.

image: mongo:4.4.3: Especifica a imagem Docker que será utilizada para o serviço do MongoDB. Nesse caso, a imagem oficial do MongoDB na versão 4.4.3 será baixada e utilizada.

ports: Define a configuração de mapeamento de porta entre o host e o container. Neste caso, a porta 27017 do host é mapeada para a porta 27017 do container. Isso permite que você acesse o MongoDB no host através da porta 27017.

volumes: Especifica os volumes Docker a serem utilizados pelo serviço. Neste caso, o volume chamado mongo_data é montado em /data/db dentro do container do MongoDB. Isso garante que os dados do MongoDB sejam persistidos mesmo que o container seja reiniciado.

networks: Define a rede à qual o serviço do MongoDB será conectado. Neste caso, o serviço está conectado à rede mongo-network, que foi definida anteriormente no arquivo de configuração.

environment: Permite configurar variáveis de ambiente dentro do container do MongoDB. Aqui, são definidas duas variáveis de ambiente:

MONGO_INITDB_ROOT_USERNAME: Define o nome de usuário raiz (root) do MongoDB como "mongouser".
MONGO_INITDB_ROOT_PASSWORD: Define a senha do usuário raiz (root) do MongoDB como "mongopwd".

Essa parte do código configura o serviço do MongoDB no ambiente Docker, definindo a imagem, as portas, os volumes, a rede e as variáveis de ambiente necessárias para inicializar o MongoDB com um nome de usuário e senha específicos. Essas configurações permitem que você acesse e administre o MongoDB por meio do serviço "mongo-express" ou de qualquer outra aplicação que precise se conectar ao banco de dados.




-----------------------------------------------------------------

Essa foi a configuração básica de um ambiente Docker com MongoDB e Mongo Express. Ele cria dois serviços em uma rede compartilhada, permitindo que o Mongo Express se conecte ao MongoDB e seja acessível através da porta 8081 do host. Os dados do MongoDB são persistidos usando um volume para garantir a preservação dos dados, mesmo que os containers sejam reiniciados.


Abra seu navegador e acesse http://localhost:8081. Você será direcionado para a interface web do Mongo Express, onde poderá visualizar e administrar o banco de dados MongoDB.

