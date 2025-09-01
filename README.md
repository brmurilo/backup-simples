**# Backup Simples via SSH**

Este repositório contém um **script em Bash** para realizar backups automáticos de uma pasta local para um servidor remoto via SSH, com suporte a senha automática utilizando `sshpass`.

O script cria uma **pasta separada para cada execução**, usando data e hora no formato `YYYY-MM-DD_HH-MM-SS`, e registra logs de execução.

Esse script serve como um quebra galho, confiável, porém tem suas limitações de segurança (por inserir a do servidor no próprio script) e por usabilidade, **o rsync é MUITO MAIS CONFIÁVEL.**


✅ Diferença chave:

scp sempre copia tudo, mesmo que não tenha mudado.
rsync só copia o que mudou e ainda pode comprimir dados durante a transferência.

---


**## Pré-requisitos**

- Debian / Ubuntu ou similar
  
- sshpass instalado:
sudo apt update
sudo apt install sshpass -y


- SSH configurado no servidor remoto e usuário de destino criado.
- Permissão para o usuário rodar ssh e scp.


**Configuração do script**
Edite o arquivo backup_simples.sh e configure:

PASTA="/caminho/para/pasta/original"
USUARIO="nome_do_usuario_remoto"
SENHA="senha_do_usuario_remoto"
SERVIDOR="ip_do_servidor_remoto"
O script cria automaticamente um diretório remoto com o timestamp da execução:

DESTINO="/home/$USUARIO/backups/$DATA/"



**Como usar**
Torne o script executável:

chmod +x backup_simples.sh

Execute manualmente:
./backup_simples.sh

Verifique o log de execução:
tail -f /var/log/backup_simples.log


**Agendamento via Cron**
Para rodar automaticamente a cada minuto, adicione no /etc/crontab:

*/1 * * * * root /usr/src/backup_simples.sh >> /var/log/backup_cron.log 2>&1
>> /var/log/backup_cron.log 2>&1 garante que todas as saídas e erros fiquem registradas.

Estrutura de pastas no servidor remoto
Após cada execução, o backup será armazenado em:

/home/backupuser/backups/YYYY-MM-DD_HH-MM-SS/
Cada execução cria uma nova pasta com timestamp.


**Logs**
Log principal do script: /var/log/backup_simples.log

Log do cron: /var/log/backup_cron.log (se configurado no crontab)


**Observações**
Recomenda-se usar um usuário dedicado para backup, não root.
O script utiliza scp -rv para exibir cada arquivo sendo transferido.
A senha é armazenada em texto no script. Para produção, considere chaves SSH sem senha.

