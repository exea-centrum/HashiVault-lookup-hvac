# HashiVault-lookup-hvac
vault operator unseal
k8s
kubectl exec vault-1 -- vault login hvs
or 
if use docker
docker exec -it xxxxxxx /bin/sh
vault login

vault secrets enable -tls-skip-verify -path=ansible kv
##################################
vault kv put -tls-skip-verify ansible/serverlab/production/db \
   dbusername=wp_user \
   dbpassword=davtro123 \
   dbhost=vault.exampe.local \
   dbname=blog
######################################################################

$ vault kv get ansible/serverlab/production/db
======= Data =======
Key           Value
---           -----
dbhost        vault.exampe.local
dbname        blog
dbpassword    passwd123
dbusername    wp_user
##################################################################################

ansible.hcl

$ nano ansible.hcl
path "ansible/*" {
  capabilities = [ "read", "list" ]
}
#################################################################

$ vault policy write ansible ansible.hcl
Success! Uploaded policy: ansible
#####################################################################

$ vault token create -policy="ansible"
Key                  Value
---                  -----
token                s.XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
token_accessor       XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
token_duration       10h
token_renewable      true
token_policies       ["ansible" "default"]
identity_policies    []
policies             ["ansible" "default"]

################################################
$ ansible-playbook -i hosts -l servers playbook.yml --tags "test"
$ ansible-playbook -i hosts -l servers playbook.yml --tags "update_wp_config"
