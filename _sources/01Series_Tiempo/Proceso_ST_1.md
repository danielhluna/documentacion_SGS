# Series de tiempo InSAR en la Supercomputadora Miztli UNAM
---

Este documento describe, los pasos a seguir del flujo de trabajo para realizar una serie de tiempo con InSAR, usando imágenes de satélite de Sentinel 1A, haciendo uso de Miztli, ISCE2+InSAR, y Mintpy.

Se recomienda antes de iniciar leer la documentación de los comandos básicos de Miztli, el cual se puede encontrar en la pagina [Supercomputo de la UNAM](https://www.super.unam.mx/guias-y-manuales).

Se ingresa a Miztli por medio de un usuario y contraseña, accediendo por medio de una conexión SSH, en donde se coloca el nombre del usuario un @ seguido de la dirección a conectar.

```bash
ssh usuario@Direccion IP
```

Los datos que se ingresan es el usuario y contraseña del Dr. Enrique Cabral Cano, que es el encargado del Servicio Geodésico y de Cartografiá Digital, es de su uso exclusivo, y el acceso solo se otorga mediante su autorización.

Para ingresar a Miztli abrimos la terminal de nuestro sistema operativo, en linux la terminal, en windows cmd o powershell, Se escribe el siguiente comandos

```bash
ssh -Y usuario@Direccion IP
```

Pide la contraseña, se ingresa y arroja el mensaje de bienvenida de Miztli, indicando que la conexión fue exitosa.

```bash
usr@compu-name:~$ ssh -Y ecabral@132.247.177.99
ecabral@132.247.177.99's password: 
Last login: Mon Sep  4 13:10:45 2023 from zianya.geofisica.unam.mx
================================================================
   El RESPALDO de informacion es RESPONSABILIDAD del USUARIO
================================================================
___  ____     _   _ _ 
|  \/  (_)   | | | (_) 
| .  . |_ ___| |_| |_ 
| |\/| | |_  / __| | |  Atencion a usuarios: ayuda@super.unam.mx
| |  | | |/ /| |_| | |			     5622-8599	
\_|  |_/_/___|\__|_|_|       Administracion: 5622-8577 5622-8164

================================================================
          Horario de Mantenimiento: Lunes 9:00 a.m. - 12:00 p.m.
        			     Tiempo del Centro de Mexico
================================================================
   El RESPALDO de informacion es RESPONSABILIDAD del USUARIO
================================================================
Atentamente
Area de administracion de supercomputo
================================================================
-bash: setenv: no se encontró la orden
[ecabral@mn325 ~]$
```

Para poder realizar o enviar instrucciones, tenemos que ingresar los siguientes comandos uno a uno

```bash
s.c  
s.bgood
```

Se muestra las siguientes lineas.

```bash
[ecabral@mn325 ~]$ s.c
[ecabral@mn325 ~]$ s.bgood
sourcing /tmpu/ecabral_g/ecabral/apps/rsmas_insar/default_isce22.bash ...
sourcing /tmpu/ecabral_g/ecabral/apps/rsmas_insar/bashfiles/platforms.bash ...
sourcing /tmpu/ecabral_g/ecabral/apps/rsmas_insar/bashfiles/alias.bash ...
sourcing /tmpu/ecabral_g/ecabral/apps/rsmas_insar/bashfiles/custom.bash ...
sourcing /tmpu/ecabral_g/ecabral/apps/rsmas_insar/source_dario_extras.sh  ...
/home/ecabral_g/ecabral
//mn325/home/ecabral_g/ecabral[1003]
```

```{note}
:bulb: Para el uso y la navegación dentro de Miztli, se utilizan las herramientas de bash
```

## 1. Preparación de datos

A continuación, se accede a la carpeta **Temporal**, donde se encuentra el archivo **Templete**, el cual contiene la configuración de los parámetros necesarios para el procesamiento con **ISCE+** y **Mintpy**. Para desplazarse a dicha carpeta, se utiliza el siguiente comando.

```bash
cd $TE
```


```{tip}
`cp` es el comando que sirve para cambiar de directorio y tiene la siguiente estructura.  
`cd ruta_del_directorio`
```

Se muestra el siguiente resultado.

```bash
//mn325/home/ecabral_g/ecabral[1004] cd $TE
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1005] 
```

Dentro de la carpeta podemos ver que hay dentro de ella, podemos usar el comando `dir` o `ls`, nos listan el contenido, se agrego un `-l` al comando, para listar el contenido.

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1005] ls -l
total 196
drwxrwxr-x 2 ecabral ecabral_g  4096 mar  5  2021 Cabral
drwxrwxr-x 2 ecabral ecabral_g  4096 ago 13 10:50 CDMX
drwxrwxr-x 2 ecabral ecabral_g  4096 dic 25  2022 Colima
drwxrwxr-x 5 ecabral ecabral_g 16384 ago 13 10:51 Enrique
-rw-rw-r-- 1 ecabral ecabral_g 75735 ago 30 16:35 example.log
-rw-rw-r-- 1 ecabral ecabral_g 21636 ago 18  2020 isce.log
drwxrwxr-x 2 ecabral ecabral_g  4096 ago 13 10:53 Jalisco
drwxrwxr-x 3 ecabral ecabral_g 12288 jul  5 22:28 Josue
drwxrwxr-x 2 ecabral ecabral_g  4096 mar  6  2021 Katia
drwxrwxr-x 2 ecabral ecabral_g  4096 sep 24  2020 Pachuca
drwxrwxr-x 2 ecabral ecabral_g  4096 mar  6  2023 Uruapan
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1006] 
```

Se crea una carpeta para almacenar el archivo **template**. Para ello, se utilizan los siguientes comandos: `mkdir` para generar la carpeta con el nombre deseado y `cp` para copiar archivos dentro de ella. Como ejemplo, se empleará la carpeta denominada **dan**.

```bash
mkdir nombre_carpeta
cp nombre_carpeta
ls -l
```

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1012] ls -l
total 0
```

