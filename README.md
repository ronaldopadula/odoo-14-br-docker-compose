# Em primeiro lugar:
Fork criado apenas para utilização em demonstração. Tradução ao Português com todos os créditos ao criado do código.

# Instalação simplificada

Instalando o Odoo 14 com apenas um comando.

(Suporta várias instâncias na mesma máquina.)


Instalar primeiramente na sua máquina o [docker](https://docs.docker.com/get-docker/) e o [docker-compose](https://docs.docker.com/compose/install/) para que seja possível fazer a instalação. 

Vamos usar a ferramenta cURL ("URL do cliente"), para fazer as tranferências de dados, assim, devese-se checar se há o cURL instalado:

``` bash
curl --version
```
Se não tiver instalado, fazê-lo com o seguinte código:

``` bash
$ sudo apt-get install curl
# ou
$ sudo yum install curl
```

Agora, rodar a instalação do Odoo14 através do seguinte código:

``` bash
curl -s https://raw.githubusercontent.com/ronaldopadula/odoo-14-br-docker-compose/master/instalar.sh | sudo bash -s odoo14-one 10014 20014
```

para configurar a primeira instância do Odoo14 @ 'localhost:10014' (senha mestra padrão: 'ronaldo.padula')

e

``` bash
curl -s https://raw.githubusercontent.com/ronaldopadula/odoo-14-br-docker-compose/master/instalar.sh | sudo bash -s odoo14-two 11014 21014
```

para configurar uma segunda instância do Odoo @ 'localhost:11014' (senha mestra padrão: 'ronaldo.padula')

Nos links anteriores, :
* Primeiro Argumento (**odoo14-one**): Pasta de deploy do Odoo14
* Segundo argumento (**10014**): Porta para o Odoo
* Terceiro argumento (**20014**): porta para o live chat

# Usando

Iniciar o container:
``` sh
docker-compose up
```

* Posteriormente abrir `localhost:10014` para acessar a instalação do Odoo 14. Se precisar inciar o servidor Odoo em uma porta diferente, mudar **10014** para outro valor referente a porta escolhida no arquivo **docker-compose.yml**:

```
ports:
 - "10014:8069"
```

Executar o contêiner Odoo no modo desanexado (ser capaz de fechar o terminal sem parar Odoo):

```
docker-compose up -d
```

**Se receber mensagem com o problema de permissão**, altere a permissão da pasta para certificar-se de que o contêiner é capaz de acessar o diretório:

``` sh
$ git clone https://github.com/ronaldopadula/odoo-14-br-docker-compose.git
$ sudo chmod -R 777 addons
$ sudo chmod -R 777 etc
$ mkdir -p postgresql
$ sudo chmod -R 777 postgresql
```

Para evitar erros quando executamos várias instâncias do Odoo., deve-se aumentar o número máximo de arquivos em execução de 8192 (padrão) para 524288. Esta é uma etapa opcional, que no ubuntu é feita com os seguintes comandos:

```
$ if grep -qF "fs.inotify.max_user_watches" /etc/sysctl.conf; then echo $(grep -F "fs.inotify.max_user_watches" /etc/sysctl.conf); else echo "fs.inotify.max_user_watches = 524288" | sudo tee -a /etc/sysctl.conf; fi
$ sudo sysctl -p    # apply new config immediately
```

# Custom addons

A pasta addons/ conportará addons personalizados. Basta colocar seus addons personalizados, se você tiver algum.

# Configuração e log do Odoo

* Para mudar as configurações do odoo, editar o arquivo: **etc/odoo.conf**.
* O arquivo de log do Odoo é : **etc/odoo-server.log**
* O password padrão da base de dados (**admin_passwd**) é `ronaldo.padula`, mas você pode mudar isso em @ [etc/odoo.conf#L60](/etc/odoo.conf#L60)

# Para administrar os containeres

**Run Odoo**:

``` bash
docker-compose up -d
```

**Restart Odoo**:

``` bash
docker-compose restart
```

**Stop Odoo**:

``` bash
docker-compose down
```

# Live chat

Em [docker-compose.yml#L21](docker-compose.yml#L21), nós expomos a porta **20014** para o host do live-chat.

Configurar o **nginx** para ativar o recurso de live-chat (em produção):

``` conf
#...
server {
    #...
    location /longpolling/ {
        proxy_pass http://0.0.0.0:20014/longpolling/;
    }
    #...
}
#...
```

# docker-compose.yml

* odoo:14.0
* postgres:13

# Odoo 14 screenshots

![odoo-14-welcome-docker](screenshots/odoo-14-welcome-screenshot.png)

![odoo-14-apps-docker](screenshots/odoo-14-apps-screenshot.png)

![odoo-14-sales](screenshots/odoo-14-sales-screen.png)

![odoo-14-form](screenshots/odoo-14-sales-form.png)
