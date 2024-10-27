# 🚧 MQTT_MOSQUITTO 🚧
Então, pessoal, o principal objetivo do repositório é descomplicar a utilização do protocolo MQTT, mostrar as etapas de instalação/configuração, como enviar e receber valores, 
além de dicas e soluções de possíveis problemas, irei estar utilizando o Broker MOSQUITTO para gerenciamento, mas nada impede de ser outro, porém recomendo iniciar por ele
</br>

![Badge em Desenvolvimento](https://img.shields.io/badge/MQSQUITTO_VERSÃO-2.0.15-yelow?style=flat-square&link=https%3A%2F%2Fgithub.com%2Fwell1ngt0nso%2Fwell1ngt0nso%2Fblob%2Fmain%2FREADME.md)
![Badge em Desenvolvimento](https://img.shields.io/badge/MQTT_VERSÃO-3.11-yelow?style=flat-square&link=https%3A%2F%2Fgithub.com%2Fwell1ngt0nso%2Fwell1ngt0nso%2Fblob%2Fmain%2FREADME.md)
![Badge em Desenvolvimento](https://img.shields.io/badge/INICIATED-2024-blue?style=flat-square&link=https%3A%2F%2Fgithub.com%2Fwell1ngt0nso%2Fwell1ngt0nso%2Fblob%2Fmain%2FREADME.md)

 
O que veremos:
* O que é o protocolo MQTT:
* O QUE É UM BROKER:
* Como instalar e configurar o Mosquitto
* Como utilizar, efetuando Publish e Subscribe
* Solução para alguns problemas
## O QUE É O PROTOCOLO MQTT?
É uma forma de comunicação entre duas ou mais partes que reune todo um conjunto características, definições ou regras para que a comunicação seja bem efetuada. Em outras palavras o protocolo MQTT é como se fosse um idioma que permite a comunucação entre sistemas. Temos diferentes protocolos existentes, alguns que demandam uma comunicação física (i2C) outros apenas a conexão wirelles (http), o protocolo MQTT se encaixa nesse último. Pensado em termos de diferenciação podemos destinguir os mesmos como diferentes idiomas, cada qual com sua especificidade. Optei iniciar pelo MQTT por sua simples manipulação e por já está a muito temppo no mercado.


## O QUE É UM BROKER?

O broker nada mais é que  o intermediário, quem faz o gerenciamento da comunicação entre ambas as partes. Esse sistema pode estar instalado em um servidor ou em uma máquina local, no nosso caso estaremos instalando no PC (nosso servidor temporário) e por meio dele vamos estabelecer a comunicar entre Esp32, Esp8266, Arduino, Telefone, PC, TV....

<p align="center">
  <img src="https://github.com/well1ngt0nso/MQTT_MOSQUITTO/assets/58373332/ddd58aa1-eebd-4cf9-93bf-d6ba39e8df64" alt="Descrição da imagem" width="50%" />
</p>

A imagem acima estabelece diretamente como é feita a comunicação, temos um Broker (Intermediário) e os clientes que podem publicar ou receber algo..
O protocolo MQTT funciona por meio de tópicos, assim conseguimos diferenciar a natureza dos valores além de criar ramificações(veremos isso)

Entendendo os tópicos:
Quando trabalhamos com MQTT estamos trabalhando com envio e recebimento de dados, mas como eu diferencio os dados que envio? 
Por exemplo, suponha que eu tenha um sistema que me envie a temperatura de dois quartos A e B, nos dois quartos temos dispositivos que estabelecem comunicação com meu Broker, um desses quartos envia a temperatura, como diferencio? Utilizamos tópicos! Entenda tópico como o local onde o dado foi publicado, o nome é da escolha do desenvolvedor, basta acrescentar a barra "/" (/quartoA e /quartoB), ou seja, quando recebo um dado eu sei de qual tópico ele veio. O ato de publicar em um tópico também é muito conhecido no termo em inglês "PUBLISH".

Agora temos outro problema: Quando eu envio um dado para o Broker todos os clientes conectados recebem? Se eu conectar meu telefone em um Broker de 1000 tópicos, eu automaticamente recebo os dados dos 1000 tópicos? Não, o cliente recebe apenas as mensagens dos tópicos que quer monitorar, o cliente increve-se nos tópicos... também muito conhecido no inglês pelo termo "SUBSCRIBE"

### Processo de inscrição em um tópico:

* O cliente envia uma mensagem SUBSCRIBE para o broker, especificando o tópico em que deseja se inscrever.
* O broker armazena a inscrição do cliente e associa-a ao tópico.
* O broker começa a enviar todas as mensagens publicadas no tópico para o cliente.
### Comportamento do broker após a inscrição:

* O broker monitora continuamente novas mensagens publicadas nos tópicos.
* Quando uma nova mensagem é publicada em um tópico, o broker verifica se há clientes inscritos nesse tópico.
* Se houver clientes inscritos, o broker envia a mensagem para cada um desses clientes

## Instalação

1. **Baixe o Mosquitto MQTT**:
   - Acesse o [site oficial](https://mosquitto.org/download/) do Mosquitto e faça o download do instalador para o seu sistema operacional.

2. **Acesse a pasta do Mosquitto**:
   - Navegue até a pasta onde o Mosquitto foi instalado (ex_windows: `C:\Program Files\mosquitto`).

### Configuração do arquivo `mosquitto.conf`

1. **Abra o arquivo `mosquitto.conf`**:
   - Este é um arquivo de texto que contém várias configurações comentadas.

2. **Entenda as configurações**:
   - Para ativar uma configuração, remova o `#` do início da linha. 
   - Exemplo:
     - Para habilitar o listener na porta 1883, altere:
       ```plaintext
       #listener 1883
       ```
       para
       
       ```plaintext
       listener 1883
       ```

3. **Execução sem modificações**:
   - O Mosquitto pode ser executado sem alterações no arquivo de configuração, mas apenas em `localhost` (127.0.0.1).

### Habilitando Autenticação

1. **Conexão inicial**:
   - Você pode se conectar ao Mosquitto sem senha, por exemplo, usando um ESP8266.

2. **Habilite a autenticação**:
   - Descomente as seguintes linhas no `mosquitto.conf`:
     ```plaintext
     #anonymous true
     #password_file /path/to/passwordfile
     ```
   - Altere para:
     ```plaintext
     anonymous false
     password_file caminho/do/arquivo/de/senha
     ```

## Observações Importantes

- Apenas as linhas que têm espaço antes do `#` são configurações válidas para serem alteradas. 
- Por exemplo, o caso 1 pode ser modificado
     ```plaintext
      # list (caso 1)
      #list (caso 2)
     ```

### Configurando o Firewall

1. **Acesse o Firewall do Windows**:
   - Abra o menu Iniciar e digite `wf.msc` para abrir o Gerenciador de Firewall do Windows.

2. **Criar nova regra**:
   - Clique em "Regras de Entrada" (canto superior esquerdo) e depois em "Nova Regra" (canto superior direito).

3. **Configuração da porta**:
   - Selecione "Porta", clique em "Avançar", selecione "TCP" e insira "1883" no campo de porta.
   - Clique em "Avançar" até concluir, nomeando a regra como "mqtt".
