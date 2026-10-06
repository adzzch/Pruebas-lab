# Instalación y puesta en marcha del entorno `.silver`

Curso Analítica de Datos (709749) · Universidad Cooperativa de Colombia · Semestre 2026-2.

Esta guía se hace **una sola vez**. Toma entre 30 y 60 minutos, y buena parte de ese tiempo es esperar
descargas. Está escrita para **Windows y PowerShell**.

**No hace falta saber programar ni haber abierto una terminal nunca.** Cada paso dice qué escribir,
qué debería pasar y cómo saber que salió bien.

Si algo falla, primero mire la [sección 10, Problemas frecuentes](#10-problemas-frecuentes). Si aun así
no sale, **lleve el error a clase**: no se pierde nada por llegar con el entorno a medias.

> **Dato clave de esta guía:** el entorno virtual se llama **`.silver`**, con un **punto al inicio**.
> El punto es parte del nombre. Todos los comandos que lo mencionan deben llevarlo, y casi todos los
> errores de "no encuentra la carpeta" vienen de olvidarlo.

---

## Índice

1. [Qué vamos a instalar y por qué](#1-qué-vamos-a-instalar-y-por-qué)
2. [Abrir una terminal](#2-abrir-una-terminal)
3. [Instalar Python](#3-instalar-python)
4. [Instalar Git](#4-instalar-git)
5. [Clonar el repositorio del curso](#5-clonar-el-repositorio-del-curso)
6. [Crear y activar el entorno virtual `.silver`](#6-crear-y-activar-el-entorno-virtual-silver)
7. [Instalar las librerías](#7-instalar-las-librerías)
8. [VS Code y los notebooks](#8-vs-code-y-los-notebooks)
9. [Verificación final](#9-verificación-final)
10. [Problemas frecuentes](#10-problemas-frecuentes)
11. [La rutina de cada semana](#11-la-rutina-de-cada-semana)

---

## 1. Qué vamos a instalar y por qué

Son cinco piezas y cada una hace una cosa distinta.

| Pieza | Qué es | Por qué la necesitamos |
|-------|--------|------------------------|
| **Python** | El lenguaje de programación | Todo el análisis del semestre se escribe en Python |
| **Git** | Programa que descarga y sincroniza carpetas de código | Es el canal por el que baja el material de cada clase |
| **Entorno virtual (`.silver`)** | Una carpeta con las librerías de *este* curso, separadas del resto del computador | Evita que instalar algo para la universidad le rompa otra cosa que ya tenía |
| **Librerías** | Código que otras personas ya escribieron: pandas, matplotlib, scikit-learn... | Son las herramientas del oficio |
| **VS Code + extensiones** | El editor donde se escribe y se ejecuta el código | Con las extensiones Python y Jupyter, ejecuta notebooks |

Cómo encajan: **Git** trae la carpeta del curso. Dentro de esa carpeta, **Python** crea el **entorno
virtual `.silver`**. Dentro de `.silver` se instalan las **librerías**. Y **VS Code** abre la carpeta y
ejecuta el código usando ese entorno.

```
carpeta del curso  (la trae Git)
  .silver/         el entorno virtual  (lo crea Python)
    pandas, numpy, matplotlib, ...     (las instala pip)
  clase01/  clase02/  ...  datos/      el material
```

**El entorno virtual no se comparte y no se sube a ningún lado.** Es suyo, vive en su computador y se
puede borrar y volver a crear en cinco minutos. Lo que se comparte es la lista de librerías, que es el
archivo `requirements.txt`.

---

## 2. Abrir una terminal

La terminal es una ventana donde se escriben comandos en vez de hacer clic.

Presione la tecla `Windows`, escriba `PowerShell` y abra **Windows PowerShell**. Use siempre
PowerShell en esta guía, no el "Símbolo del sistema" (CMD), salvo donde se diga.

Para comprobar que la entiende, escriba esto y presione `Enter`:

```powershell
cd
```

No pasa nada visible. Bien: el comando funcionó. La terminal solo habla cuando tiene algo que decir,
casi siempre un error.

Comandos que va a usar todo el semestre:

| Comando | Qué hace |
|---------|----------|
| `cd nombre-de-carpeta` | Entrar a una carpeta |
| `cd ..` | Salir a la carpeta de arriba |
| `dir` | Ver qué hay en la carpeta actual |
| `dir -Force` | Ver también lo oculto (como `.silver`) |
| `Get-Location` | Ver en qué carpeta está parado |

> **Al copiar comandos de esta guía, no copie el símbolo del sistema** (`PS C:\>`) si lo ve en
> capturas. Los bloques de código no lo traen: se copian tal cual.

---

## 3. Instalar Python

### 3.1 Qué versión

El curso pide **Python 3.12 o superior**. La recomendación es **Python 3.12**, la versión con la que se
probó el entorno completo sin conflictos.

Si tiene 3.13, sirve igual. Si tiene 3.11 o menos, actualice: `numpy` y `scipy`, en las versiones que
el material necesita, ya no se publican para esas versiones, y `pip` le va a instalar unas más viejas
sin avisarle.

#### Por qué el `requirements.txt` tiene versiones mínimas

Cuatro líneas llevan `>=` con un número: `pandas`, `numpy`, `scipy` y `scikit-learn`. Son las que
**producen los números** de los cuadernos (un promedio, un intervalo de confianza, el puntaje de un
modelo). Sus versiones nuevas cambian de vez en cuando algún detalle de cálculo, y eso mueve el
resultado en la tercera cifra. **El verificador de los retos compara una huella de su resultado contra
la esperada**, y una huella distinta es un rechazo aunque su código esté perfecto.

| Librería | Versión verificada | Piso en `requirements.txt` |
|----------|--------------------|----------------------------|
| pandas | 3.0.5 | `>=3.0.5` |
| numpy | 2.5.2 | `>=2.5.2` |
| scipy | 1.18.0 | `>=1.18.0` |
| scikit-learn | 1.9.0 | `>=1.9.0` |

Si el verificador rechaza algo que usted revisó y está correcto, **lo primero que hay que mirar son las
versiones**, con `pip list`. `>=` es un piso, no un clavo: permite versiones más nuevas, no más viejas.

### 3.2 Ver si ya lo tiene

```powershell
python --version
```

**Qué debería pasar:** imprime algo como `Python 3.12.5`. Si el número es 3.12 o mayor, salte a la
sección 4.

**Si dice que el comando no existe**, o si se abre la tienda de Microsoft, todavía no lo tiene
instalado. Siga abajo.

### 3.3 Instalar

1. Vaya a https://www.python.org/downloads/ y descargue el instalador de Windows.
2. Ejecute el archivo descargado.
3. **En la primera pantalla, ANTES de dar clic en "Install Now", marque la casilla
   "Add python.exe to PATH"**, abajo del todo.
4. Dé clic en **Install Now** y espere.
5. Cierre la terminal que tuviera abierta y **abra una nueva**. Es obligatorio: la terminal solo se
   entera de los programas nuevos al arrancar.

> **La trampa clásica.** Esa casilla es la causa número uno de "instalé Python y la terminal dice que
> no existe". `PATH` es la lista de sitios donde el sistema busca programas. Si ya instaló sin
> marcarla: vuelva a ejecutar el instalador, elija **Modify**, y active
> **"Add Python to environment variables"**.

**Verificar:**

```powershell
python --version
```

Debe imprimir `Python 3.12.x` o similar.

---

## 4. Instalar Git

Git descarga el material del curso y mantiene su copia al día. **No se enseña ni se evalúa en este
curso**: es la vía de entrega y nada más. Con `clone` y `pull` le alcanza.

**Ver si ya lo tiene:**

```powershell
git --version
```

Si imprime algo como `git version 2.43.0`, ya está. Si no, descargue e instale desde
https://git-scm.com/download/win , acepte todos los valores por defecto del asistente, cierre la
terminal y abra una nueva.

---

## 5. Clonar el repositorio del curso

**Clonar** es descargar una copia de la carpeta del curso, conectada al original. Se hace **una sola
vez en el semestre**. Después, cada semana se actualiza con `git pull` (sección 11).

### 5.1 Dónde conviene clonarlo

En una carpeta suya, con ruta corta y **sin espacios ni tildes en ningún nombre de la ruta**. Por
ejemplo, una carpeta llamada `Pruebas lab` (con espacio) no es buena idea. La sugerencia:

```
C:\Users\SU_USUARIO\Documents
```

Evite el Escritorio si está sincronizado con OneDrive: la sincronización en segundo plano puede
corromper el entorno virtual.

Ubíquese ahí:

```powershell
cd $HOME\Documents
```

### 5.2 Clonar

```powershell
git clone https://github.com/juliangarzon/analitica-datos-estudiantes.git analitica-datos
```

> **Copie el comando tal cual.** El repositorio en GitHub se llama `analitica-datos-estudiantes`, pero
> la carpeta que se crea en su computador se llama `analitica-datos`: eso lo hace la última palabra
> del comando, y es a propósito.

**Qué debería pasar:** varias líneas tipo `Cloning into 'analitica-datos'...` y `Receiving objects:
100%`. Tarda menos de un minuto.

**Cómo saber que salió bien:**

```powershell
cd analitica-datos
dir
```

Debe ver `clase01`, `clase02`, ..., `datos`, `requirements.txt`, `README.md`, `INSTALACION.md`,
`verificacion.ipynb`.

**A partir de aquí, todos los comandos se ejecutan desde dentro de `analitica-datos`.** Si cierra la
terminal y vuelve mañana, lo primero es volver a entrar con `cd`.

---

## 6. Crear y activar el entorno virtual `.silver`

### 6.1 Qué es y por qué

Un **entorno virtual** es una carpeta que contiene una copia aislada de Python con sus propias
librerías. Todo lo que instale mientras está activo se guarda ahí y en ningún otro lado.

Sin él, cada `pip install` se le mete al Python del sistema, el mismo que usan todos sus proyectos.
Dos proyectos que necesiten versiones distintas de la misma librería entran en conflicto. Con entorno
virtual, el peor escenario es borrar la carpeta `.silver` y empezar de nuevo.

La regla mental: **una carpeta de proyecto, un entorno virtual.**

### 6.2 Si ya tenía un entorno anterior (opcional)

Si está rehaciendo el entorno y quedó una carpeta vieja (por ejemplo `.adzzch` o `.venv`) dentro de
`analitica-datos`, puede borrarla para no confundirse. **Solo hágalo si está seguro de que está en la
carpeta correcta**, porque borrar no tiene vuelta atrás:

```powershell
deactivate
Remove-Item -Recurse -Force .\.adzzch
```

Cambie `.adzzch` por el nombre exacto de la carpeta vieja. Si `deactivate` dice que no existe, no
pasa nada: significa que no había un entorno activo.

### 6.3 Crearlo

Desde dentro de `analitica-datos`:

```powershell
python -m venv .silver
```

**Qué debería pasar:** nada en pantalla, y tarda unos segundos. Aparece una carpeta nueva llamada
`.silver`. El punto al principio la hace oculta en el explorador de archivos; es normal.

**Confirme que quedó bien creada:**

```powershell
Test-Path .\.silver\Scripts\Activate.ps1
```

Debe imprimir `True`. Si imprime `False`, no está en la carpeta correcta (revise con `Get-Location`) o
el comando de creación falló.

**Se crea una sola vez.** No repita este comando cada semana.

### 6.4 Evitar que Git la vea

El `.gitignore` del repositorio ignora `.venv`, pero **no conoce el nombre `.silver`**. Sin este paso,
`git status` mostrará la carpeta como "sin seguimiento" y podría subirla por accidente. Para que Git
la ignore solo en su computador, sin modificar el repositorio, ejecute:

```powershell
Add-Content .git\info\exclude ".silver/"
```

Para comprobar, `git status` ya no debe mencionar `.silver`.

### 6.5 Activarlo

Activar es decirle a la terminal: "de aquí en adelante, cuando diga `python` o `pip`, use los de esta
carpeta".

```powershell
.\.silver\Scripts\Activate.ps1
```

Fíjese en los **dos puntos**: el primero con la barra (`.\`) significa "en esta carpeta", y el segundo
(`.silver`) es el nombre real.

**Cómo saber que salió bien:** al principio de la línea de la terminal aparece `(.silver)`.

```
(.silver) PS C:\Users\ana\Documents\analitica-datos>
```

Si no ve `(.silver)`, el entorno **no** está activo, y todo lo que haga después va al sitio
equivocado.

> **La trampa número uno del semestre.** La activación dura lo que dure esa ventana de terminal. Al
> cerrarla, o al relanzarla desde VS Code, se pierde. Mañana, al abrir una terminal nueva, hay que
> activar otra vez. Es el origen del 90% de los "ayer me funcionaba y hoy no". **Antes de escribir
> cualquier comando, mire si dice `(.silver)`.**

Para desactivarlo, cuando termine de trabajar (opcional; cerrar la terminal hace lo mismo):

```powershell
deactivate
```

**Si aparece un error rojo que menciona "ejecución de scripts está deshabilitada"**, vaya a la
sección 10, problema 3. Es un ajuste de seguridad de Windows y se arregla con un comando.

**Alternativa sin cambiar nada:** use CMD en vez de PowerShell y active con
`.silver\Scripts\activate.bat`.

---

## 7. Instalar las librerías

### 7.1 Qué es pip y qué es requirements.txt

**pip** es el instalador de librerías de Python. Va a internet, descarga lo que le pida y lo deja
dentro del entorno virtual activo.

**`requirements.txt`** es un archivo de texto con la lista de librerías del curso, una por línea. La
lista es la misma para todo el salón.

### 7.2 Instalar

**Con `(.silver)` visible en la terminal**, y desde la carpeta `analitica-datos`:

```powershell
pip install -r requirements.txt
```

**Qué debería pasar:** decenas de líneas `Collecting ...`, `Downloading ...`, barras de progreso, y al
final una línea larga que empieza con `Successfully installed`. Descarga unos cuantos cientos de
megabytes.

**Cuánto tarda:** entre 2 y 10 minutos según su conexión. Es normal que parezca colgado en algún
paquete grande; espere.

Puede aparecer un aviso amarillo diciendo que hay una versión nueva de pip. Es informativo, no un
error. Ignórelo.

> **Si no tiene el `requirements.txt`** (por ejemplo, si no clonó el repositorio), puede instalar lo
> mismo a mano. Con `(.silver)` activo:
>
> ```powershell
> pip install "pandas>=3.0.5" "numpy>=2.5.2" "scipy>=1.18.0" "scikit-learn>=1.9.0" matplotlib seaborn plotly streamlit ipykernel jupyterlab
> ```
>
> Las comillas son necesarias en PowerShell para que `>` no se interprete como redirección.

### 7.3 Verificar

```powershell
pip list
```

Deben estar `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly`, `streamlit`, `scipy`,
`scikit-learn`, `ipykernel` y `jupyterlab`, entre muchas dependencias. Mire de paso los números de
`pandas`, `numpy`, `scipy` y `scikit-learn`: deben ser iguales o mayores a los de la tabla de la
sección 3.1. Si alguno salió menor, casi siempre es porque su Python es anterior a 3.12.

Una comprobación más directa:

```powershell
python -c "import pandas, numpy, matplotlib, seaborn, plotly, streamlit, sklearn, scipy; print('Entorno listo')"
```

Si imprime `Entorno listo`, esta parte terminó.

Y para confirmar que está usando el Python correcto:

```powershell
python -c "import sys; print(sys.prefix)"
```

La ruta que imprime debe terminar en `.silver`.

---

## 8. VS Code y los notebooks

### 8.1 Instalar VS Code

Visual Studio Code es el editor que se usa en clase. Es gratis.

1. Descargue de https://code.visualstudio.com/ e instale.
2. Si el instalador ofrece "Add to PATH" o "Agregar acción Abrir con Code", acepte.

### 8.2 Instalar las dos extensiones

Ambas deben estar publicadas por **Microsoft** (hay imitaciones con nombres parecidos).

1. Abra VS Code.
2. Clic en el icono de **Extensiones** en la barra izquierda (cuatro cuadritos), o `Ctrl+Shift+X`.
3. Busque **Python** (de Microsoft) e instale.
4. Busque **Jupyter** (de Microsoft) e instale.

La extensión Python le enseña a VS Code qué es Python. La extensión Jupyter le permite abrir y
ejecutar archivos `.ipynb`, que son los notebooks del curso.

### 8.3 Abrir la carpeta del curso

**Menú `File` > `Open Folder...`** y elija la carpeta `analitica-datos` completa.

No abra archivos sueltos: abra **la carpeta**. VS Code necesita ver la carpeta entera para encontrar
el entorno virtual y para que las rutas relativas a `datos/` funcionen. Si aparece un cuadro
preguntando si confía en los autores de la carpeta, responda que sí.

### 8.4 Seleccionar el intérprete `.silver`

**Este es el paso donde más gente se atasca**, porque VS Code escoge un Python por su cuenta y casi
nunca es el correcto. El síntoma es un `ModuleNotFoundError` en un computador donde las librerías sí
están instaladas.

1. Presione `Ctrl+Shift+P`. Se abre una barra de búsqueda de comandos.
2. Escriba `Python: Select Interpreter` y presione `Enter`.
3. En la lista, elija el que dice **`.silver`**. La ruta se ve así:
   `.\.silver\Scripts\python.exe`
4. Si no aparece en la lista: elija `Enter interpreter path...` > `Find...` y navegue a mano hasta
   ese archivo. Recuerde que la carpeta es oculta; si no la ve en el selector, escriba la ruta
   directamente.

VS Code lo recuerda para esta carpeta. No hay que repetirlo cada día.

Para que las terminales **nuevas** que abra VS Code activen el entorno solas, confirme en `Ctrl+,`
(Configuración) que `python.terminal.activateEnvironment` esté activada (lo está por defecto).

### 8.5 Qué es un notebook y cómo se ejecuta

Un **notebook** (`.ipynb`) mezcla texto y código en bloques llamados **celdas**. Se ejecuta un pedazo,
se mira el resultado, se ajusta, se sigue.

- **Celda de texto** (markdown): explicaciones. No hace nada al ejecutarse.
- **Celda de código**: Python. Al ejecutarla, muestra su resultado justo debajo.

**Cómo se ejecuta una celda:** clic dentro de ella y `Shift + Enter`. También hay un botón de "play"
a la izquierda de cada celda.

**El número entre corchetes**, a la izquierda de cada celda de código:

| Se ve | Significa |
|-------|-----------|
| `[ ]` | Nunca se ha ejecutado en esta sesión |
| `[*]` | Se está ejecutando ahora mismo. Espere |
| `[3]` | Terminó, y fue la tercera celda que se ejecutó en esta sesión |

Ese número es el **orden real de ejecución**. **Regla: ejecute siempre de arriba hacia abajo.**

**El kernel** es el proceso de Python que está corriendo el notebook por detrás. Se ve arriba a la
derecha y debe decir `.silver`. Si dice otra cosa, haga clic ahí, elija **Select Another Kernel** >
**Python Environments** y escoja `.silver`.

Si algo se enreda sin explicación, **Restart** en la barra superior reinicia el kernel: borra todas
las variables. Después hay que volver a ejecutar desde la primera celda.

---

## 9. Verificación final

En la raíz del repositorio hay un archivo llamado **`verificacion.ipynb`**. Es la prueba de que todo
quedó bien: importa las ocho librerías, lee un CSV real del repositorio y pinta un gráfico.

1. En VS Code, con la carpeta `analitica-datos` abierta, haga clic en `verificacion.ipynb`.
2. Confirme que arriba a la derecha el kernel dice `.silver` (sección 8.4).
3. Ejecute las celdas de arriba hacia abajo con `Shift + Enter`.

**Qué debería pasar:**

- La primera celda imprime su versión de Python y la ruta del intérprete.
- La segunda imprime las versiones de las ocho librerías.
- La tercera dice `Filas: 21816` y `Columnas: 12`, y lista los nombres de las columnas.
- La cuarta muestra una tabla con las primeras cinco filas.
- La quinta pinta un gráfico de barras horizontal.
- La última imprime `Entorno listo`.

> **Atención con el nombre `.silver`.** El material del curso fue escrito pensando en un entorno
> llamado `.venv`, y las celdas de verificación pueden revisar si la ruta del intérprete **contiene
> `.venv`**. Con `.silver` es posible que una celda imprima un `AVISO` diciendo que el intérprete no
> es el esperado, **aunque todo esté bien**. Lo que importa es que la ruta impresa contenga
> **`.silver`**. Si contiene `.silver`, está usando el entorno correcto: ignore ese aviso en
> particular. Si no lo contiene, el kernel o el intérprete están mal seleccionados (sección 8.4).

Si llegó al final sin ningún recuadro rojo de error, terminó. Cierre el notebook **sin guardar**.

Si prefiere no usar VS Code, el mismo notebook se puede abrir en el navegador. Con `(.silver)`
activo:

```powershell
jupyter lab
```

Se abre solo. Para cerrarlo, `Ctrl+C` en la terminal.

### 9.1 Cada notebook de clase se verifica solo

Los `demo.ipynb` y `reto.ipynb` del curso empiezan con una celda de **verificación**: importa las
librerías de esa clase, imprime la ruta del intérprete y confirma que el CSV de la clase está donde
debe. Ejecútela siempre primero.

**Si esa celda falla, deténgase ahí.** El problema es de entorno, no del contenido de la clase, y el
resto del notebook va a fallar en cadena. Recuerde lo de la nota anterior: con `.silver`, un `AVISO`
sobre `.venv` puede ser solo un falso positivo; lo que debe mirar es que la ruta contenga `.silver`.

---

## 10. Problemas frecuentes

### Problema 1: `python` no se reconoce como comando

**Síntoma.** La terminal dice que no conoce el comando, o se abre la Microsoft Store.

**Causa.** Python no está instalado, o está instalado pero no en el `PATH`, o abrió la terminal antes
de instalarlo.

**Solución.**

1. Cierre **todas** las terminales y abra una nueva. Muchas veces es solo eso.
2. Reinstale desde https://www.python.org/downloads/ marcando **"Add python.exe to PATH"**, o ejecute
   el instalador otra vez, elija **Modify** y active **"Add Python to environment variables"**.

### Problema 2: `pip` no se reconoce como comando

**Causa.** Casi siempre el entorno virtual no está activo.

**Solución.**

1. Mire si la línea de la terminal empieza con `(.silver)`. Si no, actívelo (sección 6.5).
2. Si aun activo falla, use la forma larga, que siempre funciona:

```powershell
python -m pip install -r requirements.txt
```

### Problema 3: "la ejecución de scripts está deshabilitada en este sistema"

**Síntoma.** Al activar el entorno sale un texto rojo largo que menciona `UnauthorizedAccess` o
`execution policy`.

**Causa.** Windows bloquea por defecto la ejecución de scripts de PowerShell, y el activador del
entorno es uno de esos scripts.

**Solución.** En la misma ventana de PowerShell:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

Confirme con `S` o `Y`. Es un cambio limitado a su usuario y solo permite ejecutar scripts locales.
Luego vuelva a activar:

```powershell
.\.silver\Scripts\Activate.ps1
```

### Problema 4: "No se encuentra la ruta de acceso ... `.silver` porque no existe"

**Síntoma.** Al activar o listar, PowerShell dice que la ruta no existe.

**Causas, de la más a la menos frecuente.**

1. **Está parado en otra carpeta.** El comando se ejecuta desde la carpeta que **contiene** `.silver`
   (la raíz `analitica-datos`), no desde dentro de `.silver` ni desde otra ruta.
2. **Olvidó el punto del nombre.** Es `.silver`, no `silver`.
3. **El entorno nunca se creó** o se creó en otra carpeta.

**Diagnóstico.**

```powershell
Get-Location
dir -Force
Test-Path .\.silver\Scripts\Activate.ps1
```

`Get-Location` dice dónde está. `dir -Force` muestra también las carpetas ocultas: debe aparecer
`.silver`. `Test-Path` debe dar `True`.

**Para buscarlo por todo su usuario**, si no recuerda dónde lo creó:

```powershell
Get-ChildItem -Path $env:USERPROFILE -Directory -Recurse -Force -Filter .silver -ErrorAction SilentlyContinue | Select-Object FullName
```

Si no aparece o no tiene `Scripts\Activate.ps1`, créelo de nuevo (sección 6.3).

### Problema 5: VS Code no encuentra el kernel, o la lista está vacía

**Causa.** Falta la extensión Jupyter, falta `ipykernel` dentro del entorno, o VS Code no está viendo
la carpeta correcta.

**Solución, en este orden.**

1. Confirme que abrió **la carpeta** `analitica-datos` (`File > Open Folder`), no un archivo suelto.
2. Confirme que las extensiones **Python** y **Jupyter** de Microsoft están instaladas (sección 8.2).
3. Con `(.silver)` activo en la terminal: `pip install ipykernel`.
4. Recargue VS Code: `Ctrl+Shift+P` > `Developer: Reload Window`.
5. Seleccione el intérprete otra vez: `Ctrl+Shift+P` > `Python: Select Interpreter` > `.silver`.

### Problema 6: `ModuleNotFoundError: No module named 'pandas'` aunque sí lo instalé

**Causa.** Se instaló en un Python y se está ejecutando con otro.

**Solución.**

1. Ejecute en una celda: `import sys; print(sys.prefix)`. **Si esa ruta no termina en `.silver`, ese
   es el problema.**
2. Seleccione el intérprete correcto (sección 8.4) y **reinicie el kernel** (botón `Restart`).
3. Si la ruta sí termina en `.silver`, la instalación se hizo sin el entorno activo. Actívelo
   (sección 6.5) y repita `pip install -r requirements.txt`.

### Problema 7: `FileNotFoundError` al leer un CSV

**Causa.** Las rutas de los notebooks son **relativas** a la carpeta donde está el notebook. Si movió
archivos de sitio, o abrió el notebook desde otra carpeta, la ruta deja de apuntar a donde debe.

**Solución.**

1. No mueva ni renombre carpetas del repositorio. `datos/` vive en la raíz y los notebooks de clase la
   alcanzan con `../datos/`.
2. Abra siempre la carpeta raíz `analitica-datos` en VS Code.
3. Si el CSV no existe todavía, revise que hizo `git pull`: los datos de cada clase se publican junto
   con el material de esa clase.

### Problema 8: `git pull` falla porque edité un archivo del repositorio

**Síntoma.**

```
error: Your local changes to the following files would be overwritten by merge:
        clase04/demo.ipynb
```

**Solución para salir del paso**, descartando sus cambios en ese archivo:

```powershell
git checkout -- clase04/demo.ipynb
git pull
```

Si quiere conservar su trabajo, primero cópielo con otro nombre.

**Convención del curso, que evita este problema por completo: trabaje siempre sobre una copia.**

```powershell
copy clase04\demo.ipynb clase04\demo_mio.ipynb
```

El archivo `demo.ipynb` queda intacto, `git pull` nunca reclama, y su trabajo vive en
`demo_mio.ipynb`.

### Problema 9: aparece un triángulo de advertencia en la pestaña de la terminal de VS Code

**Síntoma.** Junto al nombre de la terminal aparece un triángulo amarillo. Al pasar el mouse, dice
que una extensión (por ejemplo **Python Debugger**) quiere relanzar la terminal para aportar a su
entorno.

**Causa.** No es un error. Una extensión cambió variables de entorno después de abrir esa terminal, y
VS Code avisa que está desactualizada.

**Solución.** Haga clic en **Relaunch Terminal** dentro de ese mensaje, o use `Ctrl+Shift+P` >
`Terminal: Relaunch Terminal`. Si hay algo corriendo en esa terminal, se detiene.

**Consecuencia importante: al relanzar se pierde la activación de `.silver`.** Hay que volver a
activarlo (sección 6.5). Si prefiere no ver el aviso, en `Ctrl+,` cambie
`terminal.integrated.environmentChangesIndicator` a `off`.

### Problema 10: `git status` muestra `.silver` como archivo sin seguimiento

**Causa.** El `.gitignore` del repositorio conoce `.venv`, pero no `.silver`.

**Solución.**

```powershell
Add-Content .git\info\exclude ".silver/"
```

Si ya la había agregado con `git add` por error, antes de hacer `commit` ejecute
`git rm -r --cached .silver` para sacarla.

### Problema 11: Se me dañó todo y no sé qué toqué

**Solución.** El entorno virtual es desechable. Bórrelo y vuelva a crearlo; no pierde nada porque ahí
no vive su trabajo.

```powershell
deactivate
Remove-Item -Recurse -Force .\.silver
python -m venv .silver
.\.silver\Scripts\Activate.ps1
pip install -r requirements.txt
```

Después, vuelva a seleccionar el intérprete en VS Code (sección 8.4) si hace falta.

---

## 11. La rutina de cada semana

La instalación fue una sola vez. Lo de cada clase son tres líneas:

```powershell
cd $HOME\Documents\analitica-datos
git pull
.\.silver\Scripts\Activate.ps1
```

Y después, abrir VS Code en esa carpeta.

Cuatro cosas para no olvidar:

1. **`git pull` antes de cada clase.** Si no, llega con el material de la semana pasada.
2. **Activar el entorno en cada terminal nueva o relanzada.** Busque el `(.silver)`.
3. **Trabajar sobre copias, no sobre los archivos originales del repositorio.**
4. **Ejecutar primero la celda de verificación del notebook** (sección 9.1). Son dos segundos y le
   dice si el entorno está bien antes de que empiece la clase, no a mitad de camino.
