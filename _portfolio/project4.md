---
title: Instalando o Zabbix via Puppet
subtitle:
image:
alt:

caption:
  title: Instalando o Zabbix via Puppet
  subtitle:
  thumbnail: assets/img/portfolio/blog2.png
  alt: Icons made by [Cornelia Springer](https://www.pngitem.com/userpic/13649/) from [Pngitem](https://www.pngitem.com/middle/iwhTmbo_blogging-png-transparent-png/)
---
OBS.: Este tutorial foi executado usando o Puppet 4.x, Zabbix 3.2 e as VMs do Debian 8 criadas para o livro [Gerência de Configuração com Puppet](https://novatec.com.br/livros/puppet/). Para a execução deste tutorial, é assumido que você executou todos os passos do livro até o capítulo 5.

O [Zabbix](http://zabbix.com) é um serviço de gerenciamento e monitoramento de aplicações e equipamentos de rede. No Puppet ele pode ser gerenciado usando o módulo puppet-zabbix. Nos passos a seguir será apresentada uma configuração bem simples para este serviço no Debian. Mais opções e exemplos de configuração deste módulo podem ser encontrados na página [https://forge.puppet.com/puppet/zabbix](https://forge.puppet.com/puppet/zabbix).

1) Acesse o Puppet Server e execute o comando a seguir para instalar o módulo **puppet-zabbix**.

```bash
puppet module install puppet-zabbix
```

2- Ainda no Puppet Server, edite o arquivo ``/etc/puppetlabs/code/environments/production/manifests/site.pp`` e defina a seguinte configuração para gerenciar o serviço Zabbix no host **node1** usando o módulo e as dependências instaladas anteriormente.

```bash
node node1.domain.com.br {
 #Configurando o Apache 
 class { 'apache':
   mpm_module       => 'prefork',
   default_vhost    => false,
   server_signature => 'Off',
   server_tokens    => 'Prod',
   trace_enable     => 'Off',
 }

 #Incluindo o suporte ao PHP no Apache
 include apache::mod::php

 #Definindo a porta padrao do HTTP
 apache::listen { '80': }

 #Defindo as cifras e protocolos SSL a serem usados no acesso via HTTPS
   class { 'apache::mod::ssl':
   ssl_cipher   => 'HIGH:MEDIUM:!aNULL:!MD5:!SSLv3:!SSLv2:!TLSv1:!TLSv1.1',
   ssl_protocol => [ 'all', '-SSLv2', '-SSLv3', '-TLSv1', '-TLSv1.1' ],
 }

 #Configurando o modulo wsgi
 class { 'apache::mod::wsgi':
   wsgi_socket_prefix => '/var/run/wsgi',
 }

 #Adicionando o suporte a MySQL
 class { 'mysql::server': }

 #Configurando o Zabbix Server
 class { 'zabbix':
   zabbix_url     => 'node1.domain.com.br',
   database_type  => 'mysql',
   zabbix_version => '3.4',
   apache_use_ssl => true,
 }

 #Configurando o Zabbix Agent
 class { 'zabbix::agent':
  server => 'node1.domain.com.br',
 }
}
```

3- Acesse o host ``node1.domain.com.br`` e execute o comando a seguir para obter o novo catálogo de configuração.

```bash
puppet agent -t
```

4- Verifique o funcionamento do serviço acessando ``https://node1.domain.com.br``. Aceite o certificado auto-assinado e acesse o Zabbix com o login **Admin** e a senha **zabbix**.

Se for necessário, ajuste os valores das opções de configuração no arquivo ``/etc/puppetlabs/code/environments/production/manifests/site.pp`` do host **master** para atender as suas necessidades do seu ambiente.

Para obter documentação sobre o uso do Zabbix, acesse a página [http://zabbixbrasil.org/?page_id=7](http://zabbixbrasil.org/?page_id=7).
