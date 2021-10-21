---
title: Boxes do Vagrant para o livro
subtitle:
image:
alt:

caption:
  title: Boxes do Vagrant para o livro
  subtitle:
  thumbnail:
---
O Vagrant é um software que permite criar máquinas virtuais (para Virtualbox ou Vmware) de forma simples para agilizar a criação de ambientes de testes e desenvolvimento. Mais informações podem ser obtidas no site [https://www.vagrantup.com](https://www.vagrantup.com)

Para automatizar e simplificar a criação de máquinas virtuais com Vagrant, você pode criar um arquivo chamado Vagranfile e nele definir as configurações da mesma. Mais informações sobre esse arquivo podem ser obtidas na página: [https://www.vagrantup.com/docs/vagrantfile](https://www.vagrantup.com/docs/vagrantfile)

As VMs são criadas no Vagrant usando boxes que são como templates de um sistema básico pronto. Várias boxes podem ser encontradas na página: [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

Para a execução das boxes homologadas para o livro, siga os passos abaixo.

1) Instale o [Virtualbox](https://www.virtualbox.org/wiki/Downloads) e o [Vagrant](https://www.vagrantup.com/downloads.html).

2) Instale o plugin ``vagrant-disksize`` no GNU/Linux com o seguinte comando.

```bash
vagrant plugin install vagrant-disksize
```

3) Baixe as configurações criadas especialmente para o livro na página: [https://gitlab.com/livro/puppet/tree/master/vagrant](https://gitlab.com/livro/puppet/tree/master/vagrant)

Nesta página, você encontrará a configuração realizada para o host master nas distros: CentOS, Debian e Ubuntu, além dos manifests Puppet que serão executados nas máquinas virtuais quando forem criadas.

4) Dentro dos diretórios do master tem o arquivo Vagrantfile com as especificações e configurações da máquina virtual. O arquivo contém explicações detalhadas sobre algumas variáveis e sobre a configuração da mesma. Altere conforme a sua necessidade.

5) Esses Vagranfiles só criarão a máquina virtual do host master. Para criar máquinas virtuais para os hosts **node1** e **node2**, basta criar outro diretório, copiar o Vagrantfile do master para o novo diretório e alterar o conteúdo no diretório destino conforme a necessidade.

6) Depois execute os comandos abaixo.
Para ligar a VM:

```bash
vagrant up
```

Para acessar a VM via ssh:

```bash
vagrant ssh
```

Para recarregar as configurações da VM:

```bash
vagrant reload
```

Para destruir a VM:

```bash
vagrant destroy
```

7) Para mais informações, acesse a documentação do Vagrant: [https://www.vagrantup.com/docs/](https://www.vagrantup.com/docs/)

Bons testes!
