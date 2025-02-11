# MintPy en Miztli

El **run** 13 es el proceso que realiza `Mintpy`, en esta ocasión, por ser una serie muy larga, nos quedamos sin tiempo en el proceso en la miztli. Como es el último proceso, se puede enviar por separado.

Para iniciar, tenemos que estar dentro de el directorio del proceso, en la carpeta de `scratch`, donde podemos ver todos los resultados del proceso.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023[1020]
```

Para modificar el tiempo del run 13 que pertenece a la parte del proceso con `Mintpy`, nos moveremos a la carpeta `run_files`, en el directorio de trabajo.

```bash
cd run_files
```

Se observan los archivos que se encuentran en la carpeta.

```bash
ls -l
```

Se identifica el archivo ` run_13_smallbaseline_0.job` es importante la terminación `run_13_*.job`, vemos cual es el contenido de dicho archivo.

```bash
cat run_13_smallbaseline_0.job
```

Muestra el siguiente resultado:

```bash
#! /bin/bash
#BSUB -J run_13_smallbaseline_0
#BSUB -P insarlab
#BSUB -n 1
#BSUB -R span[hosts=1]
#BSUB -o /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files/run_13_smallbaseline_0_%J.o
#BSUB -e /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files/run_13_smallbaseline_0_%J.e
#BSUB -q q_hpc
#BSUB -W 12:00
#BSUB -R rusage[mem=3000]
```

En donde podemos ver la bandera -W con 12:00 horas, este tiempo se tendrá que extender, por lo que se modificará con un vim o nano, ingresando 72:00 en vez de las 12 horas que tenía.

El archivo guardado tiene que quedar de la siguiente forma.

```bash
#! /bin/bash
#BSUB -J run_13_smallbaseline_0
#BSUB -P insarlab
#BSUB -n 1
#BSUB -R span[hosts=1]
#BSUB -o /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files/run_13_smallbaseline_0_%J.o
#BSUB -e /tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files/run_13_smallbaseline_0_%J.e
#BSUB -q q_hpc
#BSUB -W 72:00
#BSUB -R rusage[mem=3000]
```

Comprobando que el archivo esta modificado tenemos que enviar el siguiente comando para que inicie a trabajar de nuevo.

```bash
bsub< run_13_smallbaseline_0.job
```

Envía el siguiente resultado:

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files[12] bsub< run_13_smallbaseline_0.job
Job <193335> is submitted to queue <q_hpc>.
```

Se revisan los trabajos enviados a la miztli se obtiene.

```bash
//mn325/tmpu/ecabral_g/ecabral/scratch/TorreonSenDT12_2014_2023/run_files[13] bjobs
JOBID   USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
193335  ecabral RUN   q_hpc      mn325       g2_b        *aseline_0 Sep 18 14:31
```

