# ansible-infra
ansible-playbook deploy_replication.yml --extra-vars "ansible_sudo_pass=mot de passe"
ansible-playbook deploy_glpi_database.yml --extra-vars "ansible_sudo_pass=mot de passe"
ansible-playbook deploy_hearbeat.yml --extra-vars "ansible_sudo_pass=mot de passe"
ansible-playbook deploy_ldap.yml --extra-vars "ansible_sudo_pass=mot de passe"
ansible-playbook deploy_glpi.yml --extra-vars "ansible_sudo_pass=mot de passe"
ansible-playbook default-iptables-and-update.yml --extra-vars "ansible_sudo_pass=mot de passe"
