# ModoQuiosque

1. Instalando o Raspberry Pi OS Lite (32-bit) sem interface gráfica e sem programas adicionais:

    1.1. Baixe o software Raspberry Pi Manager para instalação do SO no raspberry. [Link para download](https://www.raspberrypi.com/software/).

    1.2. Instale o **Raspberry Pi OS Lite (32-bit)** (sem GUI) no cartão de memória do raspberry.

2. Após instalado, atualize os pacotes e aplique as novidadas:

```
# apt update
# apt upgrade -y
```

4. Instale os pacotes necessários:

```
# apt-get update -qq

# apt-get install --no-install-recommends xserver-xorg-video-all xserver-xorg-input-all xserver-xorg-core xinit x11-xserver-utils chromium-browser unclutter
```

5. Crie o arquivo **/home/pi/.bash_profile** e insira:

```
if [ -z $DISPLAY ] && [ $(tty) = /dev/tty1 ]
then
  startx
fi
```

6. Configure o login automático:

    6.1. Digite ```sudo raspi-config```, selecione **"System Options"**, selecione **"Boot / Auto Login"** e depois **"Console Autologin"**

7. Crie o arquivo **/home/pi/.xinitrc** e insira:

```
#!/usr/bin/env sh
xset -dpms
xset s off
xset s noblank

unclutter &
chromium-browser http://192.168.2.69:8080/guacamole \ 
  --window-size=1920,1080 \ 
  --window-position=0,0 \
  --start-fullscreen \
  --kiosk \
  --incognito \
  --noerrdialogs \
  --disable-translate \
  --no-first-run \
  --fast \
  --fast-start \
  --disable-infobars \
  --disable-features=TranslateUI \
  --disk-cache-dir=/dev/null \
  --overscroll-history-navigation=0 \
  --disable-pinch
 
 ```

- Caso a tela se apresente de forma estranha (cortada, distorcida ou pequena), ajuste os parâmetros abaixo:

1. Acesse o menu do raspberry pelo comando raspi-config,
2. Selecione "Display Options", "D1 Resolution"
3. Escolha a opção "CEA Mode 16 1920x1080 60Hz 16:9"


- Caso tenha um so e queira desabilitar a interface gráfica, execute:

```
# systemctl set-default multi-user.target
```


Referência: 
[blog R0b](https://blog.r0b.io/post/minimal-rpi-kiosk/)
