
Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux nos protocolos de rede FTP (transferência de arquivos), HTTP (web) e SMB (compartilhamento de recursos).

Elaborei este passo-a-passo em tópicos para facilitar a localização:

#Configuração Inicial
#Simulando Ataque FTP
#Simulando Ataque web (http)
#Simulando Ataque SMB (Server Message Block)


#Configuração Inicial

1º - Instalar o Kali Linux e o Metasploitable no Virtual Box.

2º - Iniciar os dois e fazer um snapshot no metasploitable para recuperar o trabalho em caso de falha da máquina. 
Como? Na guia do virtualbox com o metasploitable aberto clicar em máquina > criar snapshot > adicionar nome e descrição do snapshot > clicar em ok.

3º - Acessar o metasploitable com o login padrão: msfadmin e senha padrão: msfadmin

4º - Acionar o comando ifconfig no metasploitable e anotar o ip da máquina que estará na linha inet addr. Este será o ip utilizado para os testes. 

5º - Alcançando a máquina vulnerável no metasploitable

Como? Abra o Terminal do Kali Linux e digite o comando ping -c 3 nú.me.ro.ip

Se houver resposta saberemos que a comunicação entre as duas máquinas está funcionando corretamente.


#Simulando Ataque FTP
Simulando um cenário de auditoria em um servidor FTP que pode conter falhas de segurança

1º - Enumeração para descobrir quais serviços estão disponíveis no sistema com suspeita de vulnerabilidade. 
comando: nmap -sV -p 21,22,80,445,139 nú.me.ro.ip 

Este comando escaneia as portas 21,22,80,445 e 139. O parâmetro -sV identifica a versão do serviço que está rodando em cada porta.

Se a porta ftp estiver aberta tentaremos conectá-la diretamente.

2º - Conectando diretamente ao ftp para confirmar se está ativo.

Comando: ftp nú.me.ro.ip

Caso a conexão aconteça pedirá o login e a senha. Como ainda não sabemos nenhum dos dois precisaremos fazer um ataque brute force (força bruta) utilizando a ferramenta Medusa para tentar descobri-los. Antes disso temos que criar duas listas: uma com possíveis nomes de usuários e outra com senhas comuns. 

Para sair do ftp é só digitar quit e apertar enter e para limpar a tela do terminal é só digitar clear e apertar enter


Criando nomes de usuários e senhas comuns (wordlists) em diferentes arquivos e rodando o ataque

1º - Comandos para criar e salvar no Kali Linux arquivo de texto com possíveis nomes de usuários e arquivo com senhas comuns.

Comando usuários: echo -e "user\nmsfadmin\nadmin\nroot" > users.txt  

Comando senhas: echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

2º - Rodando o ataque com a Medusa

Comando: medusa -h nú.me.ro.ip -U users.txt -P pass.txt -M ftp -t6 

Onde -t6 significa que estamos usando 6 threads simultâneas, o que torna o ataque mais rápido.

No ataque foram encontrados o login msfadmin e a senha msfadmin como credenciais válidas. Isso significa que conseguimos acessar o sistema via ftp com essas credenciais. 

3º - Validando manualmente o acesso via ftp com as credenciais encontradas 

Comando: ftp nú.me.ro.ip

#Simulando Ataque web (http)
Simulando ataque brute force em formulários de login web (http) no sistema dvwa

1º - Acessar, no navegador firefox do Kali Linux, o endereço nú.me.ro.ip/dvwa/login.php para visualizar a página de teste de login do dvwa. 

Na sequência abrir o painel de ferramentas do desenvolvedor na página de teste de login do dvwa clicando em f12 e em seguida clicar na guia network, na navegação do tipo POST e em Request, que nos mostrará tudo o que o navegador está enviando e recebendo durante a interação, incluindo os nomes dos parâmetros que o servidor espera receber. A Medusa vai simular em cima destes parâmetros.

2º - No terminal do Kali, após criadas as wordlists de usuários e de senhas, rodar o seguinte comando com a Medusa.

Comando: medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6

As credenciais corretas encontradas aparecerão com a palavra SUCCESS.

Em seguida utilizamos o primeiro login e senha encontrados para acessar o sistema.

#Simulando Ataque SMB (Server Message Block)
Simulando ataques de enumeração e spraying contra o serviço SMB (Server Message Block)

1º - Rodar a enumeração de usuários com enum4linux

Comando: enum4linux -a 192.168.56.101 | tee enum4_output.txt 

2º - Na sequência podemos abrir o arquivo do comando que acabamos de rodar e visualizar usuários que sejam possíveis alvos de ataques. O número rid é o identificador relativo do usuário no sistema. Sempre que houver nomes de usuários genéricos como null ou interrogação geralmente são de usuários mais vulneráveis. 

Comando: less enum4_output.txt

3º - Criando wordlists de usuários

No comando anterior conseguimos acesso aos nomes dos usuários reais, agora precisamos criar nosso arquivo de alvos e nosso arquivo de senhas. 

Comando: echo -e "user\nmsfadmin\nservice" > smb_users.txt

Comando: echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray

Ao contrário do brute force, que testa muitas senhas em um único usuário, o password spraying testa poucas senhas em muitos usuários.  

4º - Rodando ataque com Medusa

Comando: medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50  

Onde: 
-h é o IP do nosso alvo.
-U é a lista de usuários descoberta na enumeração.
-P é a lista de senhas fracas.
-M smbnt é o módulo específico para ataques via smb.
-t 2 é uma das duas threads simultâneas, que simulam 2 usuários testando senhas.
-T 50 significa até 50 hosts em paralelo.

5º - Testando o acesso utilizando smbclient

Verifica se teremos acesso como administrador através das credenciais encontradas no ataque anterior.

Comando: smbclient -L //192.168.56.101 -U msfadmin 