Se observa que no se tiene ningún archivo en nuestra carpeta, regresamos a la carpeta templates, usando el comando `cd`, y se busca dentro de las carpetas `ls`, un archivo con .template, en este ejemplo se usa la carpeta CDMX, copiamos el archivo `cp` y lo renombramos `mv`.

```{tip}
El comando `mv` sirve para mover carpetas o archivos.  
Su segundo uso es para cambiar de nombre.
```


Para cambiar el nombre se usa la siguiente convención:


```{important}
Nombre_de_la_ciudad+Sen+Orbita_Relativa(A/D)+T+número_de_órbita_relativa.template
```

Se regresa a la carpeta TEMPLATES, buscar el archivo .template dentro de CDMX

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1013] cd ..
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1014] ls -l CDMX
total 100
-rw-rw-r-- 1 ecabral ecabral_g 2128 ene 11  2021 CDMX_14_17_SenAT78.template
-rw-rw-r-- 1 ecabral ecabral_g 1981 oct 25  2021 CDMX_14_17_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1944 sep 23  2019 CDMX_14_19_SenDT143_1.template
-rw-rw-r-- 1 ecabral ecabral_g 1942 abr 20  2020 CDMX_14_19_SenDT143_2.template
-rw-rw-r-- 1 ecabral ecabral_g 1954 sep 12  2019 CDMX_14_19_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 2128 oct 20  2020 CDMX_14_20oct_SenAT78.template
-rw-rw-r-- 1 ecabral ecabral_g 2130 oct 20  2020 CDMX_14_20oct_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1929 ago 11  2020 CDMX_14_20_SenAT78_2.template
-rw-rw-r-- 1 ecabral ecabral_g 1929 oct  5  2020 CDMX_14_20_SenAT78_3.template
-rw-rw-r-- 1 ecabral ecabral_g 1929 jul 15  2020 CDMX_14_20_SenAT78.template
-rw-rw-r-- 1 ecabral ecabral_g 1930 jul 17  2020 CDMX_14_20_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1929 oct  5  2020 CDMX_14_20_testSenAT78_3.template
-rw-rw-r-- 1 ecabral ecabral_g 1979 jun 14  2022 CDMX_14_21_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 2065 jun 21  2022 CDMX_14_22_SenAT78.template
-rw-rw-r-- 1 ecabral ecabral_g 1980 ago 11 07:58 CDMX_14_22_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1980 mar  2  2023 CDMX_17_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1980 feb 28  2023 CDMX_1_month_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1979 dic 15  2021 CDMX_SenDT143_14_21.template
-rw-rw-r-- 1 ecabral ecabral_g 1979 dic 20  2021 CDMX_SenDT143_all_2021.template
-rw-rw-r-- 1 ecabral ecabral_g 1948 abr 14  2020 CDMXSenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 1954 sep 10  2019 CDMXSenDT143_V2.template
-rw-rw-r-- 1 ecabral ecabral_g 1947 abr 14  2020 CDMXSenDT41.template
-rw-rw-r-- 1 ecabral ecabral_g 1980 feb  9  2022 CDMX_test_SenDT143.template
-rw-rw-r-- 1 ecabral ecabral_g 7920 ago  9 11:22 example.log
-rw-rw-r-- 1 ecabral ecabral_g    0 oct  5  2020 isce.log
```

Se copia del archivo a la carpeta creada cambiando el nombre, se puede ahorrar el paso de renombrar desde el comando `cp`, colocando el nombre en el directorio destino.

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1015] cp CDMX/CDMXSenDT41.template dan/
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1016] cd dan/
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1017] ls -l
total 1
-rw-rw-r-- 1 ecabral ecabral_g 1947 sep  6 19:08 CDMXSenDT41.template
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1018] mv CDMXSenDT41.template TorreonSenDT12_2015.template
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1019] ls -l
total 1
-rw-rw-r-- 1 ecabral ecabral_g 1947 sep  6 19:08 TorreonSenDT12_2015.template
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1020]
```

