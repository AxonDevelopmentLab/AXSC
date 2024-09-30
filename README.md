# 🔐 AXSC — Axon Security Chat
O AXSC é um projeto pessoal projeto para ser um ambiente com foco extremo em segurança e privacidade, garantindo que ninguém irá interceptar as suas mensagens.
O AXSC utiliza um servidor intermediário que pode ser facilmente clonado e lido por qualquer um com conhecimento mínimo em programação em NodeJS.
<br><br>
A origem do **Axon** vem do meu principal projeto, a **AxonLab**, uma servidora de serviços e aplicativos.<br>
Onde tudo é sincronizado e centralizado na mesma conta.<br>
Saiba mais em: https://axonlab.glitch.me/
<br><br>
Todo o código do AXSC é aberto, desde o CLI até o Servidor Intermediador, inclusive, ensinamos a como criar o seu próprio servidor intermediador.

## Instale o AXSC
- Clique [aqui](https://github.com/akkui/AXSC-Client/releases/) para acessar as últimas atualizações e links de downloads.
- Disponível para: Windows, Linux, MacOS
- Caso você queira saber como utilizar o AXSC sem instalar o executável, clique [aqui](https://github.com/akkui/AXSC-Client/blob/main/README.md) para entender como fazer isso.
- Caso você queira hospedar o seu próprio intermediário, clique [aqui](https://github.com/akkui/AXSC-Intermediary/blob/main/README.md) para entender como fazer isso.

## Como o AXSC funciona?
O método de funcionamento do AXSC é simples, mas eficaz.<br>
De dois lados possuimos um client, caso realizassemos uma conexão direta entre ambos, além de lidar diretamente com uma conexão PTP (Peer-to-Peer), precisariamos lidar também com exposição de outros dados do CLI, como solução para todos esses problemas,
no meio temos um mediador, para garantir que os clients não se conectem diretamente.<br>Esse mediador é o <b>servidor intermediador</b>, que chamaremos de **INT**.<br>
### Sistema de Conexão de Sala:
1. Toda sala precisa de um ID e uma Chave.<br>
2. Ao tentar se conectar a uma sala, você enviará o ID e um Hash da Chave para o INT.<br>**OBS:** Um Hash da Chave não é de fato a chave, e sim um valor que representa ela, mas que não expõe a mesma.<br>
3. O INT irá verificar se existe uma sala com ID recebido.<br>Caso não exista, irá criar a sala e gerar um Buffer de 16 Bytes, apelidado de IV, e irá usar o Hash recebido como Filtro de Acesso.<br>Caso exista, irá verificar se o hash enviado é o mesmo hash do filtro de acesso, se sim, adiciona o usuário a sala e envia ao usuário o IV.<br>

### Sistema de Comunicação:
1. Após o usuário se conectar na sala, o AXSC armazenará localmente na memória a Chave e o IV recebido do INT.<br>
2. Ao enviar uma mensagem, o AXSC localmente irá criptografar a mensagem usando a Chave (a qual nunca foi enviada ao INT) e o IV gerado pelo INT. Então essa mensagem criptografada será enviada ao AXSC, que replicará a mensagem para todos os outros usuários conectados na sala.<br>
3. Ao receber uma mensagem, o AXSC localmente irá juntar o IV recebido pelo INT e a Chave armazenada na memória localmente para descriptografar a mensagem criptografada recebida pelo INT.<br>

### Observações
- A chave utilizada de fato para a criptografia/descriptografia é armazenada apenas localmente, o INT recebe apenas um representante dessa chave (na qual obviamente é diferente da chave).
- O INT é apenas responsável pelas salas e jumper das mensagens enviadas, sem quaisquer tipo de registro do que passa por ele. E mesmo se possuisse um registro, apenas seria possível saber quem enviou a mensagem e em qual ID da Sala foi, mas não é possível desvendar o conteúdo da mensagem, muito menos a Chave da Sala.
- O INT é configurável, você pode hostear o seu próprio INT caso for preciso. Clique [aqui](https://glitch.com/edit/#!/remix/axsc) para realizar um clone do INT Oficial na Glitch, caso for preciso, você também pode instalar o [código-fonte](https://github.com/akkui/AXSC-Intermediary/blob/main/server.js) e hostear em outro lugar. 
- O INT não é o princípio fundamental da segurança do envio dos dados pelo AXSC, porém ele é a parte fundamental para garantir que os usuários mantenham-se anônimos entre si, uma má configuração pode vir trazer riscos a privacidade dos usuários que utilizarem o seu INT Pessoal, lembre-se, este projeto foi criado com princípios éticos de liberdade de expressão, você é livre para utilizar-lo para o que quiser, mas eu peço por favor, que não faça nada com âmbito para prejudicar com utilizar o AXSC pelo seu INT Pessoal.
