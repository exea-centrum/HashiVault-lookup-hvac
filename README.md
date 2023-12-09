# HashiVault-lookup-hvac

<li> vault operator unseal </li>
<li>  k8s  </li>
<li>  kubectl exec vault-1 -- vault login hvs  </li>
<li>  or  </li>
<li>  if use docker  </li>
<li> docker exec -it xxxxxxx /bin/sh  </li>
<li>  vault login  </li>

# vault secrets enable -tls-skip-verify -path=ansible kv
 
<li> vault kv put -tls-skip-verify ansible/serverlab/production/db \ </li>
<li>   dbusername=wp_user \ </li>
<li>   dbpassword=davtro123 \ </li>
<li>   dbhost=vault.exampe.local \ </li>
<li>   dbname=blog </li>
<li>

# $ vault kv get ansible/serverlab/production/db
<li> ======= Data ======= </li>
<li> Key           Value </li>
<li> ---           ----- </li>
<li> dbhost        vault.exampe.local </li>
<li> dbname        blog </li>
<li> dbpassword    DavTro123 </li>
<li> dbusername    wp_user </li>


# ansible.hcl

<li> $ nano ansible.hcl </li>
<li> path "ansible/*" { </li>
<li>  capabilities = [ "read", "list" ] </li>
<li> } </li>


# $ vault policy write ansible ansible.hcl
<li> Success! Uploaded policy: ansible </li>


# $ vault token create -policy="ansible"
<li> Key                  Value </li>
<li> ---                  ----- </li>
<li> token                s.XXXXXXXXXXXXXXXXXXXXXXXXXXXXX </li>
<li> token_accessor       XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX </li>
<li> token_duration       10h </li>
<li> token_renewable      true </li>
<li> token_policies       ["ansible" "default"] </li>
<li> identity_policies    [] </li>
<li> policies             ["ansible" "default"] </li>

# test
<li> $ ansible-playbook -i hosts -l servers playbook.yml --tags "test" </li>
<li> $ ansible-playbook -i hosts -l servers playbook.yml --tags "update_wp_config" </li>