Dentro de la carpeta se abre el archivo .template, se puede usar el editor de texto vim o el nano.

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1020] nano TorreonSenDT12_2015.template
```

Se muestra el siguiente archivo.

```bash
######################################################
cleanopt                            = 0          # [ 0 / 1 / 2 / 3 / 4]   0,1: none 2: keep merged,geom_master,SLC 3: keep MINTPY 4: everything
ssaraopt.platform                   = SENTINEL-1A,SENTINEL-1B
ssaraopt.relativeOrbit              = 41
ssaraopt.startDate                  = 20141001
#ssaraopt.endDate                    = 20160831
ssaraopt.endDate                    = 20171031
ssaraopt.parallel                   = 20
processor                           = isce
demMethod                           = boundingBox
hazard_products_flag                = False
insarmaps_flag                       = False

#topsStack.demDir                   = /nethome/famelung/insarlab/DEMDIR/Sentinel/GalapagosSenDT128/DEM
#topsStack.boundingBox               = '-1 -0.6 -91.7 -90.9' # '-1 0.15 -91.6 -90.9'
topsStack.boundingBox               = '19.13 19.52 -99.46 -98.9' # '-1 0.15 -91.6 -90.9'
topsStack.subswath                  = 1    # '1 2'
topsStack.numConnections            = 3    # comment
topsStack.azimuthLooks              = 34    # comment
topsStack.rangeLooks                = 102   # comment
topsStack.filtStrength              = 0.2  # comment
topsStack.unwMethod                 = snaphu  # comment
topsStack.coregistration            = auto  # [NESD geometry], auto for NESD

#mintpy.reference.lalo               = 20.108384, -98.859349     # N of SN
mintpy.troposphericDelay.method     = height_correlation    #[pyaps / height_correlation / base_trop_cor / no], auto for pyaps
mintpy.save.hdfEos5                 = yes   #[yes / update / no], auto for no, save timeseries to UNAVCO InSAR Archive format
mintpy.save.hdfEos5.update          = yes   #[yes / no], auto for no, put XXXXXXXX as endDate in output filename
mintpy.save.kml                     = yes    #[yes / no], auto for yes, save geocoded velocity to Google Earth KMZ file

######################################################
```

```{tip}
Los datos necesarios para modificar el archivo template, se describen en la sección 3.1.1 de la tesis **CARACTERIZACIÓN DE SUBSIDENCIA Y EXPOSICIÓN DE LA POBLACIÓN VULNERABLE EN LA ZONA METROPOLITANA DE LA LAGUNA** 
```

En el archivo anterior, se modifican datos importantes, como son la fecha de inicio y final de la serie de tiempo, donde buscara las imágenes de ese periodo de tiempo, y la órbita relativa de la imagen.

```bash
ssaraopt.relativeOrbit              = 12
ssaraopt.startDate                  = 20150101
#ssaraopt.endDate                    = 20160831
ssaraopt.endDate                    = 20151231
```

Las coordenadas norte, sur, este y oeste, donde se buscaran las imágenes, recuerda solo poner un decimal, y las coordenadas tienen que estar en latitud y longitud, estos datos los obtuvimos de la sección anterior..

También se pueden seleccionar los subswath que se procesan , para este caso se trabajara la escena completa.

```bash
topsStack.boundingBox               = '24.3 26.5 -104.8 -101.8' # '-1 0.15 -91.6 -90.9'
topsStack.subswath                  = 1 2 3    # '1 2'

```

Se guardan los cambios y se tiene preparado el archivo con la configuración requerida para iniciar el proceso.



## 2. Generación de carpetas

Para iniciar con el proceso, se tiene que ejecutar la siguiente instrucción, se debe de realizar desde la carpeta de temporales, este proceso generara el directorio y descargara el DEM del la zona que ingresamos.


```{important}
```bash
process_rsmas.py ruta_carpeta/nombre_archivo.template --submit
```

Ejemplo real del comando:

```bash
process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --submit 
```

