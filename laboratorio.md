# Construcción del laboratorio
Los siguientes apartados ya vienen configurados en el kali que se encuentra instalado en los equipos de la clase, en caso de que decidas llevar tu propio equipo o replicarlo en casa, deberás realizarlas de la misma forma.

## Configuración de kali
Pasos opcionales
Sé que podrías preferir el uso de zsh sobre bash, si quieres continuar con zsh no hay problema solo ten presente que muchos de los scripts que estaré compartiendo en este repositorio se hicieron sobre bash y no zsh entonces puede que requieran algunos cambios.
```bash
sudo usermod --shell /bin/bash kali
```

En el archivo '/home/kali/.bashrc' agrega las siguientes líneas para guardar en la carpeta logs todas las instrucciones y su resultado ejecutado en la terminal, evidencia y para que recuerden qué hicieron en clase.
```bash
nano ~/.bashrc
```

Esto abrirá un editor de texto y al final deberá agregar las siguientes líneas.
```bash
LOGDIR=${HOME}/logs
LOGFILE=${LOGDIR}/$(date +%F.%H:%M:%S).log

if [[ ! -d ${LOGDIR} ]]; then
   mkdir -p ${LOGDIR}
fi

# Starting a script session
if [[ -z $SCRIPT ]]; then
    export SCRIPT=${LOGFILE}
    echo "The terminal output is saving to ${LOGFILE}"
    script -f -q ${SCRIPT}
fi
```

Cambiando los permisos de la carpeta /opt
```bash
cd /opt/
sudo chown kali .
```

Recomiendo que se emplee docker para montar le laboratorio de forma rápida, copie las instrucciones una por una.

## Instalando docker
Para instalar docker se requiere agregar los repositorios a Kali
```bash
printf '%s\n' "deb https://download.docker.com/linux/debian bookworm stable" | sudo tee /etc/apt/sources.list.d/docker-ce.list 
```
Agregamos la llave de los repositorios  
```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-ce-archive-keyring.gpg
```
Instalando docker y sus componentes
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io 
```
Habilitando docker
```bash
sudo systemctl enable docker --now
```
Dando permiso al usuario kali para el uso de docker
```bash
sudo usermod -aG docker kali
```
Puede que se requiera reiniciar el equipo para que los cambios funcionen

### Instalando Juice Shop
Para instalar Juice Shop, se requiere instalar previamente docker
Descarga de Juice Shop mediante docker
```bash
docker pull bkimminich/juice-shop
```
Ejecución de la Juice Shop
```bash
docker run --rm -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```
Ingresa a la URL http://localhost:3000 para ver si está funcionando correctamente.

### Instalando el lab final de pruebas
Iniciando el laboratorio final
```bash
cd /opt/
git clone [git@github.com:dda-ex/2025-2_SC.git](https://github.com/dda-ex/2025-2_SC.git)
cd 2025-2_SC/
sudo make deploy
```
Deteniendo el laboratorio
```bash
sudo make teardown
```
En caso de tener un problema con el sistema y solamente bajo condiciones particulares destruya el laboratorio
```bash
sudo make clean
```

Tambien se puede instalar casi todo lo previo (la parte de Juice Shop solo se tendria que hacer una vez) y nada de instalar docker porque se hace de forma automatizada, escribiendo en el directorio /opt/2025-2_SC/ 
```bash
sudo make init
````

## Instalación y descarga de metasploitable 2
Debido a limitantes en la imagen de docker de metasploitable 2 y que no todos los servicios son funcionales, se tendrá que descargar y ejecutar la imagen de laboratorio, que podrán encontrar en el siguiente enlace: [https://sourceforge.net/projects/metasploitable/files/Metasploitable2/](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/)

## Easy way
Si esto te confunde puedes descargar la imagen de kali que estaremos usando en clase: [https://www.youtube.com/watch?v=7GO1OZB0UMY](https://www.youtube.com/watch?v=7GO1OZB0UMY) disponible por 7 dias.
