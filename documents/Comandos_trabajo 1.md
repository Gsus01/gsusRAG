# COMANDOS TRABAJO

## Atajos 

`cds`: current build slot

`cdi`: indra

`search_ada`: para buscar en los ficheros de ADA (ads y adb)

`search_idl`: para buscar dentro de los ficheros IDL, como los *.idl3 o *.idl

`killall.sh` script que mata toda la cmd

`startup.sh` script que levanta

`busca_label.sh` buscar una ITC

`cleartool describe -long <fichero>` Obtener la informacion asociada a un elemento de clearcase

## Localizacion de ficheros

**q_tk_cmd**: ficheros de creación y manejo de elementos de la vista están en -- `wp/cmd/toolkit` --

para ver los **save_data**: `nor@ph2ins23:/daticas/R4.4.1-02_FAT/TECHNICAL/`

para ver mi **fichero de usuario**: `nor@ph2ins23:/usuarios/jpabreu`

## Usuarios y credenciales

Credenciales para los servidores: usuario=nor, password=goteras

Dentro de la cmd:
- **R4.4.1** `CMDUSER5:INDRAC`
- **R5.1** `CMDUSER4:SUPERV04`
- **OTROS** `ALL:ALL`

root:Fandango

## Procedimientos tipicos

##### Compilar una vista y moverla a STRING:

 - `make_itec mod_cmd`: Para compilar desde una vista y generar una libreria lib_mod_cmd.so (Te aparece el directorio donde se guarda al compilar)
 - `scp [Ruta Compilado] nor@ph2ins23:/usuarios/jpabreu`
 - `scp nor@ph2ins23:/usuarios/jpabreu/[nombre_libreria] .`

 Desde el current build slot:
 - `scp nor@ph2ins63:/usuarios/jpabreu/lib_mod_cmd.so .`
 - [Reiniciar el String](#reiniciar-el-string) si es necesario

##### Reiniciar el String
- `killall.sh`
- `starts`
    - Le das a todo a intro, menos cuando te pida los slots (2 en todo)
    - Con `tso` podemos monitorizar como va el arranque. Cuando de un error/warnig de ada ya ha terminado y le podemos dar a crtl-c para volver a la shell
 

##### Acceder a la CMD:

Esto se va a hacer desde el servidor 172.30.110.63 con usuario nor

 - `ssh -XC s67cmda` X para que pueda abrir ventanas, C para que vaya compilado y vaya mas rapido. 
 - `vncviewer localhost` \\\ `vncviewer s67cmda` acceder a la interfaz grafica


##### REBOOT de la maquina (s67cmda)

Tarda unos minutos

- `reboot`

Dara un error si intentamos hacer `vncviewer`, hay un script, se ejecuta tal que:

- `/home/nor/x11vnc.sh &` 

## Comandos utiles

`tar -xzvf nombre_savedata` Descomprimir savedata

`scp [Ruta Origen] [Ruta Destino]` (ejemplo de ruta remota -> nor@ph2ins23:/usuarios/jpabreu)

`ssh -XC s67cmda` Conectarse al string s67cmda (donde se esta ejecutando la cmd)
 

## Conceptos generales

buil_slot/adap_slot/dmn_slot (normalmente 2/2/2)

## ClearCase

`rules`: **ver** las **reglas** de una vista

`ct edcs`: **cambiar** las **reglas** de una vista. Cambiamos esas 3 lineas, poniendo el numero de ITC correspondiente:

![](images/2023-12-11-11-58-00.png)

`ct lsco -r -cvi`: Ver que ficheros estan en **checkout**

`ct mkbrtype NOMBRE_BRANCH`: Para **crear una branch** en un vob, el nombre de la branch suele ser la ITC.

Ahora hay que abrir el arbolcc del archivo al que hacer checkout, y darle a checkout en la interfaz. Normalmente unreserved para que en otras vistas se pueda modificar, y las opciones por defecto.

`ctdiff nombre_ficero` abre directamente el VTree con las diferencias de la version hecha checkout con la anterior.

`arbolcc [nombre_archivo]`

![](images/2023-12-11-12-02-10.png)

`ct mklbtype -nc -pbr ITC_XXXXX`: Crear **label**

Abrir arbolcc y hacer **attach** a la label.

`arbolcc [nombre_archivo]`

![](images/2023-12-11-12-04-38.png)

(Otra alternativa) `ct mklabel -nc ITC_XXXXX fichero`: **Attach** via comandos

**Resumen** para hacer una **PTR**: 

Cambiar reglas de la vista con el nombre de la ptr, crear la nueva branch con el nombre de la ptr, crear el label de la ptr, hacer checkout al fichero que vayas a modificar, Attach del label con nombre de la ptr, hacer los cambios y comprobar que todo esté correcto, hacer checkin a la bola de la ptr, mergearlo a la rama de dev **nunca jamás a rel (release)!!!**, attach al label a la nueva bola y hacer checkin a esta.

Rellenar Change al acabar la ptr, con Software Info, el texto de lo que has hecho en iTEC Supplier Solution, CMMI

## Sin revisar

java -jar TSS_UI.jar -> cds // Estadisticas de cualquier componente de la máquina.


make_itec: compilar 
make_itec nombre_componente(para compilar solo componentes) -> genera el lib_mod_cmd.so
make_imas -> compilar iMas

cds + vim p_check_external_ntp.sh -> script de ntp

vncviewer s67itgc & -> iSim + segundo plano

regenerar una vista completa(tarda muchísimo) -> /ip23_cmd_dev_merge/fdp/macros/regenerate_view.sh

Si no hay iSim, cambio fichero y ../dmn/m_sup_dmn_subsystem_starter

Para parar/encender el demonio/icas, arrancar, parar o instalar el string ->
/icas/IS/bin/ISGui -d /tmp/String67_sala64 -r /icas/kickstart/itec_releases/itec_releases_phase2_string_67 -s /icas/kickstart/strings_def/string_def_ph2ins63_iCAS_PhaseII_String_67_R5 -e

Para parar y encender el icas de mi máquina (El demonio) ->
systemctl status icas.service
systemctl stop|start icas.service

tail - f | cat -> ver la salida de los ficheros.

tso -> ver el estado actual y como arranca la cmd




vim nombre_fichero -> ver o editar un fichero(para editarlo debe estar checkout)

scp -> copia segura de una máquina a otra
rm -rf -> borrar directorio con elementos dentro y de forma recursiva
cp -> copia normal(archivo a un ficheor por ejemplo)

zcat se puede usar para ver el contenido en bruto de los collects
Lo hemos usado en ocasiones para buscar un PID y ver que proceso correspondia a dicho PID. La forma de verlo es ->
zcat *.prc.gz | cut -d ' ' -f 1,2,3,4,5- | egrep -i "PID"
La cabecera del fichero es ->
Date Time PID User PR PPID THRD S VmSize VmLck VmRSS VmData VmStk VmExe VmLib CPU SysT UsrT PCT AccumT RKB WKB RKBC WKBC RSYS WSYS CNCL MajF MinF Command
zcat s15fdpa1-20230426.prc.gz | cut -d ' ' -f 1,2,3,4,5- | egrep -i "8993"

iSim conectado? -> indra_ph2[4/-/- root@s67cmda3:/icas/dmn] netstat -putan | grep sim
tcp        0      0 0.0.0.0:65001           0.0.0.0:*               LISTEN      110172/m_cms_sim_ga 



generate?core?txt.sh en un 

$ADA_BASE es donde esta el directorio del ada base



para levantar la cmd rapido, cat arguments_file.txt y ver que el kickstart esta false,. Luego 

cd kickstart

qickselect




ip2_cms_dev_1 -> vista de 4.4.1 que utilizar para cotillear


Para hacer las graficas

(actualizdo es asi:)
`/usuarios/integration/icas_collttis/collttis_V0.15.0_df.ph_V0.0.3_new/project_release/ICAS_5_1/collttis --prcall --all -f itec_hosts.txt -d . --from 20240215:10:20:00-20240215:10:30:00`

En un save data, en la carpeta collects. Se pueden leer los gz con zcat
Normalmente en el de proesos *prc.gz
zcat *.prc.gz | cut -d ' ' -f 1,2,3,4,5- | egrep -i "PID"

Para ver el IOWait, es la columna wait. 
Nos metemos en la cmda, df -h, se ve que la particion de icas es la sda3.
ls /var/log/collect en el so, hay muchos. En el save data solo se llevan los de el dia. Si se necesita info de otro dia hay que pedirlo.
Ver particion de icas en extra/current hardware information. (para verlo desde el save data)
en este fichero se ve tambien por ejemplo opciones con las que se ha arrancado la maquina. El uptime tambien se ve aqui. El NTP tambien se ve aqui

Que se necesita para las graficas. Saber dia y horas que queremos sacar. 

parametros: --prcall importante para generar de todos los procesos no solo lo de icas

/usuarios/kymaguchi/ken/collttis/collttis --icas-4-4-1 --prcall --all -f itec_hosts.txt -d . --from 20231219:00:00:00-20231219:10:30:00 

itec_hosts.txt en la carpeta extra. nos lo copiamos y borramos todas menos la que nos interesa. 

En extra/cmd_files/

ORB son los procesos iCAS

Los Q_DDD estan en el FDP


echo "export MW_ORB_COMM_STACK_SIZE=150000000" > CMD
echo "export LD_PRELOAD=/icas/current_build_slot/auto-mnemonics_on.so" >> CMD
#Disable drag and drop until we figure it out how to solve CMD D&D crashes
echo "gsettings set org.gnome.settings-daemon.peripherals.mouse drag-threshold 2000" >> CMD
echo "exec -a CMD spd_app_proc \$*" >> CMD
chmod a+x CMD

Compilar un fichero de gtkada en el sevidor de desarrollo
gmk simple.adb (necesita un linker como simple.linker.conf y los .lib que te pida) 


## Si te cargas el dmn 

indra_ph2[1/1/1 nor@s67cmda3:/icas] rm dmn
indra_ph2[-/1/1 nor@s67cmda3:/icas] ln -s /icas/dmn_slot_5 dmn
indra_ph2[5/1/1 nor@s67cmda3:/icas/dmn] systemctl stop icas.service
indra_ph2[5/1/1 nor@s67cmda3:/icas/dmn] systemctl start icas.service

indra_ph2[5/-/- nor@s67cmda3:/icas/dmn] pss dmn
nor       21535      1  0 12:41 ?        00:00:00 /bin/sh ./init.dmnsupervisor start
nor       21537      1  0 12:41 ?        00:00:00 ./m_sup_dmn -exercise=1
indra_ph2[5/-/- nor@s67cmda3:/icas/dmn] starts

ls -lrt *gtk* ver las librerias de gtk

Tutorial para entender clearcase: https://www.youtube.com/playlist?list=PLfGYZdZpR9Jmcaavga2KdLqn9iewXR7ji

En algunos sitios en vez de `vncviewer` hay que utilizar `vnc.sh` para conectarse a las maquinas

Hacer restart (reiniciar) a una maquina: `reboot` desde esa maquina

### Instalacion de una adap

ejemplo de lo que he tenido que hacer
en el string, en icas/install/A he buscado una adap que cumpliera las condiciones, en este caso me interesaba la parte de los subsystem groups. 

Paloma ha elegido `adap_gen_R4.4.1-04_R441_03_03_MUN_T1_V1_subsystem_groups.cmd.txt_s46941`

Ahora lo que hacemos es ir a `/icas/kickstart/quickselects]` e intentar buscar `grep "*R4.4.1-04_R441_03_03_MUN_T1_V1*" *` que no ha funcionado esta vez

luego hizo `vim quickselects_R4.4.1-04`

Con `vim itec_releases/itec_releases_phase2_string_67` se ven las versiones en las que esta el string. Aqui, en el final pegamos la definicion de la version que queramos instalar, definida en `vim quickselects_R4.4.1-04`.

vim string_def_ph2ins63_iCAS_PhaseII_String_67_R4

```sh
(IS R4.3.2-01) [S6x Sala 64 indra_ph2 nor@ph2ins63:/icas/kickstart/strings_def] grep iCAS_PhaseII_String_67_R4_TRACKERS */*
```

```sh
(IS R4.3.2-01) [S6x Sala 64 indra_ph2 nor@ph2ins63:/icas/kickstart/strings_def] vim -d different_configurations_strings/string_def_ph2ins63_iCAS_PhaseII_String_67_R4_TRACKERS_NO_TCF string_def_ph2ins63_iCAS_PhaseII_String_67_R4
```

Ahora para instalar lo que hacemos es ir al bin del IS `cd /icas/IS/bin/`

```sh
(IS R4.3.2-01) [S6x Sala 64 indra_ph2 nor@ph2ins63:/icas/IS/bin] ./ISGui -h
Install Server GUI for iCAS
Version R4.4.1-01, Copyright (C) 2013-2023, Deutsche Flugsicherung GmbH.

Usage: ISGui [options]

  basic options, with defaults in [ ]:
    -d Distribution Service Socket [/tmp/SOISD]
    -r Release Definition File [/icas/kickstart/itec_releases]
    -s String Definition File [/icas/kickstart/itec_strings_def]
    -L Logfile directory [/icas/IS/log]
    -e extended mode; enables modifications of string and release definition
    -h print this help text
    -v print version information and exit.
    -X Start the application as any user.
```

`pss ISD` para ver el de nuestro string

```
./ISGui -d /tmp/String67_sala64 -r /icas/kickstart/itec_releases/itec_releases_phase2_string_67 -s /icas/kickstart/strings_def/different_configurations_strings/string_def_ph2ins63_iCAS_PhaseII_String_67_R4_TRACKERS_NO_TCF -e
```

en -d lo que hayamos visto en el pss (/tmp/String67_sala64) y luego lo otro que nos interes

Una vez dentro se ve esto

![](images/![Alt%20text](image.png).png)

podemos ver en monitor el estado de las maquinas

A la derecha seleccionamos la release que nos interesa:

![](images/2024-02-06-16-52-03.png)

Luego puedes seleccionar diferentes formas de instalar. Seleccionas en software directory y en adaption los slots que quieras (deberian ser el mismo en ambos). Background install lo hace sin molestar a los que este en otros slots. Destructive los tira pero es mas rapido. Si haces el destructive deberias seleccionar en advanced settings el kickstart override (solo se selecciona cuando se arranca de 0). Luego le das a start y ya va saliendo un log de como va

--------------------

para hacer overload a una maquina utilizar el comando `stress`. Es posible que haya que descargarse el paquete y pasarlo mediante scp a la maquina.

### Coding ispection de un fichero

escribes m_ada_review_file.sh y tabular por si estuviera en otro sitio en otro server
`/COTS/AutoAndTools/ada_review_code-V3.65-gnat-7.0.2_just/ada_review_code/bin/m_ada_review_file.sh nombre_fichero.ada`

tambien existe una pagina para hacer las justificaciones, de donde podemos sacar que incidencia solucionar con cada excepcion:

172.30.223.165/REPOSITORY/CODING_STDS/iTEC_R4.4.1/R4.4.1-TIME_11_01_2024/full/ADA/csi_statistics.xml

172.30.223.165/general/php/access_login.php

### Hacer una build

- Asegurarse que esta todo compilado. Para ello: crs -n
- Si los Flows están sin compilar no pasa nada, el drf también suele dar problemas y en la mayoría de ocasiones podemos ignorarlo también.
- Para hacer la build: `make_builds.sh itec $nombre_De_la_build all yes` (en nuestro caso CR_1382_CMD_TESTING)`/ip2_4_4_1_CMS/fdp/macros/macros_clear/make_builds.sh itec CR_1382_CMD_TESTING "FDP CMD CMS FDO DMN XCN IMAS CORRP XML_SCEN" yes`
- La palabra itec siempre hay que ponerla por el proyecto. El all es para generar todos los bl. Y el yes es para que se copie la build en el servidor, que es necesario para luego poder instalarla.

- Luego comando: bfdp
- En el /p1itec/builds/nombre_de_la_vista ejecutar: ll */tmp/*\.bl*
- Los ficheros que salgan son los que hay que guardarse para construir la build y poder instalar.


(IS R4.3.2-01) [S6x Sala 64 indra_ph2 nor@ph2ins63:/icas] find -type f -name "*CR_1382_CMD_TESTING*\.bl*"
./install/X/xml_scen_CR_1382_CMD_TESTING.bl_s10850_v1
./install/X/xcn_CR_1382_CMD_TESTING.bl_s49481_v1
./install/I/imas_CR_1382_CMD_TESTING.bl_s27915_v1
./install/C/cms_CR_1382_CMD_TESTING.bl_s39150_v1
./install/C/corrp_CR_1382_CMD_TESTING.bl_s15185_v1
./install/C/cmd_CR_1382_CMD_TESTING.bl_s45159_v1
./install/D/dmn_CR_1382_CMD_TESTING.bl_s21229_v1
./install/F/fdo_CR_1382_CMD_TESTING.bl_s03533_v1
./install/F/fdp_CR_1382_CMD_TESTING.bl_s42382_v1

Ahora los sustituimos en la build que queremos modificar en 
`(IS R4.3.2-01) [S6x Sala 64 indra_ph2 nor@ph2ins63:/icas/kickstart] vim itec_releases/itec_releases_phase2_string_67
`

```sh
BUILD_NAME  : CR_1382_CMD_TESTING
DESCRIPTION : CR_1382_CMD_TESTING_testing_strings
BUILDS      : \
              imas_CR_1382_CMD_TESTING.bl_s27915_v1 \
              cmd_CR_1382_CMD_TESTING.bl_s45159_v1 \
              corrp_CR_1382_CMD_TESTING.bl_s15185_v1 \
              cms_CR_1382_CMD_TESTING.bl_s39150_v1 \
              dmn_CR_1382_CMD_TESTING.bl_s21229_v1 \
              fdp_CR_1382_CMD_TESTING.bl_s42382_v1 \
              fdo_CR_1382_CMD_TESTING.bl_s03533_v1 \
              xcn_CR_1382_CMD_TESTING.bl_s49481_v1 \
              drf_R4.4.1-04.bl_s38914_v1 \
              icwp_CWP_02.A07_20240117_04_FAT.bl_s56227_v1 \
              icwp_icas_components_R4.4.1-04.bl_s06356_v1 \
              icwp_CWP_02.A07_20240117_04_FAT_tools.bl_s09116_v1 \
              corrp_tools_R4.4.1-04.bl_s13196_v1 \
              corrp_ttm_tool_tcl_8_6_patch.bl_s61787_v1 \
              cms_tools_R4.4.1-04.bl_s21564_v1 \
              xml_scen_CR_1382_CMD_TESTING.bl_s10850_v1 \
              fdp_tools_R4.4.1-04.bl_s54197_v3 \
              drf_saphira_R4.4.1-04.bl_s55053_v1 \
              fdp_saphira_R4.4.1-04.bl_s37337_v1 \
              corrp_saphira_R4.4.1-04.bl_s15124_v1 \
              conf_patch_ITC161223_testing_enviroments_R441.bl_s04157_v1 \
              fpl_checker_R4.4.1-04.bl_s15995_v5 \
              atg_iSIM_v04.12_R4.4.1_20231222_r145743_147593.bl_s51615_v1 \
              epp_iSIM_v04.12_R4.4.1_20231222_r145743_147593.bl_s38603_v1 \
              pp_iSIM_v04.12_R4.4.1_20231222_r145743_147593.bl_s20260_v1 \
              smp_iSIM_v04.12_R4.4.1_20231222_r145743_147593.bl_s05646_v1 \
              isar_iSAR_V1.3.08_RH7.9.bl_s32704_v1 \
              fls_R4.4.1-04.bl_s63505_v3 \
              adap_gen_R4.4.1-04_R441_03_03_KRH_V1.bl_s25251_v3 \
              adap_icwp_CCT_KRH_R441_CWP_02.A07_20240117_04_FAT_R4.4.1-04.bl_s45127_v1 \
              conf_patch_krh_atsm_R4.bl_s11743_v11 \
              sw_configuration_R4.2_GTK3_14_15.gnat_7.1.2_CMD_FDO_for_ESS_accesibility_at_RH7.4.bl_s47572_v1 \
              atg_ESSI_FUNC_R4.4.1-04.bl_s32622_v2 \
              cmd_ESSI_FUNC_R4.4.1-04.bl_s54782_v2 \
              cms_ESSI_FUNC_R4.4.1-04.bl_s31358_v2 \
              corrp_ESSI_FUNC_R4.4.1-04.bl_s22778_v2 \
              ess_manager_ESSI_FUNC_R4.4.1-04.bl_s27297_v2 \
              fdo_ESSI_FUNC_R4.4.1-04.bl_s20680_v2 \
              fdp_ESSI_FUNC_R4.4.1-04.bl_s58702_v2 \
              icwp_ESSI_FUNC_R4.4.1-04.bl_s18188_v2 \
              isar_ESSI_FUNC_R4.4.1-04.bl_s48473_v2 \
              rfw_ESSI_FUNC_R4.4.1-04.bl_s45969_v2 \
              icwp_CWP_02.A07_20240117_04_FAT_dp_rules_set_DelivDfsDpRules.bl_s04930_v1 \
              icwp_CWP_02.A07_20240117_04_FAT_ep_rules_set_DelivDfsEpRules.bl_s62562_v1 \
              conf_patch_load_finished_install.bl_s61365_v1 \
              conf_patch_isim_mute_sessions.bl_s36994_v2 \
              conf_patch_CWP_V2A06_capture_and_replay.bl_s29575_v1
END
```

ahi estan modificados los nuestros

Luego ya instalas normal

### Hacer overload de una maquina

`ll CMS_PLS_TECH.cfg` desde el cds de la maquina, es un enlace simbolico al correspondiente. Ahi lo importante es que hay una lista con los vip, que tiene thresholds diferentes. Yo bajo el tiempo y quito el gzip de los vip, y asi se hace un gzip de / en /tmp/borrar y ya esta.

### Añadir/quitar maquinas

Mirar en el Current Build Slot el fichero `itec_hosts.txt`.

## Cambiar id de una maquina

Cambiar el id de una maquina sirve para, por ejemplo, que se pinte en otro lado. Esto puede ser util para probar diferentes configuraciones o si no se tienen suficientes maquinas para paliar esto.

Primero se cambia el `itec_hosts.txt` del cds de las maquinas que esten conectadas, cambiendo el ultimo numero, el cual es el ID.
Luego tambien en `SUP_SUBSYSTEM_ID` del cds y del dmm (slot del demonio)

Tambien hay que hacer un stop icas y relanzarlo para que se actualicen los cambios.

# Encontrar los qddd

Los qddd se encuentran en el fdp en el `ddd`

## Traza

Puede haber trazas de diferente tipo, de normal usamos warning

```ada
Q_LIB_LOG.P_TRACE_ERROR
  (V_CATEGORY  => Q_LIB_LOG.E_WARNING,
    V_UNIT      => "P_OPEN_WINDOW",
    V_TEXT_DATA => "Contenido de la traza");
```

## VER TABLA TLS

./tls_tool.sh

## Para conectarse a ip2server86 desde foticlient

desde ip2server3:
`ssh -XC jpabreu@ip2server86`

luego `su nor` como simepre para acceder como nor y ver las vistas que nos interesan 

dfs: 4.4.1 
lvnl:5.x

## Version matrix

Nos han instalado una version que no han incluido en el itec_releases del s67. Por lo tanto el version matrix se ha generado mal. Esto lo que porvoca es que no se pueda reiniciar desde la CMD porque el sistema entiende que las versiones no son compatibles, porque tiene una version que no encuentra en el version matrix. El version matrix esta en cualquier maquina en el directorio: `vim /icas/CMS_VERSIONS_MATRIX.xml`. Para arreglarlo, lo que tenemos que hacer es ir a nuestro itec_releases y agregar la version que han instalado. Despues hay que instalar una version para que se regenere el version matrix, puede ser en cold o en warm.

Si no, podemos instalar desde el cmmc (hay que hacerle chmod +x), es la opcion 13 para hacer el restart:

67623  [05/04/2024 10:42:29] ll cmmc*
67624  [05/04/2024 10:42:40] vim cmmc_test.sh
67625  [05/04/2024 10:43:37] ls *.mib
67626  [05/04/2024 10:43:43] vim CMS_ICAS_MIB.mib
67627  [05/04/2024 10:49:08] ll *.oid
67628  [05/04/2024 10:49:16] vim CMS_ICAS_MIB.oid
67629  [05/04/2024 10:50:44] chmod +x cmmc_test.sh
67630  [05/04/2024 10:50:48] ./cmmc_test.sh
67631  [05/04/2024 11:00:44] ./cmmc_test.sh
67632  [05/04/2024 11:04:21] bash -x cmmc_test.sh
67633  [05/04/2024 11:05:49] h


## Fichero profile cmd

En el cds `profile_cmd.xml` tiene la configuracion de, por ejemplo la disposicion de las CWPs o de los fdos en la cmd.

## Servicios y eventos de un modulo 

Para saber que servicios y eventos tiene un modulo, en el current_build_slot ejecutar:
 
java -jar CLR.jar lib_mod_cms_rsm.so
 
Con cualquier componente que lleve iMAS.

## Compilar y que salgan los warnings

`make_itec -f mod_cmd`
`gnatmake -sf <nombre_fichero>`

## Para trazas, convertir enum en un string

function F_GET_ENUM_AS_STRING ( V_ENUM : T_ENUM_TYPE ) return STRING; 

q_cmd_gen_conv_string.ads