```bash
This is the Open Source version of ISCE.
Some of the workflows depend on a separate licensed package.
To obtain the licensed package, please make a request for ISCE
through the website: https://download.jpl.nasa.gov/ops/request/index.cfm.
Alternatively, if you are a member, or can become a member of WinSAR
you may be able to obtain access to a version of the licensed sofware at
https://winsar.unavco.org/software/isce
Run routine processing with process_steps.py on steps: ['download', 'process']
--------------------------------------------------

*************** Template Options ****************
Custom Template File:  /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template
Project Name:  TorreonSenDT12_2014_2023
Work Dir:  /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023
1 2 3-->'1 2 3'
'24.3 26.5 -104.8 -101.8'-->'24.3 26.5 -104.8 -101.8'
auto-->auto
None-->None
generate template file: /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/TorreonSenDT12_2014_2023.template
1 2 3-->'1 2 3'
'24.3 26.5 -104.8 -101.8'-->'24.3 26.5 -104.8 -101.8'
20230906:200358 * ##### NEW RUN #####
20230906:200358 * process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --submit
process_rsmas.job submitted as LSF job #164951
164951

Total time: 00 mins 0.2 secs
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1045] 
```

Es necesario monitorear la descarga del **DEM**. Para ello, se utiliza el comando `bjobs`, que permite visualizar los procesos en ejecución. Una vez completada la descarga, se debe acceder a la carpeta **SCRATCHDIR**, donde se verificará la creación de una carpeta con el nombre del **template**, la cual debe contener el **DEM**.

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan[1045] bjobs
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
164952  ecabral RUN   q_hpc      mn358       g2_a        *and_dem_0 Sep  6 19:04
164951  ecabral RUN   q_hpc      mn325       g2_a        *2014_2023 Sep  6 19:03
```

Una vez finalizado el proceso, se debe acceder al directorio **scratch** utilizando la variable `$SCRATCHDIR`.

```bash
cd $SCRATCHDIR
```

```bash
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1010] cd $SCRATCHDIR
```

Se despliega el contenido de la carpeta y se verifica la creación de un directorio con el nombre del **template**.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch[1011] ls -l
total 308
drwxrwxr-x  5 ecabral ecabral_g   4096 Jun 23  2022 CDMX_14_22_SenAT78_mintpy
drwxrwxr-x  5 ecabral ecabral_g   4096 Jun 20  2022 CDMX_14_22_SenDT143_mintpy
drwxrwxr-x  4 ecabral ecabral_g   4096 Oct 26  2021 CDMX_mintpy
drwxrwxr-x  6 ecabral ecabral_g   4096 May 19  2021 Ceboruco
drwxrwxr-x  4 ecabral ecabral_g   4096 Sep 17  2021 Enjambres_2021
drwxrwxr-x  5 ecabral ecabral_g   4096 Sep 13  2021 IrapuatoSenDT114
drwxrwxr-x  8 ecabral ecabral_g   4096 Aug 13 10:03 JaliscoSenDT12_2014_2022_v4
drwxrwxr-x  5 ecabral ecabral_g   4096 Aug 23 12:29 MexicoCentroSenDT41_2018_2021
drwxrwxr-x  7 ecabral ecabral_g   4096 Oct 15  2021 mintpy_Uruapan
drwxrwxr-x  6 ecabral ecabral_g   4096 Mar  1  2021 Process_Chichon_asc
drwxrwxr-x 16 ecabral ecabral_g   4096 Nov 13  2021 Process_Chichon_desc
-rw-rw-r--  1 ecabral ecabral_g    460 Sep 18  2019 run_example_2014_2018
-rw-rw-r--  1 ecabral ecabral_g     26 Sep 17  2019 run_example_2018_2019
-rw-rw-r--  1 ecabral ecabral_g  42738 Oct  7  2022 sal_rsync
drwxrwxr-x  4 ecabral ecabral_g   4096 Feb 15  2023 SanRafael7SenAT149_J21_F23
drwxrwxr-x  5 ecabral ecabral_g   4096 Feb 16  2023 SanRafael7SenDT127_J21_F23
drwxrwxr-x  5 ecabral ecabral_g   4096 Sep  6 20:57 TorreonSenDT12_2014_2023
```

