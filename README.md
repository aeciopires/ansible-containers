# saltstack-examples

[Português]: #português
[English]: #english
[Developers]: #developers
[License]: #license

#### Menu

1. [Português][Português]
2. [English][English]
3. [Developers][Developers]
4. [License][License]

# Português

O Ansible é uma ferramenta de automação de TI radicalmente simples que automatiza o provisionamento em nuvem, o gerenciamento de configuração, a implantação de aplicativos, a orquestração entre serviços e muitas outras necessidades de TI. Fonte: https://docs.ansible.com/ansible/latest/dev_guide/overview_architecture.html

Ela é uma alternativa a ferramentas como: SaltStack (https://www.saltstack.com/),
Puppet (https://puppet.com) e Chef (https://www.chef.io/chef).

Na página a seguir você encontra vários links de estudo e documentação que estou
usando no aprendizado desta ferramenta.

http://blog.aeciopires.com/primeiros-passos-com-ansible/

O objetivo do código compartilhado neste repositório é...

Descrição dos diretórios deste repositório:

* bla bla bla
* bla bla bla
* bla bla bla

# Instalando e Configurando o Ansible

Neste tutorial será mostrado como instalar o Ansible no Ubuntu 18.04.
Informações sobre como instalar o Ansible em outras distribuições GNU/Linux podem ser encontradas nos links a seguir.

http://ansible-br.org/primeiros-passos/guia-rapido/
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

No Ubuntu 18.04:

```bash
sudo apt update
sudo apt install -y software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
```

Verifique a versão do Ansible, do python e dos arquivos de configuração.

```bash
ansible --version
```

O resultado deve ser algo semelhante a:

```
  ansible 2.8.5
    config file = /etc/ansible/ansible.cfg
    configured module search path = [u'/home/aecio/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/dist-packages/ansible
    executable location = /usr/bin/ansible
    python version = 2.7.15+ (default, Jul  9 2019, 16:51:35) [GCC 7.4.0]
```

Verifique a conectividade entre o Ansible Control e o host local.

```bash
ansible localhost -m ping
```

O resultado deve ser algo semelhante a:

```
  localhost | SUCCESS => {
      "changed": false, 
      "ping": "pong"
  }
```

# Gerenciando Conteineres Docker com Ansible

Foi criada uma máquina virtual também executando o Ubuntu 18.04 64 bits para ser gerenciada pelo Ansible. É nela que serão executados os conteineres docker.

Baixe o conteúdo deste repositório com os seguintes comandos.

```bash
apt install -y git
cd /tmp
git clone https://github.com/aeciopires/ansible-containers
cd ansible-containers
```

Neste diretório foi criado o arquivo [``remotehosts``](https://github.com/aeciopires/ansible-containers/blob/master/remotehosts) contendo as informações de acesso a máquina virtual. Altere de acordo com o seu ambiente de testes.

Configure a relação de confiança do SSH usando par de chaves RSA entre o host que tem o Ansible instalado e host remoto. Siga o passo 4 desse [tutorial](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04#step-four-%E2%80%94-add-public-key-authentication-(recommended))

Verifique se a máquina virtual remota está acessível para o Ansible com o seguinte comando.

```bash
ansible -i remotehosts -m ping monitoring
```

O resultado deve ser algo semelhante a:

```
  192.168.56.70 | SUCCESS => {
      "ansible_facts": {
          "discovered_interpreter_python": "/usr/bin/python3"
      }, 
      "changed": false, 
      "ping": "pong"
  }
```


Os comandos a seguir devem ser executados para aplicar o estado desejado nos hosts.

```bash

```

Bons testes!

# English


Good tests!

## Developers

developer: Aecio dos Santos Pires<br>
mail: http://blog.aeciopires.com/contato/

## License

GPL-3.0 2019 Aécio dos Santos Pires
