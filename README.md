# Como Configurar o Arduino 1.6.4 e o Digispark ATtiny85 no Linux Mint (com Layout ABNT2)

## Introdução

Neste artigo, vamos abordar o processo completo de instalação do Arduino 1.6.4 no Linux Mint(Ou qualquer distribuição linux baseada em debian, funciona pra outras também mas terá de usar  gerenciador de pacotes apt), configuração da Digispark ATtiny85 e adaptação de um script para teclado no layout ABNT2. Se você encontrou dificuldades ao compilar códigos ou configurar a biblioteca **DigiKeyboard**, este guia ajudará a resolver os principais problemas.

---

## 1. Removendo Instalações Anteriores do Arduino

Se você já tem versões anteriores do Arduino instaladas e deseja começar do zero, remova todos os arquivos relacionados:

```bash
sudo rm -rf /opt/arduino
sudo rm -rf ~/.arduino15
sudo rm -rf ~/Arduino
sudo rm -rf ~/.local/share/Trash/files/arduino*
```

Além disso, remova os atalhos antigos:

```bash
sudo rm -f /usr/local/bin/arduino
```

---

## 2. Instalando o Arduino 1.6.4

Na págia do arduino acesse o "older releases" ou o link [https://www.arduino.cc/en/software/OldSoftwareReleases](https://www.arduino.cc/en/software/OldSoftwareReleases) e baixe a versão 1.6.4 pra sua distibuição.\
Se você já possui o arquivo **arduino-1.6.4-linux64.tar.gz** na pasta Downloads, extraia e mova para o diretório correto:

```bash
tar -xf ~/Downloads/arduino-1.6.4-linux64.tar.gz -C ~/Downloads
sudo mv ~/Downloads/arduino-1.6.4 /opt/arduino
sudo ln -s /opt/arduino/arduino /usr/local/bin/arduino
```

Agora, abra o Arduino pela primeira vez para verificar se está funcionando:

```bash
arduino
```

---

## 3. Instalando as Dependências para Digispark ATtiny85

O suporte ao Digispark não vem por padrão, então precisamos instalar os pacotes necessários. Execute:

```bash
mkdir -p ~/.arduino15/packages
cd ~/.arduino15/packages
git clone https://github.com/digistump/DigistumpArduino.git
```

Agora, adicione as placas Digispark ao Arduino com a URL correta para o Gerenciador de Placas:

1. Abra o Arduino IDE.
2. Vá para **Arquivo** > **Preferências**.
3. No campo **URLs Adicionais para Gerenciador de Placas**, adicione:
   ```
   https://raw.githubusercontent.com/digistump/DigistumpArduino/master/package_digistump_index.json
   ```
4. Clique em **OK**.
5. Vá para **Ferramentas** > **Placa** > **Gerenciador de Placas** e instale o **Digistump AVR Boards**.

Após isso, reinicie o Arduino IDE.

---

## 4. Corrigindo Problemas de Bibliotecas

Se ao compilar você encontrar erros como *DigiKeyboard.h: No such file or directory*, verifique se a biblioteca está corretamente instalada:

```bash
find / -type f -name DigiKeyboard.h 2>/dev/null
```

Caso a biblioteca esteja no local errado, mova para a pasta correta:

```bash
mkdir -p ~/Arduino/libraries/DigisparkKeyboard
cp ~/.arduino15/packages/digistump/hardware/avr/1.6.7/libraries/DigisparkKeyboard/DigiKeyboard.h ~/Arduino/libraries/DigisparkKeyboard/
```

Agora, tente compilar novamente.

---

## 5. Configurando o Layout do Teclado para ABNT2

Para que o layout do teclado funcione corretamente no padrão ABNT2, o arquivo **scancode-ascii-table.h** deve ser reescrito com o conteúdo disponível no seguinte link:

```
https://github.com/jcldf/digisparkABNT2/blob/master/scancode-ascii-table.h
```

O arquivo original está em /home/usuário/.arduino15/packages/digistump/hardware/avr/1.6.7/libraries/DigisparkKeyboard\
\
Antes de substituir o arquivo, recomenda-se fazer uma cópia do original para restaurá-lo caso o computador-alvo tenha um layout US:

```bash
cp ~/.arduino15/packages/digistump/hardware/avr/1.6.7/libraries/DigisparkKeyboard/scancode-ascii-table.h \
   ~/.arduino15/packages/digistump/hardware/avr/1.6.7/libraries/DigisparkKeyboard/scancode-ascii-table.h.bak
```

Agora substitua pelo novo arquivo baixado do link acima.

---

## 6. Criando um Script para Digispark no Layout ABNT2

Agora que tudo está configurado, vamos adaptar um script para teclado ABNT2. O código abaixo abre um link no navegador e maximiza a tela:

```cpp
#include "DigiKeyboard.h"

void setup() {
  // vazio
}

void loop() {
  DigiKeyboard.delay(2000);
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT);
  DigiKeyboard.delay(600);
  DigiKeyboard.print("https://youtu.be/dQw4w9WgXcQ?t=43s");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(5000);
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT);
  DigiKeyboard.delay(3000);
  DigiKeyboard.print("http://fakeupdate.net/win10ue");
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(2000);
  DigiKeyboard.sendKeyStroke(KEY_F11);
  for(;;) { /* loop infinito */ }
}
```

Este script foi adaptado para manter compatibilidade com o layout **ABNT2**, garantindo que os atalhos e símbolos funcionem corretamente.

---

## Conclusão

Seguindo este guia, você conseguirá instalar corretamente o Arduino 1.6.4, configurar o suporte para Digispark ATtiny85 e corrigir problemas comuns de bibliotecas. Além disso, agora você tem um exemplo de código funcional adaptado para o teclado ABNT2.

Se você encontrou algum problema ou quer compartilhar sua experiência, deixe um comentário! 🚀

