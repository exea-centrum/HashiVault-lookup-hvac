# HashiVault-lookup-hvac

<li> vault operator unseal </li>
<li>  k8s  </li>
<li>  kubectl exec vault-1 -- vault login hvs  </li>
<li>  or  </li>
<li>  if use docker  </li>
<li> docker exec -it xxxxxxx /bin/sh  </li>
<li>  vault login  </li>

# vault secrets enable -tls-skip-verify -path=ansible kv
> 
> vault kv put -tls-skip-verify ansible/serverlab/production/db \
>   dbusername=wp_user \
>   dbpassword=davtro123 \
>   dbhost=vault.exampe.local \
>  dbname=blog
> 

# $ vault kv get ansible/serverlab/production/db
> ======= Data =======
> Key           Value
> ---           -----
> dbhost        vault.exampe.local
> dbname        blog
> dbpassword    passwd123
> dbusername    wp_user


# ansible.hcl

> $ nano ansible.hcl
> path "ansible/*" {
>  capabilities = [ "read", "list" ]
> }


# $ vault policy write ansible ansible.hcl
> Success! Uploaded policy: ansible


# $ vault token create -policy="ansible"
> Key                  Value
> ---                  -----
> token                s.XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
> token_accessor       XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
> token_duration       10h
> token_renewable      true
> token_policies       ["ansible" "default"]
> identity_policies    []
> policies             ["ansible" "default"]


> $ ansible-playbook -i hosts -l servers playbook.yml --tags "test"
> $ ansible-playbook -i hosts -l servers playbook.yml --tags "update_wp_config"