Se genera la carpeta correspondiente, que en este caso de ejemplo es **TorreonSenDT12_2014_2023**. A continuación, se accede a dicha carpeta para revisar su contenido.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch[1023] cd TorreonSenDT12_2014_2023/
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023[1024] ls -l
total 36
drwxrwxr-x 2 ecabral ecabral_g 4096 Sep  7 13:24 DEM
-rw-rw-r-- 1 ecabral ecabral_g   99 Sep  7 13:23 example.log
-rw-rw-r-- 1 ecabral ecabral_g   98 Sep  7 13:23 isce.log
-rw-rw-r-- 1 ecabral ecabral_g 1428 Sep  7 13:23 log
-rw-rw-r-- 1 ecabral ecabral_g  478 Sep  7 13:23 process_rsmas.job
drwxrwxr-x 2 ecabral ecabral_g 4096 Sep  7 13:27 run_files
-rw-rw-r-- 1 ecabral ecabral_g   95 Sep  7 13:23 run_files_list
drwxrwxr-x 2 ecabral ecabral_g 4096 Sep  7 13:24 SLC
-rw-rw-r-- 1 ecabral ecabral_g 2102 Sep  7 13:23 TorreonSenDT12_2014_2023.template
```

Se identifican las carpetas generadas en este primer proceso. Es fundamental verificar la creación de la carpeta **DEM**, donde se revisará su contenido. En su interior, se encuentran seis archivos que conforman el **DEM**, junto con otros dos archivos adicionales.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023[1025] ls -l DEM/
total 607536
-rw-rw-r-- 1 ecabral ecabral_g 311040000 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem
-rw-rw-r-- 1 ecabral ecabral_g       530 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem.vrt
-rw-rw-r-- 1 ecabral ecabral_g 311040000 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem.wgs84
-rw-rw-r-- 1 ecabral ecabral_g       536 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem.wgs84.vrt
-rw-rw-r-- 1 ecabral ecabral_g      4224 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem.wgs84.xml
-rw-rw-r-- 1 ecabral ecabral_g      4292 Sep  7 13:24 demLat_N24_N27_Lon_W105_W101.dem.xml
-rw-rw-r-- 1 ecabral ecabral_g         0 Sep  7 13:24 insar.log
-rw-rw-r-- 1 ecabral ecabral_g         0 Sep  7 13:23 isce.log
```



## 3. Descarga de imagenes SAR

Para descargar las imágenes que se usaran en la serie de tiempo, nos movemos en la carpeta SLC, de nuestro directorio generado, observamos el contenido, y revisamos el archivo `ssara_listing.txt` en donde nos mostrara las imagenes encontradas en el area configurada.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch[1034] cd TorreonSenDT12_2014_2023/SLC/
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1035] ls -l
total 740
-rw-rw-r-- 1 ecabral ecabral_g   1333 Sep  7 13:54 example.log
-rw-rw-r-- 1 ecabral ecabral_g    818 Sep  7 13:52 log
-rw-rw-r-- 1 ecabral ecabral_g   2580 Sep  7 13:54 out_download_ssara1.e
-rw-rw-r-- 1 ecabral ecabral_g   3214 Sep  7 13:54 out_download_ssara1.o
-rw-rw-r-- 1 ecabral ecabral_g 144231 Sep  7 13:52 ssara_listing.txt
-rw-rw-r-- 1 ecabral ecabral_g 591389 Sep  7 13:52 ssara_search_20230907195223.kml
```

> [!TIP]
>
> Para observar el contenido de archivo `ssara_listing.txt` usaremos el comando `cat` o `more`.​



```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1036] cat ssara_listing.txt
Running SSARA API Query:  https://web-services.unavco.org/brokered/ssara/api/sar/search?platform=SENTINEL-1A%2CSENTINEL-1B&relativeOrbit=12&start=%3D2014-01-01&end=%3D2023-08-31&processingLevel=L0%2CL1.0%2CSLC&intersectsWith=Polygon%28%28-104.80+24.30%2C+-104.80+26.50%2C+-101.80+26.50%2C+-101.80+24.30%2C+-104.80+24.30%29%29
SSARA API query: 22.375481 seconds
Found 633 scenes
wget -O dem.tif "http://ot-data1.sdsc.edu:9090/otr/getdem?north=28.619530&south=22.061039&east=-101.114549&west=-105.080069&demtype=SRTMGL1_E"
gdal_translate -of GMT -ot Int16 -projwin -105.080069 28.619530 -101.114549 22.061039 /vsicurl/https://cloud.sdsc.edu/v1/AUTH_opentopography/Raster/SRTM_GL1_Ellip/SRTM_GL1_Ellip_srtm.vrt dem.grd
ASF,Sentinel-1A,3409,2014-11-23T12:48:41.000000,2014-11-23T12:49:11.000000,12,3091,3091,IW,None,DESCENDING,R,VV,https://datapool.asf.alaska.edu/SLC/SA/S1A_IW_SLC__1SSV_20141123T124841_20141123T124911_003409_003FA0_DE44.zip
ASF,Sentinel-1A,3759,2014-12-17T12:48:41.000000,2014-12-17T12:49:10.000000,12,3091,3091,IW,None,DESCENDING,R,VV,https://datapool.asf.alaska.edu/SLC/SA/S1A_IW_SLC__1SSV_20141217T124841_20141217T124910_003759_0047B8_D60E.zip

