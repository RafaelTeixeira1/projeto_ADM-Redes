Administração de Redes de Computadores

    Instituição: IF Goiano - Campus Ceres
    Curso: Bacharelado em Sistemas de Informação
    Disciplina: Administração de Redes de Computadores
    Alunos: Jhannyfer S. R. Biângulo e Rafael de Souza Teixeira
    Professor: Roitier Gonçalves

Objetivo

Este trabalho visa a criação e configuração de duas máquinas virtuais (VMs) utilizando o Vagrant, automatizando a implementação de serviços de rede. O objetivo é que o usuário apenas execute os comandos necessários para inicializar as VMs e, em seguida, realize os testes de cada serviço conforme descrito neste guia.

    Server: Configurado para fornecer os serviços DHCP, DNS, FTP, Samba, NFS e Nginx.
    Client: Configurado para obter IP via DHCP, utilizar o DNS configurado no servidor e acessar os serviços FTP, Samba, NFS e Nginx.

Este documento apresenta uma explicação detalhada do ambiente, orientações para subir as máquinas virtuais e instruções de teste de cada serviço configurado.
Preparação do Ambiente

Pré-requisitos:

    Certifique-se de que o Vagrant e o VirtualBox estão instalados em seu sistema.
        Para instalar o Vagrant: https://www.vagrantup.com/downloads
        Para instalar o VirtualBox: https://www.virtualbox.org/
        Recomendação: Utilize a versão 6.1 do VirtualBox para maior compatibilidade.

Verificação da Instalação:

    Verifique se os softwares estão instalados corretamente:

    vagrant --version
    vboxmanage --version

Preparar o Ambiente:

    Crie um diretório para o projeto:

    mkdir meu_projeto
    cd meu_projeto

    Substitua o arquivo Vagrantfile pelo arquivo fornecido neste guia.

Iniciar as VMs:

    No terminal, execute o comando abaixo para iniciar e configurar as máquinas virtuais automaticamente:

    vagrant up

        Após este comando, as máquinas serão provisionadas com todos os serviços configurados automaticamente. Não é necessário executar comandos manuais dentro das VMs.

Testando os Serviços

Após a inicialização das VMs, siga as etapas abaixo para verificar o funcionamento de cada serviço:
Serviço DHCP
No cliente:

    Verifique se um IP foi atribuído automaticamente:

    ip addr show

        O cliente deve receber um IP na faixa 192.168.56.10-192.168.56.100.

Serviço DNS (BIND9)
No cliente:

    Teste a resolução de nomes configurada no servidor DNS:

    dig example.local

        O nome example.local deve ser resolvido com sucesso.

Serviço FTP
No cliente:

    Conecte ao servidor FTP e liste os arquivos disponíveis:

    ftp 192.168.56.1

        Use o usuário anonymous durante o login. Você deverá visualizar o arquivo README.txt no diretório compartilhado.

Serviço NFS

No servidor:

    Verifique o status do serviço NFS:

    systemctl status nfs-kernel-server

No cliente:

    Monte o diretório compartilhado pelo servidor NFS:

    sudo mount 192.168.56.1:/srv/nfs_share /mnt

Após montar, crie um arquivo no diretório compartilhado:

echo "Arquivo criado pelo cliente" | sudo tee /mnt/nfs_share/teste_cliente.txt

No servidor:

    Verifique se o arquivo criado no cliente está disponível no servidor:

    ls /srv/nfs_share

        O arquivo teste_cliente.txt deve aparecer na listagem.

Serviço Nginx

No cliente:

    Teste o acesso ao site estático servido pelo Nginx:

    curl http://192.168.56.1

    A página deve exibir a mensagem "Bem-vindo ao Site Estático!". Você também pode acessar o site via navegador utilizando o IP do servidor.

Verificação e Correção Manual (Caso Necessário):

    Se você já aplicou o Vagrantfile corrigido, mas ainda está enfrentando problemas com o Nginx, siga os passos abaixo para corrigir manualmente:

    Acesse a VM Server:

    vagrant ssh server

Verifique a Configuração do Nginx:

    Abra o arquivo de configuração do site:

    sudo nano /etc/nginx/sites-available/site

Certifique-se de que o conteúdo está conforme o modelo abaixo:

server {
    listen 80 default_server;
    server_name _;

    root /var/www/html/site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

Teste a Configuração do Nginx:

sudo nginx -t

    Se houver erros, revise as mensagens exibidas para corrigir quaisquer problemas.

Reinicie o Nginx:

sudo systemctl restart nginx

Referências e Documentações

    Documentação do Vagrant
    Documentação do ISC-DHCP-Server
    Documentação do BIND9
    Documentação do FTP (vsftpd)
    Documentação do Samba
    Documentação do NFS
    Documentação do Nginx

Conclusão

Com o arquivo Vagrantfile fornecido, todo o processo de configuração é realizado automaticamente, permitindo que o usuário foque apenas nos testes de cada serviço. O provisionamento automatizado facilita a criação do ambiente, garantindo consistência e praticidade.

Caso algum serviço não funcione corretamente, revise as configurações e logs correspondentes em cada máquina virtual. Utilize as referências para aprofundar o entendimento e solucionar eventuais problemas.

Este guia demonstrou como implementar e testar os principais serviços de rede utilizando ferramentas modernas como o Vagrant, promovendo um aprendizado prático e eficaz sobre administração de redes.
