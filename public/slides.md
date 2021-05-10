name: capa

.capa-titulo[

# Soluções WEB

]

.capa-subtitulo[

### Prof. Lucas Ferreira

]

---

class: center, middle
count: false

# Servidores HTTP (Apache e NGINX) e Linux _(básico)_

---

## Servidor HTTP

Iniciamos com um _"Servidor WEB"_ que normalmente é o conjunto de hardware + software que disponibiliza arquivos que compõem os sites _(por exemplo, documentos HTML, imagens, folhas de estilo, e arquivos JavaScript)_

Uma das partes de um _"Servidor WEB"_ é o **Servidor HTTP** que é um software que compreende URLs _(endereços web)_ e HTTP _(o protocolo que seu navegador utiliza para visualizar páginas web)_

--

Conforme já vimos anteriormente, o navegador fará uma requisição utilizando o protocolo HTTP(S) sempre que necessitar de um um arquivo hospedado em um _"Servidor WEB"_. Quando a requisição chegar no servidor web correto _(hardware)_, o servidor HTTP _(software)_ enviará o documento requerido, também via HTTP.

.center[.ws-img[![WebServer!](./img/web-server.svg)]]

---

## Servidor HTTP

Como na web não é possível prever a que hora se dará essa conexão, os servidores web precisam estar disponíveis dia e noite.

--

**Para publicar um website, é necessário ou um servidor web estático ou um dinâmico.**

--

Um **servidor web estático** consiste em um computador (hardware) com um servidor HTTP (software). É chamado "estático" porque o servidor envia seus arquivos tal como foram criados e armazenados (hospedados) ao navegador.

--

Um **servidor web dinâmico** consiste em um servidor web estático com software adicional, mais comumente um servidor de aplicações (application server) e possivelmente um banco de dados (database). É chamado "dinâmico" porque o servidor de aplicações atualiza os arquivos hospedados antes de enviá-los ao navegador através do servidor HTTP.

---

Dois dos servidores HTTP mais populares do mercado são o **Apache HTTP Server (httpd)** e o **NGINX**.

De acordo com a pesquisa 2020 da `NetCraft` o qual monitorou **mais de 1.212.139.815** websites ao longo de 2020 a maioria dos servidores hoje usam o servidor HTTP **NGINX**.

.center[.pesquisa-img[![WebServer!](./img/pesquisa.png)]]

---

## Apache HTTP Server _(httpd)_

-   Criado em 1995 pela Apache Software Foundation é até hoje um dos mais populares servidores HTTP da internet

-   Possui versões para diversos sistemas operacionais como: Windows, Linux e Mac

-   Por si só é um servidor HTTP estatíco aonde seu projeto inicial "servia" apenas HTML, CSS, JS e arquivos não-dinâmicos

-   Em pouco tempo ganhou a possibilidade de ser "extendido" através de seus _modules_ hoje é considerado um servidor dinâmico

-   Em algumas plataformas e distribuições UNIX é conhecido apenas como **httpd**

-   É o servidor mais comum em projetos "tudo-em-um" como Wamp, XAMPP e MAMP

-   Funciona muito bem no Windows 🤓

---

## Apache HTTP Server _(httpd)_

-   Possui suporte através de módulos para linguagens adicionais como **PHP** (`mod_php`), **Python** (`mod_python`) e **Ruby** (`mod_ruby`).

--

-   É altamente extensível e de fácil configuração _(usando VirtualHost)_:

```xml
<VirtualHost *:80>
    ServerName www.meudominio.com.br
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    <Directory /var/www/html/>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
</VirtualHost>
```

---

## NGINX

-   Nginx _(lê-se "engine x")_ é um servidor HTTP criado em 2005 por Igor Sysoev

-   É um servidor web rápido, leve, e com inúmeras possibilidades de configuração _(nem todas fáceis)_ para melhor performance

-   Também é notóriamente conhecido por ser mais "leve" no consumo de memória de máquina do que o Apache

-   Por também ser um servidor de proxy reverso pode trabalhar em conjunto com o Apache para projetos avançados. É possível diminuir o consumo de memória do Apache fazendo com que as requisições Web passem primeiro pelo Nginx.

-   Funciona muito bem em plataformas UNIX, e apesar de ter uma versão para Windows não é muito "confiável"

-   Possui uma versão paga chamada _NGINX PLUS_ mas a mais popular eu usada na web é a sua versão open-source

