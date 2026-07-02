# Guía WSL
>[!IMPORTANT]
> Es necesario ejecutar la guía dentro de PowerShell como administrador (Para evitar problemas con permisos)

```powershell
wsl --list --online   # Distros que se pueden instalar
wsl --list --verbose  # Vemos lo que hay instalado
wsl --install Debian  # Debian es la distribución más ligera
wsl --list --verbose  # Debería aparecer la nueva distro de Linux
wsl -d Debian         # Entramos a Debian y creamos un usuario (y contraseña)
WSL-DEBIAN:\~\> exit  # [Debian] Salimos de Debian (O la distribución que tengas)
```
>[!NOTE]
> Yo uso Debian, pero tú puedes usar la distribución que quieras.

```powershell
# Para mayor simpleza, establecemos lo siguiente como predeterminado
wsl --set-default Debian                                  # La distribución Debian
wsl --manage Debian --set-default-user <mi_nuevo_usuario> # El usuario que recién creamos
```

Y luego para ejecutar Debian...
```powershell
wsl # Así de simple
```

>[!CAUTION]
> *Cuando queramos desinstalar el entorno WSL:*
> ```powershell
> wsl --list --verbose                # Ver qué hay instalado
> wsl --unregister <DistributionName> # DESTRUIR la distribución de WSL
> ```

## Mover la distribución a otro HDD/SDD (Opcional)
El directorio que quiero utilizar es el siguiente: \
`D:\...\MyWorkSpace\ProyectoCrossPlatformX`

Y Windows Subsystem for Linux (WSL) monta las carpetas de Windows así: \
`/mnt/c/...` \
`/mnt/d/...` \
`/mnt/e/...` \
(Según la letra de la unidad de Disco)

Windows Subsystem for Linux almacena todos los datos relacionados a una distribución en un archivo `.vhdx` (Imagen de Disco Duro) ubicado en `%LOCALAPPDATA%\wsl\{Globally-Unique-Identifier}` dentro de Windows. Similar a una Máquina Virtual. Pero ese directorio está en `C:\` y mi espacio de trabajo está en  `D:\`.

Entonces hay que **mover** la distribución Debian (WSL) a una ubicación localizada dentro del mismo disco que el espacio de trabajo (Esto es ***opcional***, pero yo lo hago para una mejor distribuir las operaciones de lectura de disco y temas de organización).

Para *mover* la distribución WSL de Debian hay que:
1. exportar
2. desregistrar
3. importar en nueva ruta

Entonces en PowerShell con privilegios de administrador:
```powershell
wsl --list --verbose                                                        # Buscamos la distribución que queremos (Debian)
mkdir "D:\WSL-distros\Debian"                                               # Creamos carpeta de destino
wsl --export Debian "D:\WSL-distros\Debian\compilerWorkshop_bison+flex.tar" # Exportamos
wsl --unregister Debian                                                     # Damos de baja Debian de WSL (wsl --list --verbose ya no debe mostrarlo)

# Importamos "D:\Ubicacion\Nueva" "D:\Ubicacion\Nueva\backup.tar"
wsl --import Debian "D:\Distros\NombreDistro" "D:\Distros\NombreDistro\proyectWorkshop_backup.tar"
wsl --status             # Vemos qué está establecido por defecto
wsl --list --verbose     # Revisamos que se importó correctamente
wsl --set-default Debian # Si no está establecida, establecemos como distribución por defecto
wsl                      # Entramos a Debian (o a nuestra distribución de Linux elegida)

whoami # [Debian] Hacemos whoami
exit   # [Debian] Si es root, salimos

# Si es `root` el usuario al que entramos en Debian, reestablecemos el usuario por defecto
wsl --manage Debian --set-default-user <mi_nuevo_usuario>
# Por último, deberíamos de poder entrar a Debian con el usuario solo con escribir lo siguiente en la consola (de Windows) con privilegios de administrador
wsl
```

>[!IMPORTANT]
> Al final de todo lo anterior, ya no debería aparecer el archivo `.vhdx` (Imagen de Disco Duro de la distribución WSL) en el directorio de Windows: `%LOCALAPPDATA%\wsl`


_Si la guía parece ambigua y larga es porque es introductoria, son los pasos que **yo** seguí para instalar `WSL-Debian`_
