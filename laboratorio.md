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

Recomiendo que se emplee docker para montar le laboratorio de forma rápida, copie las instrucciones una por una.

## Instalando docker
Para instalar docker se requiere agregar los repositorios a Kali
```bash
printf '%s\n' "deb https://download.docker.com/linux/debian bullseye stable" | sudo tee /etc/apt/sources.list.d/docker-ce.list 
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


