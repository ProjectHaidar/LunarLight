# Lunar Light

[showcase](https://youtu.be/wR-dfwmnezg)

## Protocol

Protocol is strict, it means that if wrong data supplied, server will just drop the connection.<br><br>
![LLP](llproto.PNG)

## Arguments
You <b>must</b> supply one of the arguments<br>
`--install` or `-i` to initialize database structure<br>
`-c <pathtocfg>` to supply config file<br>

## Config File
Base config file structure:<br>
`CLIENTPORT=3333` port number for bots<br>
 `COMMNDPORT=4440` port for server setup<br>
 `THREADS=1000` number of threads, that server should serve simultaneously<br>
 `DBPORT=3306` database port<br>
 `DBUSER=lunarlight` database username<br>
 `DBPASS=lunarlight` database password<br>
    `DBHOST=localhost` host where database engine is running<br>

## Clients
Each client is being assigned with unique id and aes 128bit key.<br>
Key is different for every client, however it's possible to get the same key.<br>
Clients should connect to server's port 3333

## Command interface 
First thing you need to do is to connect on server's port 3330<br>
Therefore, you must proceed the authentication.<br>
Current login is "lunar" and the password is "light"<br>

To get current commands list type "help"<br>

## Server
When new client connects to server, server follows these steps
1. Generates random key
2. Creates an entry in the database with client's IP, key and unique id
3. Sends to client his unique id, and base64 encoded aes key. id and key are delimited by semicolon like that<br>
`'52:Infq7SqZKgLEXObKJfSggA=='`<br>
where 52 is UID and the rest is key
4. Afterwards server immediately closes the connection
<br><br>

When client sends his unique id, server does this:
1. Querying database for key, where id is which client sent.
2. If there is no entry with such id, server just drops the connection
3. Else if there is an entry with such id, server grabs it's encryption key.
4. Server encrypts current command with key from database, decoding it from base64 in advance
5. Then server just sends the encrypted command.
