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

1) Neste tutorial será mostrado como instalar o Ansible no Ubuntu 18.04.
Informações sobre como instalar o Ansible em outras distribuições GNU/Linux podem ser encontradas nos links a seguir.

* http://ansible-br.org/primeiros-passos/guia-rapido/
* https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

No Ubuntu 18.04:

```bash
sudo apt update
sudo apt install -y software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
```

2) Verifique a versão do Ansible, do python e dos arquivos de configuração.

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

3) Verifique a conectividade entre o Ansible Control e o host local.

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

4) Foi criada uma máquina virtual também executando o Ubuntu 18.04 64 bits para ser gerenciada pelo Ansible. É nela que serão executados os conteineres docker.

5) Baixe o conteúdo deste repositório com os seguintes comandos.

```bash
apt install -y git
cd /tmp
git clone https://github.com/aeciopires/ansible-containers
cd ansible-containers
```

Neste diretório foi criado o arquivo [``production_hosts``](https://github.com/aeciopires/ansible-containers/blob/master/production_hosts) contendo as informações de um grupo de host, o IP do host, usuário do SSH e algumas variáveis a serem aplicadas apenas nos hosts que fazem parte do grupo. Altere os valores de acordo com o seu ambiente.

6) Configure a relação de confiança do SSH usando par de chaves RSA entre o host que tem o Ansible instalado e host remoto. Siga o passo 4 desse [tutorial](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04#step-four-%E2%80%94-add-public-key-authentication-(recommended))

7) Remova o arquivo ``passwd.yml``.

```bash
rm passwd.yml
```

8) Use o comando ``ansible-valt`` para criar uma nova senha para encriptografar e descriptgrafar o conteúdo do arquivo a ser criado novamente.

```bash
ansible-vault create passwd.yml
```
Defina a senha do vault. Lembre-se dela pois será usada constantemente.

Em seguida, será exibido um editor de texto para informar o conteúdo do arquivo ``passwd.yml``.

Informe o seguinte conteúdo, alterando o termo SENHA pela que deve ser usada para acessar o sudo de todos os hosts do grupo monitoring com o usuário definido na variável ``ansible_user`` do arquivo ``production_hosts``.

```
sudo_password: SENHA
```
Salve o arquivo.

APENAS se precisar alterar a senha do sudo futuramente, use o comando a seguir.

```bash
ansible-vault edit passwd.yml
```

APENAS se quiser alterar a senha do vault, use o comando a seguir.

```bash
ansible-vault rekey passwd.yml
```

9) Verifique se a máquina virtual remota pertencente ao grupo ``monitoring`` está acessível para o Ansible com o seguinte comando.

```bash
ansible -i production_hosts  --ask-vault-pass --extra-vars '@passwd.yml' -m ping monitoring
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

10) Ainda no diretório ``ansible-containers``, existe o arquivo [``playbook.yml``](https://github.com/aeciopires/ansible-containers/blob/master/playbook.yml) contendo o nome do grupo de hosts alvo e a sequência de roles e tasks a serem executadas.

O comando a seguir deve ser executado para aplicar o estado desejado nos hosts do grupo ``monitoring``.

```bash
ansible-playbook -i production_hosts --ask-vault-pass --extra-vars '@passwd.yml' playbook.yml
```

11) Algumas dicas e truques para usar o Ansible, estão publicadas [aqui.](http://ansible-br.org/primeiros-passos/guia-rapido/passo-7/)

Fonte:
* https://8gwifi.org/docs/ansible-sudo-ssh-password.jsp
* https://www.cyberciti.biz/faq/how-to-set-and-use-sudo-password-for-ansible-vault/
* http://ansible-br.org
* https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04
* https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
* https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html
* https://raymii.org/s/tutorials/Ansible_-_Only_if_on_specific_distribution_or_distribution_version.html
* https://docs.docker.com/install/linux/docker-ce/ubuntu/
* https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
* https://dev4devs.com/2018/10/29/ansible-how-to-include-tasks-and-variables-file-definitions/
* https://www.slideshare.net/jtyr/variable-precedence-where-should-i-put-a-variable
* https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html
* https://www.toptechskills.com/ansible-tutorials-courses/ansible-include-import-tasks-tutorial-examples/
* https://www.cyberciti.biz/faq/how-to-set-and-use-sudo-password-for-ansible-vault/

Bons testes!

# English


Good tests!

## Developers

developer: Aecio dos Santos Pires<br>
mail: http://blog.aeciopires.com/contato/

## License

GPL-3.0 2019 Aécio dos Santos Pires