...
```

🐾  Del archivo **ssara_listing.txt**, solo es relevante el encabezado, ya que muestra la cantidad de imágenes encontradas, seguido de un listado con cada una de ellas. En este caso, el encabezado indica la presencia de **633** escenas.

A continuación, se accede al archivo **log** utilizando el comando `cat`, al igual que con el archivo anterior. En este caso, se debe revisar la última línea, que contiene la instrucción para ejecutar la descarga.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1037] cat log
20230907:135127 * ssara_federated_query.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --kml
20230907:135223 * ssara_federated_query.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --print > /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC/ssara_listing.txt
20230907:135245 * ssara_federated_query-cj.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --parallel=20 --print --download
```


> [!IMPORTANT]
>
> Ser cuidadoso con la generación del archivo.



Se crea un archivo denominado **run** utilizando editores como **nano** o **vim**. En su interior, se debe agregar la última línea del archivo **log**, la cual comienza con `ssara_federated_query-cj.py`. Es importante modificar el nombre del archivo eliminando `-cj`, de modo que quede como `ssara_federated_query.py`, asegurando que la instrucción se mantenga en una sola línea continua.

```bash
#Generación del archivo

//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1040] nano run
```

Se abre el editor, se pega la linea.

```bash
ssara_federated_query-cj.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --parallel=20 --print --download
```

Se modifica para que quede sin el `-cj`.

```bash
ssara_federated_query.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --parallel=20 --print --download
```

Se guarda con `ctrl^O`, se escribe el nombre que es `run` y salimos de nano con `ctrl^X`.

```bash
#Se muestra la generacion de archivo y su contenido

//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1041] ls -l
total 741
-rw-rw-r-- 1 ecabral ecabral_g   1333 Sep  7 13:54 example.log
-rw-rw-r-- 1 ecabral ecabral_g    818 Sep  7 13:52 log
-rw-rw-r-- 1 ecabral ecabral_g   2580 Sep  7 13:54 out_download_ssara1.e
-rw-rw-r-- 1 ecabral ecabral_g   3214 Sep  7 13:54 out_download_ssara1.o
-rw-rw-r-- 1 ecabral ecabral_g    211 Sep  7 16:37 run
-rw-rw-r-- 1 ecabral ecabral_g 144231 Sep  7 13:52 ssara_listing.txt
-rw-rw-r-- 1 ecabral ecabral_g 591389 Sep  7 13:52 ssara_search_20230907195223.kml
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1057] cat run
ssara_federated_query.py --platform=SENTINEL-1A,SENTINEL-1B --relativeOrbit=12 --intersectsWith='Polygon((-104.80 24.30, -104.80 26.50, -101.80 26.50, -101.80 24.30, -104.80 24.30))' -s=2014-01-01 -e=2023-08-31 --parallel=20 --print --download
```

 El paso siguiente es correr el archivo **run** para descargar las imágenes encontradas, para esto se usa la instrucción ` split_job.py` seguido del nombre de nuestro archivo.

 ```bash
split_jobs.py run
 ```

 ```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1043] split_jobs.py run
 ```

Se despliega como resultado

```bash
Go to job directory /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC
input run file: /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC/run
number of lines: 1
split -a 1 -l 1 -d /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC/run z_input_run.
number of jobs to submit: 1
job queue: q_hpc
walltime: 3:00
memory: 3700
number of processors: 1
email: None
bsub < z_input_run.0.sh
Job <166706> is submitted to queue <q_hpc>.
--------------------------------------------------
sleeping until 1 jobs are done for run
# of TorreonSenDT12_2014_2023/z_output_run.*.o files: 0 / 1 after 0 mins
# of TorreonSenDT12_2014_2023/z_output_run.*.o files: 0 / 1 after 1 mins
--------------------------------------------------
ALL 1 jobs are done for run
Total used time: 00 hours 01 mins 36 secs
moving z_* files into /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC/run_jobs
finished at 2023-09-07 16:52:00.324981
```

Una vez finalizado el proceso, es necesario verificar que todas las escenas se hayan descargado correctamente. Para ello, se utiliza la siguiente instrucción, que combina dos comandos de **bash**. Primero, `ls -l *.zip` lista todos los archivos con la extensión `.zip`. Luego, el conector `|` envía este resultado al comando `wc -l`, el cual cuenta la cantidad de archivos encontrados. Específicamente, este último comando contabiliza el número de líneas generadas por el listado de archivos `.zip`, proporcionando así un recuento preciso de los archivos descargados.

```bash
ls -l *.zip | wc -l
```



> [!NOTE]
>
> Es importante asegurarse de estar dentro de la carpeta correspondiente al proceso, específicamente en el directorio **SLC**, donde se almacenan las escenas, antes de ejecutar este comando.



