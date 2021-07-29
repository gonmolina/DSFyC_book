# Guía de instalación de las herramientas computacionales

## Verificación del tipo de sistema operativo (32 o 64 bits)

### En Windows 10 y Windows 8.1

Selecciona el botón Inicio y después, ``Configuración > Sistema > Acerca de``. A la derecha, en Especificaciones del dispositivo, consulta Tipo de sistema.

### En Windows 7

Selecciona el botón Inicio, haz clic con el botón derecho en  
``Equipo`` y selecciona `Propiedades`. En `Sistema`, consulta el `tipo de sistema`.

## Descargar Miniconda de acuerdo al tipo de sistema operativo

Seguir el enlace de [Miniconda](https://docs.conda.io/en/latest/miniconda.html) y elegir la versión de `Python 3.X` (donde X es un número entero) que este acorde al tipo de sistema operativo instalado es su máquina según lo averiguado en el punto anterior.

## Instalar Miniconda

Seguir las recomendaciones por defecto dadas por el instalador. Una vez terminada la instalación tendremos Python instalado en nuestro sistema. Además tendremos en el menú de inicio un ítem
que se llama Anaconda Powershell Prompt, el cual abriremos para continuar con la instalación de las herramientas del curso.

## Instalación de las herramientas especificas del curso

Para continuar con la instalación necesitaremos descargar el archivo [dyc.yml](https://drive.google.com/file/d/1agx9I7KoTB2Fw9MO6_tuzQqRSai2iBgV/view?usp=sharing) haciendo clock sobre el enlace anterior. Una vez descargado, dirigirse con la consola de Anaconda Powershell Prompt abierta a la carpeta de descargas donde se encuentra el archivo `dyc.yml` descargado anteriormente. En general, para dirigirse a esta carpeta debemos tipear

```bash
cd ~\Downloads\
```

Para verificar que el archivo se encuentre ahí, podemos tipear:

```bash
dir dyc.yml
```

Si nos encontramos en la carpeta donde está el archivo este deberá aparecer luego del comando anterior.

Finalmente para instalar todas las herramientas necesarias en el curso debemos ejecutar en este comando:

```bash
conda env create --file dyc.yml
```

Una vez terminado este paso tendremos instaladas todas las herramientas necesarias para la materia. Este paso puede tardar un tiempo largo dependiendo de la velocidad de conexión. Si la instalación fué exitosa encontraremos dos ítems en el menú de inicio que son los que utilizaremos:

1. Spyder (dyc)
1. Jupyter Notebooks (dyc)

## Retoques a la herramientas para mayor comodidad de uso

### Jupyter Notebooks en Spyder

Spyder nos proporciona un entorno de desarrollo muy parecido a  Matlab, donde tenemos paneles similares a los de Matlab:

1. Panel de Comandos (command window)
1. Explorador de variables, donde podemos ver las variables  definidas y sus valores
1. Un editor de scripts para programar
1. Un editor de cuadernos de Jupyter (a continuación veremos como activarlo)
1. Una ventana de ayuda, que nos brinda rápidamente ayuda del comando que necesitemos usando las teclas `Ctrl+I`.
1. Historial de comandos, etc.

Una configuración útil en `Spyder` es tener la herramienta de los `Jupyter Notebook` en el mismo `Spyder`. Para esto abrir Spyder en `View> Panes` activar Notebooks.

La mayor parte del material de este curso está escrito en formato de de Jupyter Notebooks, y puede ser descargado abierto y modificado mediante usando Spyder y el plugin de Jupyter Notebooks.

### Enlace directo a JupyterLab

Otro entorno que nos brinda casi las mismas posibilidades que Spyder es JupyterLab. La ventaja es que los cuadernos de Jupyter son mucho mejor soportados en este entorno y además es más liviano que Spyder.

Por defecto Miniconda/Anaconda/Conda no instala un acceso directo desde el Menú de Inicio en Windonws. Sin embargo este puede llegar a ser muy útil. Si desea tenerlo puede seguir los siguientes pasos:

1. Ir al directorio `%AppData%\Microsoft\Windows\Start Menu\Programs`
1. Ir al directorio `Anaconda3 (64-bits)`
1. Copiar y pegar en el mismo lugar el acceso de `Jupyter Notebook (dyc)`
1. Renombrar con `Jupyter Lab (dyc)`
1. Con botón derecho sobre el icono que acabamos de renombrar abrir Propiedades
1. En destino el campo Destino, buscar donde dice `... scripts\jupyter-notebook-script.py....` y cambiar por `...\scripts\jupyter-lab-script.py...`.
1. En el campo Comentario cambiar Jupyter Notebook (dyc) por Jupyter Lab (dyc)

Con esto ya tendremos instaladas las herramientas de Python  necesarias para resolver los problemas de curso y el acceso directo a Jupyter Lab.
