#linux #jwt #openssl

Passo 1: Abra o terminal
Você pode usar o terminal do Linux para executar os comandos necessários. Certifique-se de estar logado como um usuário com permissões adequadas.

Passo 2: Gere o par de chaves
Use o comando `openssl` para gerar o par de chaves RSA-256. A chave privada será armazenada em um arquivo seguro, enquanto a chave pública será usada para criptografar.

```bash
openssl genpkey -algorithm RSA -out private_key.pem -aes256
```

Este comando gerará a chave privada e a criptografará com AES-256 para maior segurança. Você será solicitado a definir uma senha para proteger a chave privada.

Passo 3: Gere a chave pública
Agora, extraia a chave pública do par de chaves gerado anteriormente:

```bash
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

Você será solicitado a fornecer a senha que definiu para a chave privada.

Passo 4: Verifique as chaves geradas
Você pode verificar as chaves geradas usando os seguintes comandos:

Para verificar a chave privada:

```bash
openssl rsa -check -in private_key.pem
```

Para verificar a chave pública:

```bash
openssl rsa -pubin -in public_key.pem -text -noout
```

Isso deve mostrar informações detalhadas sobre as chaves geradas.

Passo 5: Converter a chave privada para Base64:

```bash
openssl base64 -in private_key.pem -out private_key_base64.txt
```

Isso converterá a chave privada (que está no arquivo `private_key.pem`) para o formato Base64 e salvará o resultado em um arquivo chamado `private_key_base64.txt`. Lembre-se de que a chave privada ainda está protegida por senha e não é recomendado compartilhá-la em texto simples.

Passo 6: 2. Converter a chave pública para Base64:

bashCopy code

```bash
openssl base64 -in public_key.pem -out public_key_base64.txt`
```

Isso converterá a chave pública (que está no arquivo `public_key.pem`) para o formato Base64 e salvará o resultado em um arquivo chamado `public_key_base64.txt`.


outra forma

```
# Gerar a chave privada
openssl genpkey -algorithm RSA -out private.key -pkeyopt rsa_keygen_bits:2048

# Gerar a chave pública
openssl rsa -pubout -in private.key -out public.key -outform PEM

# Converter a chave privada para base64
JWT_PRIVATE_KEY=$(openssl base64 -in private.key -A)

# Converter a chave pública para base64
JWT_PUBLIC_KEY=$(openssl base64 -in public.key -A)

# Adicionar as chaves ao arquivo .env
echo "JWT_PRIVATE_KEY=\"$JWT_PRIVATE_KEY\"" >> .env
echo "JWT_PUBLIC_KEY=\"$JWT_PUBLIC_KEY\"" >> .env

# Remover os arquivos de chave
rm private.key public.key
```