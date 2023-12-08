# HashiVault-lookup-hvac
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
