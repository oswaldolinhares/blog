# VAGRANT - Criando um ambiente de Desenvolvimento

A grande dificuldade em desenvolver aplicações, se dá pela diversidade de ambiente de desenvolvimento existente na equipe do projeto, versões diferentes de S.O, versões de software, etc. Mix de diferentes coisas só é bom em salada de frutas, no desenvolvimento de software isso gera incompatibilidade, é o famoso na minha máquina roda.

E é aí que o vagrant é de um auxílio primordial, com ele você consegue criar um ambiente único de desenvolvimento, fazendo com que o software rode em todas as máquinas da equipe.

## Primeiros Passos

### Instalar Virtualbox

* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) - Baixe e Instale

### Instalar Vagrant

* [Vagrant](https://www.vagrantup.com/downloads.html) - Baixe e Instale

### Windows: Putty e Puttygen

Se você é usuário windows baixe esses dois aplicativos para acessar o Vagrant via ssh

* [Puttygen](http://the.earth.li/%7Esgtatham/putty/latest/x86/puttygen.exe) - Baixe

* [Putty](http://the.earth.li/%7Esgtatham/putty/latest/x86/putty.exe) - Baixe

* [Chave Vagrant](https://github.com/mitchellh/vagrant/raw/master/keys/vagrant) - Baixe

```
Execute o Puttygen clique no botão LOAD e procure a chave do vagrant que você baixou
```

```
Clique no Botão "Save Private Key", clique em YES na janela que vai abrir
```

```
Salve o arquivo com o nome vagrant.ppk em: C:\Usuários\onomedoseusuario\.vagrant.d
```

## Iniciar ambiente para máquina virtual

Vamos agora de fato criar uma máquina virtual

Abra o Terminal ou o Prompt se estiver no Windows

Crie uma pasta onde ficará sua máquina virtual

```
mkdir minhabox
```

Entre na pasta

```
cd minhabox
```
Execute o comando

```
vagrant init
```

Instale o plugin VBGUEST

```
vagrant plugin install vagrant-vbguest
```

## Vagrantfile

Após a execução do comando init um arquivo de nome vagrantfile é criado na pasta, ele que servirá de guia para a sua máquina virtual

Esse é o meu vagrant file
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
    config.ssh.insert_key = false

  config.vm.synced_folder "projetos", "/home/vagrant/projetos", type: "virtualbox"

  config.vm.network :forwarded_port, guest: 1234, host: 1234   #Node
  config.vm.network :forwarded_port, guest: 3000, host: 3000   #Rails
  config.vm.network :forwarded_port, guest: 3306, host: 3306   #Mysql
  config.vm.network :forwarded_port, guest: 4567, host: 4567   #Sinatra
  config.vm.network :forwarded_port, guest: 5432, host: 5432   #Postgresql
  config.vm.network :forwarded_port, guest: 80, host: 8080     #Apache/Nginx
  config.vm.network :forwarded_port, guest: 8888, host: 8888   #Jasmine
  config.vm.network :forwarded_port, guest: 27017, host: 27017 #Mongodb

end
```
### Vagrant Cloud - "config.vm.box = "ubuntu/bionic64"

O vagrant cloud é um repositório disponibilizado pela vagrant para que você armazene suas box, além disso há muitas box neles com ambientes de outros usuários já configurados

Essa configuração do vagrantfile permite que você baixe diretamente do cloud uma box, eu escolhi a ubuntu/bionic64 que é a que vem o ubuntu zerado para fazermos as configurações

Para outras boxes acesse:

* [Vagrant Cloud](http://app.vagrantup.com/boxes/search)


### Meu Editor - config.vm.synced_folder

Embora o Vagrant tenha o excelente editor VIM, ás vezes você que poder usar o seu editor do seu sistema operacional, ás vezes você gastou alguns dólares num editor profissional

Essa configuração do vagrantfile permite que você sincronize a pasta do seu projeto da máquina virtual com a sua máquina real, no seu sistema operacional.

Nessa configuração abaixo, estou sincronizando a pasta projetos dentro de vagrant no home, com a pasta projetos onde está a minha máquina virtual

config.vm.synced_folder "projetos", "/home/vagrant/projetos", type: "virtualbox"


### Abrindo Portas - config.vm.network :forwarded_port, guest: 1234, host: 1234

Essa configuração do vagrantefile faz uma ligação das portas do máquina virtual com as da sua máquina

## Ligando a máquina virutal

Execute o comando abaixo

```
vagrant up
```
Se você fez tudo direitinho o Vagrant vai baixar do site dele a box e instalará na sua máquina


## Acessando a Máquina Virtual pelo ssh ou pelo Putty

### Linux

Se você usar o Linux basta digitar:

```
vagrant ssh
```
### Windows

Execute o programa putty

Em Category>Session entre com os seguintes dados: Host Name: vagrant@127.0.0.1 e Port: 2222

Em Category>SSH>Auth clique em Browse e selecione o arquivo vagrant.ppk em: C:/Users/nomedousuário/.vagrant.d/vagrant.ppk.

Para não ter que ficar repetindo esse procedimento vá em Category>Session em Saved Sessions digite um nome qualquer e pressione o botão Save

Clique em Open e depois pressione o botão Yes, sua conexão com servidor foi iniciada

Agora é configurar seu ambiente, baixando os pacotes necessários


## Compartilhando a Box

### Criando a Box

Entre com o comando abaixo no prompt ou no terminal dentro da pasta onde o vagrant está:

```
vagrant package
```
Vai ser gerado um arquivo package.box que você pode distribuir

### Importando a Box

Na máquina nova você instala o virtualbox e vagrant, copia o arquivo box gerado para alguma pasta, e nessa pasta você executa o seguinte comando

```
vagrant box add
```
Depois crie a pasta onde ficará a máquina virtual e execute o comando INIT

```
vagrant init
```

## Autor

* **Oswaldo Linhares** - [oswaldolinhares](oswldolinhares@gmail.com)


## Mensagem

Se esse guia foi útil a você, por favor me dê uma star(estrela) pra eu saber e me incentivar a escrever mais.

:) Grato

