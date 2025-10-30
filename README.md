
## Simulando Ataques de Brute Force de Senhas com Medusa e Kali Linux

![warning image](https://www.sandsteinwandern.de/wandern/wp-content/uploads/2019/11/giphy-hack2.gif) 

### Neste projeto simulei um ataque de Brute Force de Senhas com Medusa e Kali Linux nos protocolos de rede FTP (transferência de arquivos), HTTP (web) e SMB (compartilhamento de recursos).

Um ataque de força bruta é um método cibernético que utiliza tentativa e erro para adivinhar credenciais de login, chaves de criptografia ou páginas da web ocultas. O termo "força bruta" se refere ao uso de softwares automatizados para testar sistematicamente todas as combinações possíveis de caracteres até encontrar a correta. Esse tipo de ataque é uma das técnicas mais antigas e ainda eficazes contra senhas fracas.

Existem diferentes variações do ataque de força bruta, cada uma com uma abordagem específica: 

* Ataque simples de força bruta: O invasor tenta adivinhar a senha de forma manual, usando lógica e dedução para tentar combinações óbvias, como "123456", "senha" ou "data de nascimento". Este tipo de ataque é menos sofisticado, mas ainda funciona contra senhas extremamente fracas.

* Ataque de dicionário: O atacante utiliza listas de palavras (dicionários) para testar possíveis senhas. Essas listas podem incluir palavras comuns em vários idiomas, nomes próprios, termos técnicos e outras informações que as pessoas costumam usar em suas senhas.

* Ataque híbrido: Combina os ataques de dicionário com a força bruta simples. O software do atacante usa uma lista de palavras e, em seguida, adiciona variações a elas, como números no final ("senha123"), maiúsculas no início ("Senha123") ou substituições de caracteres ("s3nha").

* Ataque de pulverização de senhas (Password Spraying): Em vez de tentar várias senhas para uma única conta, o invasor tenta uma ou poucas senhas comuns (por exemplo, "password123") em várias contas diferentes. Essa abordagem evita que as contas sejam bloqueadas por um grande número de tentativas falhadas, o que pode ser uma defesa contra ataques de força bruta tradicionais.

* Ataque de força bruta reversa: O invasor começa com uma senha conhecida (geralmente obtida em vazamentos de dados anteriores) e tenta associá-la a milhões de nomes de usuário até encontrar a combinação correta.



  

