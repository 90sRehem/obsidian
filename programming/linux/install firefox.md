#linux #terminal 

Para instalar a última versão do Firefox no Linux, faça o seguinte:

Passo 1. Abra um terminal;  
Passo 2. Caso já tenha feito alguma instalação manual, apague a pasta, o link e o atalho anterior com esse comando;

```
sudo rm -Rf /opt/firefox*
```

```
sudo rm -Rf /usr/bin/firefox
```

```
sudo rm -Rf /usr/share/applications/firefox.desktop
```

Passo 3. Confira se o seu sistema é de 32 bits ou 64 bits, para isso, use o seguinte comando no terminal:

```
uname -m
```

Passo 4. Se seu sistema for de 32 bits, use o comando abaixo para baixar o programa. Se o link estiver desatualizado, acesse [essa página](https://download.mozilla.org/?product=firefox-latest&os=linux&lang=pt-BR) e baixe a última versão e salve-o com o nome firefox.tar.bz2:

```
wget "https://download.mozilla.org/?product=firefox-latest&os=linux&lang=pt-BR" -O firefox.tar.bz2
```


Passo 5. Se seu sistema for de 64 bits, use o comando abaixo para baixar o programa. Se o link estiver desatualizado, acesse [essa página](https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=pt-BR) e baixe a última versão e salve-o com o nome firefox.tar.bz2:

```
wget "https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=pt-BR" -O firefox.tar.bz2
```

Passo 6. Execute o comando abaixo para descomprimir o pacote baixado, para a pasta /opt/;

```
sudo tar -jxvf  firefox.tar.bz2 -C /opt/
```

Passo 7. Renomeie a pasta criada. Se ao executar o comando abaixo ocorrer um erro com a mensagem iniciando com “mv: é impossível sobrescrever o não-diretório”, pule este passo;

```
sudo mv /opt/firefox*/ /opt/firefox
```

Passo 8. Finalmente, crie um atalho para facilitar a execução do programa;

```
sudo ln -sf /opt/firefox/firefox /usr/bin/firefox
```

Passo 9. Se seu ambiente gráfico atual suportar, crie um lançador para o Firefox, usando comando abaixo;

```
echo -e '[Desktop Entry]\n Version=59.0.3\n Encoding=UTF-8\n Name=Mozilla Firefox\n Comment=Navegador Web\n Exec=/opt/firefox/firefox\n Icon=/opt/firefox/browser/chrome/icons/default/default128.png\n Type=Application\n Categories=Network' | sudo tee /usr/share/applications/firefox.desktop
```

Pronto! Agora, quando quiser iniciar o programa, digite `firefox` em um terminal, seguido da tecla TAB.

Já se a sua distribuição suportar, coloque o atalho na sua área de trabalho usando o gerenciador de arquivos do sistema ou o comando abaixo, e use-o para iniciar o programa.

```
sudo chmod +x /usr/share/applications/firefox.desktop
```

```
cp /usr/share/applications/firefox.desktop  ~/Área\ de\ Trabalho/
```

Se seu sistema estiver em inglês, use este comando para copiar o atalho para sua área de trabalho:

```
cp /usr/share/applications/firefox.desktop ~/Desktop
```