```bash
#Comando para estar en el directorio SLC
//mn325/tmpu/ecabral_g/ecabral/scratch[1005] cd TorreonSenDT12_2014_2023/SLC/
#Comando para contar las imagenes
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1006] ls -l *.zip | wc -l
459
```

🔁  

Se observa que no todas las escenas se han descargado completamente, por lo que es necesario ejecutar nuevamente el comando `split_jobs.py run`. Se debe monitorear el proceso hasta alcanzar las **633** escenas.

Posteriormente, se realiza una nueva verificación. Si la cantidad de imágenes coincide con el número esperado, se procede al siguiente paso. En caso de que el total de imágenes descargadas sea correcto, se continúa con la **Parte 4**.

---

### 3.1 Número de imágenes no coincide

> [!WARNING]
>
> Se sugiere el siguiente procedimiento.
>
> Se observo que las imágenes descargadas que son 624 no coinciden con las imágenes encontradas en el `ssara_listing` que son 633, una de las razones es por que no se encuentran las imágenes, al momento de realizar el `split_jobs`, indica que ya termino la descargas, podemos pasar a procesar o realizar las siguientes instrucciones.
>
> 🚨 💡 🚨  
> Debido a que no se encontraron todas la imágenes en el repositorio de **ASF**, el proceso no se completara, se espera a que finalice y se buscara que fechas se tiene que excluir.
>
> Una forma de saber cuales son las imágenes que no se encuentran es comparando la lista del archivo `ssara_listing.txt` con las imágenes descargadas, para eso podemos seguir las siguientes instrucciones [Busqueda de archivos](../01Series_Tiempo/Comparacion/Busqueda.ipynb)
>
> Detectando las imágenes que no se descargaron, se tiene que proceder con copiar la carpeta `SLC` fuera de la carpeta del procesamiento, en este caso es ` TorreonSenDT12_2014_2023`. La forma de realizarlo es ingresando en la carpeta de Torreón y escribir el siguiente comando 
>
> ```bash 
> mv SLC .. 
> ```
>
> ```forma 
> mv ruta_origen/carpeta_o_archivo_mover ruta_destino/nombre_de_archivo_o_carpeta 
> ```
>
> El comando `mv` se utiliza para mover o renombrar, le sigue la ruta/archivo o carpeta a mover y después la ruta donde se moverá, se coloca `..` debido a que se movera una dirección superior a la de Torreon. 
>
> Cuando se termine de mover la información, procedemos con la eliminación de la carpeta Torreon, usaremos el siguiente comando.
>
> ```bash
> rm -r TorreonSenDT12_2014_2023
> ```
>
> Nos movemos a la carpeta de ` TEMPLATES` y editamos nuestro template, con las imágenes que encontramos corriendo el script de búsqueda, armamos una lista con las fechas donde solo se separan con comas, las fechas se colocan en
>
> ``` bash
> fecha1,fecha2,fecha3 
> ```
>
> Se tiene que ubicar dentro de la carpeta `TE/carpeta_proyecto`, en donde se buscara el template a modificar.
>
> ```bash
> cd $TE/dan
> nano TorreonSenDT12_2014_2023.template
> ```
>
> Se muestra el archivo editado
>
> ```bash
> ######################################################
> cleanopt                            = 0          # [ 0 / 1 / 2 / 3 / 4]   0,1: none 2: keep merged,geom_master,SLC 3: keep MINTPY 4: everything
> ssaraopt.platform                   = SENTINEL-1A,SENTINEL-1B
> ssaraopt.relativeOrbit              = 12
> ssaraopt.startDate                  = 20140101
> #ssaraopt.endDate                    = 20160831
> ssaraopt.endDate		    = 20230831
> ssaraopt.parallel                   = 20
> processor                           = isce
> demMethod                           = boundingBox
> hazard_products_flag                = False
> insarmaps_flag                       = False
> 
> #topsStack.demDir                   = /nethome/famelung/insarlab/DEMDIR/Sentinel/GalapagosSenDT128/DEM
> #topsStack.boundingBox               = '-1 -0.6 -91.7 -90.9' # '-1 0.15 -91.6 -90.9'
> topsStack.boundingBox               = '24.3 26.5 -104.8 -101.8' # '-1 0.15 -91.6 -90.9'
> topsStack.subswath                  = 1 2 3    # '1 2'
> topsStack.numConnections            = 3    # comment
> topsStack.azimuthLooks              = 5    # comment
> topsStack.rangeLooks                = 15   # comment
> topsStack.filtStrength              = 0.2  # comment
> topsStack.unwMethod                 = snaphu  # comment
> topsStack.coregistration            = auto  # [NESD geometry], auto for NESD
> topsStack.excludeDates              = 20220602,20150802,20150919,20151130,20160305,20160422,20160516,20160609,20160703 #auto for all dates
> 
> #mintpy.deramp		 	    = linear #[no / linear / quadratic], auto for no - no ramp will be removed
> #mintpy.reference.lalo               = 28.9,-113.5     # N of SN
> mintpy.troposphericDelay.method     = height_correlation    #[pyaps / height_correlation / base_trop_cor / no], auto for pyaps
> mintpy.save.hdfEos5                 = yes   #[yes / update / no], auto for no, save timeseries to UNAVCO InSAR Archive format
> mintpy.save.hdfEos5.update          = yes   #[yes / no], auto for no, put XXXXXXXX as endDate in output filename
> mintpy.save.kml                     = yes    #[yes / no], auto for yes, save geocoded velocity to Google Earth KMZ file
> 
> ######################################################
> 
> ```
>
> Fechas excluidas:
>
> ```bash
> topsStack.excludeDates              = 20220602,20150802,20150919  #auto for all dates
> ```
>
> Se guarda los cambios, procedemos a enviar el proyecto, para que se descargue el DEM y el listado.
>
> ```bash
> process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --submit 
> ```
>
> Se revisa que genere el directorio en `SCRATCHDIR` , que se encuentre la carpeta `DEM`, y el listado en la carpeta `SLC`, una vez generada la información de forma correcta, borramos la carpeta `SLC` y movemos la carpeta `SLC` con las imágenes dentro del directorio, ahora podemos enviar el proyecto. 
>
> ```bash
> cd $SCRATCHDIR/TorreonSenDT12_2014_2023
> ls -l DEM
> cat SLC/ssara_listing.txt
> rm -r SLC/
> cd ..
> mv SLC TorreonSenDT12_2014_2023/
> ls -l TorreonSenDT12_2014_2023
> ```
>
> Se comprueba que la información esté de forma correcta, con estos pasos adicionales descartamos las imágenes no encontradas.

