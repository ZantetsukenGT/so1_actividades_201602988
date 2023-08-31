# Instrucciones

1. Darle permisos de ejecución al script y al archivo de servicio
```bash
$ chmod a+x script.sh
$ chmod a+x actividad4-201602988.service
```

2. Ajustar el atributo `ExecStart` del archivo `actividad4-201602988.service` para que apunte el archivo `script.sh` que está en este directorio.

3. Copiar el archivo `actividad4-201602988.service` en `/etc/systemd/system/`
```bash
$ sudo cp ./actividad4-201602988.service /etc/systemd/system/
```

4. Iniciar el servicio
```bash
$ sudo systemctl start actividad4-201602988.service
```

5. Ver el estado del servicio
```bash
$ sudo systemctl status actividad4-201602988.service

● actividad4-201602988.service - Actividad4-201602988
     Loaded: loaded (/etc/systemd/system/actividad4-201602988.service; static)
     Active: active (running) since Wed 2023-08-30 22:09:04 CST; 3s ago
   Main PID: 5353 (script.sh)
      Tasks: 2 (limit: 38101)
     Memory: 588.0K
        CPU: 2ms
     CGroup: /system.slice/actividad4-201602988.service
             ├─5353 /bin/bash /home/oescobar/Documents/so1_actividades_201602988/actividad4/script.sh
             └─5354 sleep 10

Aug 30 22:09:04 debian-pc systemd[1]: Started actividad4-201602988.service - Actividad4-201602988.
```

6. Ver los logs del servicio
```bash
$ sudo journalctl -u actividad4-201602988 -e

Aug 30 22:09:14 debian-pc script.sh[5353]: Saludos, soy Ozmar Escobar con carnet 201602988, y la fecha actual es  Wed Aug 30 22:09:14 CST 2023
Aug 30 22:09:24 debian-pc script.sh[5353]: Saludos, soy Ozmar Escobar con carnet 201602988, y la fecha actual es  Wed Aug 30 22:09:24 CST 2023
Aug 30 22:09:35 debian-pc script.sh[5353]: Saludos, soy Ozmar Escobar con carnet 201602988, y la fecha actual es  Wed Aug 30 22:09:35 CST 2023
Aug 30 22:09:45 debian-pc script.sh[5353]: Saludos, soy Ozmar Escobar con carnet 201602988, y la fecha actual es  Wed Aug 30 22:09:45 CST 2023
Aug 30 22:09:55 debian-pc script.sh[5353]: Saludos, soy Ozmar Escobar con carnet 201602988, y la fecha actual es  Wed Aug 30 22:09:55 CST 2023
```

7. Detener el servicio
```bash
$ sudo systemctl stop actividad4-201602988.service
```

8. Borrar el servicio
```bash
$ sudo rm /etc/systemd/system/actividad4-201602988.service
```
