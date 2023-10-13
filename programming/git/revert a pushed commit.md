1. Use o comando `git log` para listar os commits e encontre o hash (identificador único) do commit que deseja reverter.
    
2. Use o comando `git revert <hash_do_commit>` para criar uma reversão do commit. Isso criará um novo commit que desfaz as alterações introduzidas pelo commit original.
    
3. Após criar a reversão, você precisará enviar essa alteração para o repositório remoto. Use `git push origin <nome_da_branch>` para atualizar o repositório remoto com a reversão.