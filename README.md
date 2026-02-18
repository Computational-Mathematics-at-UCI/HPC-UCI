# 🚀 Guía Rápida de Uso del HPC-UCI con SLURM

Host: `10.1.6.70` nombre:`hpc-login.uci.cu`

## 1️⃣ **Usa `screen` para mantener tus sesiones activas**

Antes de lanzar tareas largas o trabajar en entornos interactivos:

```bash
screen -S miproyecto
```

Esto crea una sesión persistente llamada `miproyecto`. Si se corta la conexión, puedes volver con:

```bash
screen -r miproyecto
```

Para salir sin cerrar la sesión:

- Pulsa `Ctrl+A`, luego `D` (de detach)

---

## 2️⃣ **Ejecuta tareas con `srun` o `sbatch` según el caso**

### 🔹 Para sesiones interactivas (pruebas, compilación, notebooks, debugging):

```bash
srun --partition=interactive --ntasks=1 --time=01:00:00 --pty bash
```

### 🔹 Para tareas con GPU:

```bash
srun --partition=gpu  --ntasks=1 --time=02:00:00 --pty bash
```

### 🔹 Para tareas largas o automatizadas con `sbatch`:

Crea un script `mi_tarea.sh`:

```bash
#!/bin/bash
#SBATCH --partition=gpu
#SBATCH --ntasks=1
#SBATCH --time=04:00:00
#SBATCH --job-name=mi_simulacion
#SBATCH --output=salida_%j.log


conda activate mi_entorno
python mi_script.py
```

Y ejecútalo con:

```bash
sbatch mi_tarea.sh
```

---

## 3️⃣ **Buenas prácticas para un uso eficiente del HPC**

✅ Usa `screen` o `tmux` para evitar perder tu trabajo  
✅ Usa `srun` para pruebas interactivas, `sbatch` para producción  
✅ Elige la **partición adecuada**:  
- `interactive` para pruebas, compilación, notebooks, desarrollo  
- `gpu` para tareas que requieren aceleración gráfica  dedicada

✅ Usa `--time` para limitar la duración de tus jobs  
✅ Usa `--output=archivo.log` para guardar la salida  
✅ No ejecutes tareas pesadas directamente en el nodo login  directamente
✅ Limpia tus archivos temporales en `/scratch` cuando termines  
✅ Documenta tus scripts y entornos

---

## 4️⃣ **Crea y usa entornos con Conda o Anaconda**

### 🔹 Usar un entorno ya creado:

```bash

conda  activate ~/mi_entorno
```

o para los de uso general que se muestran con `conda env list` como `SuperPy`

```bash
conda  activate SuperPy
```


### 🔹 Crear un nuevo entorno en tu carpeta personal:

```bash

conda create --prefix ~/myenv python=3.11 numpy scipy matplotlib
```

Activar:

```bash

conda activate ~/myenv
```
Entornos disponibles: [Entornos](Entornos)

Desactivar:

```bash
conda deactivate
```

> 🧠 *Los entornos en tu carpeta personal (`~/myenv`) son privados y persistentes.*


