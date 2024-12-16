# Administração de Redes de Computadores

*Instituição: IF Goiano - Campus Ceres*  
*Curso: Bacharelado em Sistemas de Informação*  
*Disciplina: Administração de Redes de Computadores*  
*Alunos: Jhannyfer S. R. Biângulo e Rafael de Souza Teixeira*   
*Professor: Roitier Gonçalves*  

# Projeto de Configuração Automática de Serviços com Vagrant

## Objetivo

Este projeto tem como objetivo a criação e configuração de uma máquina virtual utilizando o Vagrant, de forma que a instalação e a configuração de serviços de rede sejam realizadas automaticamente. O intuito é que o usuário execute apenas um comando para iniciar a VM e, após isso, possa realizar os testes dos serviços configurados, conforme descrito neste guia.

### Serviços Configurados:

   * DHCP: Instalação e configuração de um servidor DHCP para fornecimento automático de IPs.
   * DNS (BIND9): Implementação de um servidor DNS para resolução de nomes de domínio.
   * FTP (ProFTPD): Instalação e configuração de um servidor FTP para compartilhamento de arquivos.
   * NFS: Configuração de um servidor NFS para o compartilhamento de diretórios.

Neste documento, você encontrará instruções detalhadas sobre o ambiente, como subir a máquina virtual e testar cada serviço configurado.

## Preparação do Ambiente

### Requisitos:

Certifique-se de que o Vagrant e o VirtualBox estão instalados corretamente em sua máquina:

  Para instalar o Vagrant: [Vagrant Downloads](https://www.vagrantup.com/downloads)  
  Para instalar o VirtualBox: [VirtualBox Downloads](https://www.virtualbox.org/)

Recomendação: Utilize a versão 6.1 do VirtualBox para melhor compatibilidade.

### Verificando a Instalação:

Certifique-se de que os programas estão funcionando corretamente com os seguintes comandos:

    vagrant --version
    vboxmanage --version

### Configurando o Ambiente:

Crie um novo diretório para o projeto:

    mkdir vagrant_config_dhcp_dns_ftp_nfs
    cd vagrant_config_dhcp_dns_ftp_nfs

 Substitua o arquivo Vagrantfile:
        Coloque o arquivo Vagrantfile que você recebeu neste diretório.

### Subindo a VM:

No terminal, execute o seguinte comando para iniciar e configurar a máquina virtual automaticamente:

    vagrant up

Após esse comando, a máquina será configurada com todos os serviços necessários de forma automática. Não será necessário rodar comandos manuais dentro da VM.

### Testando os Serviços

Depois de inicializar a máquina, siga os passos abaixo para testar o funcionamento de cada serviço:

### Serviço DHCP

No cliente:

Verifique se o IP foi atribuído automaticamente:

    ip addr show

O cliente deverá receber um IP na faixa 192.168.10.10-192.168.10.200.

### Serviço DNS (BIND9)

No cliente:

Verifique se a resolução de nomes configurada no servidor DNS está funcionando:

    dig roitier.com.br

O nome roitier.com.br deve ser resolvido corretamente.

### Serviço FTP

No cliente:

Acesse o servidor FTP e liste os arquivos disponíveis:

    ftp 192.168.10.1

Use o login anonymous. O arquivo README.txt deve estar visível no diretório compartilhado.

### Serviço NFS

No servidor:

 Verifique se o serviço NFS está ativo:

    systemctl status nfs-kernel-server

No cliente:

Monte o diretório compartilhado pelo servidor NFS:

    sudo mount 192.168.10.1:/home/storage /mnt

Crie um arquivo no diretório compartilhado:

    echo "Arquivo criado pelo cliente" | sudo tee /mnt/teste_cliente.txt

No servidor:

Verifique se o arquivo criado no cliente está visível no servidor:

    ls /home/storage

O arquivo teste_cliente.txt deverá aparecer.

## Referências e Documentação

[Documentação do Vagrant](https://developer.hashicorp.com/vagrant/docs)
[Documentação do ISC-DHCP-Server](https://manpages.ubuntu.com/manpages/bionic/en/man8/dhcpd.8.html)
[Documentação do BIND9](https://bind9.readthedocs.io/en/latest/)
[Documentação do ProFTPD](http://www.proftpd.org/docs/)
[Documentação do NFS](https://wiki.linux-nfs.org/wiki/index.php/Main_Page)

## Conclusão

Com o Vagrantfile fornecido, todo o processo de configuração é feito automaticamente, permitindo que o usuário se concentre apenas em testar os serviços configurados. A automação na configuração do ambiente garante consistência e facilita o processo.

Se algum serviço não funcionar como esperado, confira as configurações e logs em cada máquina virtual. Consulte as documentações e referências indicadas para mais informações e resolução de problemas.

Este guia apresentou como implementar e testar serviços de rede fundamentais utilizando o Vagrant, proporcionando uma experiência prática no gerenciamento de redes.