---

## 4. Envío para procesar las imágenes para la serie de tiempo.

Para realizar el procesamiento, se debe acceder a la carpeta de archivos temporales utilizando el comando `cd $TEM` y ejecutar la siguiente instrucción:

```bash
process_rsmas.py ruta_del_archivo temporal --start process --submit --walltime 120:00
```

Es posible agregar la opción `--walltime tiempo_horas`, que define la duración máxima del procesamiento. El límite permitido es de **120 horas** (equivalente a **5 días**).

En este ejemplo, el comando se ejecuta de la siguiente manera:

```bash
process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --start process --submit --walltime 120:00
```

Proceso desde el cambio de directorio hasta el incio del proceso.

```bash
#cambio de directorio
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/SLC[1029] cd $TE
#inicio del procesamiento
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1030] process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --start process --submit --walltime 120:00
#muestra que se ha enviado
This is the Open Source version of ISCE.
Some of the workflows depend on a separate licensed package.
To obtain the licensed package, please make a request for ISCE
through the website: https://download.jpl.nasa.gov/ops/request/index.cfm.
Alternatively, if you are a member, or can become a member of WinSAR
you may be able to obtain access to a version of the licensed sofware at
https://winsar.unavco.org/software/isce
Run routine processing with process_steps.py on steps: ['process']
Remaining steps: []
--------------------------------------------------

*************** Template Options ****************
Custom Template File:  /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template
Project Name:  TorreonSenDT12_2014_2023
Work Dir:  /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023
1 2 3-->'1 2 3'
'24.3 26.5 -104.8 -101.8'-->'24.3 26.5 -104.8 -101.8'
auto-->auto
None-->None
generate template file: /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/TorreonSenDT12_2014_2023.template
1 2 3-->'1 2 3'
'24.3 26.5 -104.8 -101.8'-->'24.3 26.5 -104.8 -101.8'
20230912:121006 * ##### NEW RUN #####
20230912:121006 * process_rsmas.py /tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES/dan/TorreonSenDT12_2014_2023.template --start process --submit --walltime 120:00
process_rsmas.job submitted as LSF job #174111
174111

Total time: 00 mins 0.2 secs
```

Ahora solo queda monitorear la serie con el comando `bjobs`.

```
//mn325/tmpu/ecabral_g/ecabral/insarlab/infiles/ecabral/TEMPLATES[1031] bjobs
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
174111  ecabral RUN   q_hpc      mn325       g3_a        *2014_2023 Sep 12 11:10
```

Si todo sale bien, en nuestro directorio se encontraran los resultados de la serie de tiempo. 

Uno de los errores que puede ocurrir, es que no se termine el proceso con Mintpy debido al tiempo, para poderlo solucionar realizamos las [Tiempo de Mintpy](mintpy.md)





