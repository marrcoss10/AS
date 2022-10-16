---HISTORIAL---
vim .bashrc
  shopt -s histappend                      # append to history, don't overwrite it
  export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"

source .bashrc
.bash_history > historia.txt

---COPIAR DE GOOGLE CLOUD A MI PC---
scp marcos@35.205.43.164:/home/marcos/mifichero.tar.gz /home/marcos/Escritorio

---1.1.ADMINISTRACIÓN EN LINUX---
-Listar sólo ficheros --> ls -F | grep /
-Listar en orden de modificación --> ls -lt
-Comprimir:
  tar -cvf name.tar /dir
  tar -cvzf name.tar.zp /dir
-Descomprimir:
  tar -xvf name.tar     o     tar -xvf name.tar -C /dir
  tar -xvzf name.tar    o     tar -xvzf name.tar -C /dir
-Variable de entorno --> export VAR="value"
-Modificar extensión --> mv $file "${file.txt}".t
-Sustituir palabra --> sed -i "s/PAL/$palsustituta" $ficherosustituir.txt
-Ordenar por separador --> sort -t ':' -k2 /fich

---1.2.SISTEMAS DE FICHEROS---
-Crear partición y montarla:
  lsblk -e7
  sudo cfdisk /dev/sdb
  sudo mkfs.ext3 /dev/sdb1
  sudo mkdir /disco1
  sudo mount -t ext3 /dev/sdb1 /disco1
  sudo cp -r /var* /disco1
  sudo rezize2fs -M /dev/sdb1
  sudo umount /disco1
-Partición permamente:
  lsblk -e7
  sudo cfdisk /dev/sdb
  sudo mkfs.ext3 /dev/sdb1
  sudo mkdir /disco1
  sudo cp /etc/fstab /etc/fstab.backup
  sudo blkid /dev/sdb1  (UUID)
  sudo vim /etc/fstab
    UUID=X /disco1 etx3 discard,defaults,nofail 0 2
-Volúmenes:
  -sudo vgcreate volumen1 /dev/sdb1 /dev/sdb2
  -sudo lvcreate volumen1 -l 100%FREE -n volumen1
  -sudo mkfs.ext3 /dev/volumen1/volumen1
  -sudo mkdir /miVol
  -sudo mount -t ext3 /dev/volumen1/volumen1 /miVol
  -*sudo pvcrete /dev/sdb3
  -sudo vgextend volumen1/volumen1 /dev/sdb3
  -sudo umount /miVol
  -sudo lvremove /dev/volumen1/volumen1
-RAID:
  -sudo apt install mdadm rsync initramfs-tools -y
  -sudo mdadm --create /dev/md0  -l raid -n 2 /dev/sdb1 /dev/sdb2
  -sudo mdadm --detail /dev/md0
  -sudo mkfs.ext3 /dev/md0
  -sudo mkdir /miRaid
  -sudo mount -t ext3 /dev/md0 /miRaid
  -sudo cp -r /var* /miRaid
  -sudo mdadm /dev/md0 -f /dev/sdb1    (simular fallo)
  -sudo mdadm /dev/md0 -r /dev/sdb1    (eliminar disco roto)
  -sudo mdadm /dev/md0 -a /dev/sdb2    (añadir el otro disco)
  -cat /proc/mdstat
  -sudo umount /miRaid
  -sudo mdadm --stop /dev/md0
-Rsnapshot:
  -sudo apt install rsync rsnapshot
  -sudo vim /etc/rsnapshot.conf
    rsnapshot_root /backups/
    retain hourly 24
    backup marcos@35.205.43.164:/home/ /35.205.43.164/
  -sudo rsnapshot configtest
  -sudo rsnapshot -t hourly 
  -sudo rsnapshot-diff /dir1 /dir2
  
---1.3.MONITORIZACIÓN---
-Prioridad (PR) --> 20 + NI
  -NI --> [-20..19]
-Ordenar PR --> top -o PR
-Stress-ng:
  -sudo apt install stress-ng
  -stress-ng -c1 -t20 (nucleos, segundos)
-Pausar --> kill -STOP PID
-Reanudar --> kill -CONT PID
-Reducir prioridad al mínimo --> renice -n -20 -p PID
-Limitar --> ulimit -t300 (5 minutos)
-Crontab:
  -sudo crontab -u root -e
  -https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/
-Cuotas:
  -sudo apt install quota
  -sudo apt install quotatool
  -sudo apt install linux-image-extra-virtual
  -sudo apt install linux-modules-extra-gcp
  -sudo cfdisk /dev/sdb
  -sudo mkfs.ext4 -O quota /dev/sdb1
  -sudo mkdir /disco1
  -vim /etc/fstab
    /dev/sdb1 /disco1 ext4 defaults,usrquota,grpquota 0 0
  -sudo mount -O remount /disco1
  -sudo quotaon -v /disco1
  -sudo edquota -u marcos (En KB)
  -sudo chown marcos:marcos /disco1
  -quota
  -sudo quotaoff -v /disco1
-Logs:
  -Enviar mensaje a /var/log/syslog --> logger "Hola"
  -Nivel debug:
    sudo vim /etc/syslog.d/50-default.conf
      ssh.debug /var/log/ssh.log
    sudo service rsyslog restart
  -Mail: 
    -##########################
-Benchmarks:
  -wget https://tu-dresden.de/zih/forschung/ressourcen/dateien/projekte/firestarter/FIRESTARTER_2.0.tar.gz
  -sudo mkdir firestarter
  -sudo tar -xvzf FIRESTARTER_2.tar.gz /firestarter 
  -cd /firestarter
  -Ejecutar durante 30s --> ./FIRESTARTER -t 30 -r
  -Ejecuta x instrucción --> ./FIRESTARTER -t 30 -i x
  -Muestra los tipos disponibles en la CPU --> ./FIRESTARTER -a

---2.1.SERVICIOS EN RED---
-NFS:
  -apt update
  -apt install nfs-common (Cliente)
  -apt install nfs-kernel-server nfs-common (Servidor)
  -Compartir una carpeta (Servidor):
    -sudo mkdir /compartir
    -sudo vim /etc/exports
      /compartir *(ro,root_squash,no_subtree_check)
    -sudo chown nobody:nogroup /compartir
    -sudo exportfs -ra
    -sudo exportfs -v
    -service nfs-kernel-server restart
  -Montar la carpeta en /dir (Cliente):
    -sudo mount -t nfs IPSERVIDOR:/compartir /dir
    -df -h
  -Cambiar permisos para que se pueda editar (Servidor):
    -sudo chown marcos:marcos /compartir
-Mosquitto:
  -sudo apt install mosquitto-clients
  -Servidor se subscribe a un topic --> mosquitto_sub -h localhost -t "ciudades/bizkaia"
  -Cliente escribe un mensaje --> mosquitto_pub -h IPSERVIDOR -t "ciudades/bizkaia" -m "Bilbao"
  -Servidor se subscribe a un topic y todos sus subtopic --> mosquitto_sub -h localhost -t "ciudades/#"
  -Publicar un mensje retenido --> mosquitto_pub -h IPSERVIDOR -t "ciudades/bizkaia" -m "Barakaldo" -r

---WHILE---
-i=$((i+1))
-Cuando reciba mensaje --> mosquitto_sub -h IP -t "topic" | while read value; do
  -$value
