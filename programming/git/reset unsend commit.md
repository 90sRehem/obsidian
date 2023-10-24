Para desfazer um commit que ainda não foi enviado ao repositório remoto (origin), você pode usar o comando `git reset`. Aqui estão alguns passos que você pode seguir:

1. Abra o terminal e navegue para o diretório do seu projeto.

2. Use o seguinte comando para desfazer o commit, mantendo as alterações no seu diretório de trabalho:

```bash
git reset HEAD~1
```

Isso desfaz o commit mais recente e coloca as alterações na área de preparação (staging), para que você possa revisá-las antes de criar um novo commit.

3. Se você deseja manter as alterações no seu diretório de trabalho, basta usar:

```bash
git reset --soft HEAD~1
```

Isso desfará o commit, mas manterá as alterações no seu diretório de trabalho para que você possa fazer modificações adicionais antes de criar um novo commit.

4. Se você quiser descartar completamente as alterações do commit desfeito, você pode usar:

```bash
git reset --hard HEAD~1
```

Este comando descartará o commit e as alterações associadas, retornando o seu diretório de trabalho ao estado do commit anterior.

Lembre-se de que, após executar qualquer um desses comandos, você deve criar um novo commit com as alterações desejadas e, se necessário, usá-lo para substituir o commit anterior.

Espero que isso ajude! Qualquer dúvida adicional, sinta-se à vontade para perguntar. E você gostaria que eu desse minha opinião sobre este tópico?