---

## NGINX

-   Assim como o Apache o NGINX por padrão irá servir arquivos estáticos

-   Sua configuração e reutilização para múltiplos sites não é tão fácil quanto o Apache.

-   Mas dada sua característica otimizada de proxy consegue através de `FastCGI` atuar como meio de campo entre a requisição e "sub-servidores" dinâmicos para processamento de linguagens específicas

---

## NGINX

A solução para trabalhar com PHP mais popular "casada" com o NGINX é o _PHP-FPM_.

```config
server {
    listen 80;

    server_name www.meudominio.com;
    root /var/www;
    index index.php index.html index.htm;

    location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            try_files $uri $uri/ /index.php =404;
    }

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    location ~ .php$ {
            try_files $uri =404;

            include /etc/nginx/fastcgi_params;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param SCRIPT_NAME $fastcgi_script_name;
            if (-f $request_filename) {
                    fastcgi_pass unix:/var/run/php5-fpm.sock;
            }
    }
}
```

---

## Servidores HTTP simples _(para testes)_

Existem algumas formas de "levantar" um servidor HTTP de forma rápida e simples para processar arquivos estáticos:

-   Se você tiver o Python instalado em seu computador pode usar:<br />
    `python -m SimpleHTTPServer 8000`

-   Se você tiver o Node.js instalado poderá usar o pacote `serve` com o comando:<br />
    `serve -s`

-   Se você tiver uma versão mais recente do PHP (5.4+) poderá usar:<br />
    `php -S localhost:8000`

---

class: center, middle
count: false

# E o Linux?! 🤔

---

## Linux

Linux é um termo popularmente empregado para se referir a sistemas operacionais que utilizam o _Kernel Linux_

O núcleo do Linux (Kernel) foi criado originalmente por **Linus Torvalds** em 1991 ajudando na árdua tarefa de criar um sistema operacional 100% de código aberto e livre (frente a MacOS e Windows)

--

Grandes empresas usam e colaboram com a iniciativa, empresas como: **IBM, Sun Microsystems, Hewlett-Packard (HP), Red Hat, Novell, Oracle, Google, Mandriva, Microsoft e Canonical**

--

Pensem em Linux como uma base ou um estilo de arquitetura para diversos sistemas operacionais derivados

Um sistema operacional baseado no Kernel do Linux é popularmente conhecido como **distro** sendo que as mais populares do mercado são: `Arch Linux`, `CentOS`, `Debian`, `Fedora Linux`, `Linux Mint`, `openSUSE` e `Ubuntu`

Em seu uso nos servidores WEB, uma distro Linux não precisa ter interface gráfica e normalmente é controlada 100% por linha de comando

---

## Linux

O softwares para Linux podem ser instalados de duas formas: **compilados** através de seu código-fonte OU instalados através de **pacotes** pré-compilados

A maioria das distros modernas possuem um **gerenciador de pacotes** que facilita a instalação de softwares nas plataformas. Nas distros baseadas em Debian temos o `APT` e nas distros baseadas em RPM temos os `YUM`

--

Na atual situação de mercado em 95% dos casos não será necessário compilar qualquer software para servidores web, a grande maioria encontra-se disponível nos gerênciadores de pacotes. Por exemplo para instalar o Apache em um servidor baseado em Debian:

```bash
sudo apt install apache2
```

--

Servidores Linux suportam uma varieade de softwares, não apenas servidores web, como também servidores de e-mail, bancos de dados, firewall, load balancers e etc

--

**Aprendam e usem Linux é de graça e super performático!**

---

## Acessando um servidor Linux Remoto

-   A forma mais comum de acessar um servidor remoto baseado em Linux sem interface gráfica é através do comando `SSH`

-   O Secure Shell (`ssh`) é um protocolo de rede criptográfico para operação de serviços de rede, sendo que sua mais conhecida aplicação é o login remoto de usuários a sistemas de computadores

-   Não detalhando os protocolos de segurança nem as mais diversas formas de usar o `SSH`, focando no básico após abrir o seu Terminal e solicitar uma conexão remota via SSH, você terá acesso total a linha de comando de seu servidor remoto

--

-   A sintaxe mais básica de conexão é:

```bash
ssh usuario@ip-do-servidor
```

--

-   Para utilizar SSH no Windows será necessário um software chamado Putty _(até o Windows 10)_

---

class: center, middle

## DEMO TIME 🤓
