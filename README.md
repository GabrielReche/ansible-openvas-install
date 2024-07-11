# Ansible install openvas

## Visão Geral
Este playbook Ansible foi desenvolvido para automatizar a instalação do OpenVAS (Open Vulnerability Assessment System) em servidores remotos. O OpenVAS é uma ferramenta de código aberto projetada para detecção de vulnerabilidades e gestão de vulnerabilidades de segurança.

Neste playbook:

Atualização do índice de pacotes do APT:

O playbook começa atualizando o índice de pacotes do APT nos servidores remotos para garantir que as operações subsequentes usem informações de pacotes atualizadas.
Instalação de pacotes necessários para HTTPS via APT:

Instalação dos pacotes apt-transport-https, ca-certificates, curl e software-properties-common, necessários para permitir ao APT usar repositórios via HTTPS.
Adição da chave GPG oficial do Docker:

Adiciona a chave GPG oficial do Docker ao sistema para garantir a autenticidade dos pacotes baixados do repositório do Docker.
Adição do repositório do Docker:

Adiciona o repositório do Docker ao sistema, permitindo que o APT instale pacotes do Docker diretamente.
Instalação do Docker:

Instala o Docker CE (Community Edition) nos servidores remotos.
Verificação e inicialização do serviço Docker:

Verifica se o Docker está instalado e inicia o serviço se necessário, utilizando o systemd para gerenciar o processo.
Instalação do Docker Compose:

Baixa e instala a versão mais recente do Docker Compose, uma ferramenta utilizada para definir e executar aplicativos Docker de maneira multi-container.
Configuração do ambiente com Docker Compose:

Copia o arquivo docker-compose.yml para o diretório /dl nos servidores remotos. Este arquivo contém a configuração necessária para o ambiente OpenVAS, especificando os serviços e suas dependências.
Execução do Docker Compose:

Utiliza o Docker Compose para construir e iniciar os serviços definidos no arquivo docker-compose.yml. Isso inclui serviços como gvmd (Greenbone Vulnerability Manager Daemon) e outros necessários para o funcionamento do OpenVAS.
Configuração do cron para sincronização automática:

Instala o pacote cron nos servidores remotos e adiciona uma tarefa ao cron para executar o script sync-openvas.sh diariamente às 12:00. Este script é responsável por sincronizar os dados e iniciar o OpenVAS, garantindo que o sistema esteja sempre atualizado e operacional.
Configuração de segurança:

Durante a execução, o playbook também realiza operações de segurança, como adicionar o usuário atual ao grupo Docker para permitir operações sem a necessidade de privilégios de root.
Verificação e depuração:

Após as operações principais, o playbook verifica a versão instalada do Docker e do Docker Compose, além de verificar o status do serviço gvmd para garantir que o OpenVAS esteja funcionando corretamente. Mensagens de depuração são exibidas em caso de erros durante a troca de senha ou outras operações críticas.

## Arquivos
- `inventory/hosts`: Arquivo de inventário com detalhes dos servidores.
- `playbooks/install_openvas.yml`: Playbook para iniciar a instalação.


## Uso
1. Atualize o arquivo `inventory/hosts` com os detalhes do seu servidor.
2. Trocar a senha de admin da interface gráfica.

## IP
IP: Este é o endereço IP do host ao qual o Ansible se conectará para executar as tarefas. Pode ser um endereço IP ou um nome de domínio.

## ansible_user

ansible_user=nomedousuário: Define o nome de usuário usado para se conectar ao host remoto. Neste caso, nomedousuário será usado como o nome de usuário SSH.

## ansible_ssh_pass

ansible_ssh_pass=senhadousuário: Define a senha usada para autenticação SSH. senhadousuário será usada como a senha para se autenticar no host remoto.

## ansible_become_pass

ansible_become_pass=senhadousuário: Define a senha que será usada para elevar privilégios (sudo). Isso é útil quando o usuário precisa executar comandos como superusuário (root).

## ansible_ssh_common_args

ansible_ssh_common_args='-o StrictHostKeyChecking=no': Define argumentos adicionais para a linha de comando SSH. A opção -o StrictHostKeyChecking=no desativa a verificação da chave de host SSH. Isso é útil em ambientes onde os hosts são dinâmicos ou quando você deseja evitar a verificação manual das chaves de host. No entanto, pode ser uma prática insegura, pois desativa uma camada de proteção contra ataques de "man-in-the-middle".


2. Execute o playbook com:

```sh
ansible-playbook -i inventory/hosts playbooks/install_docker.yml



