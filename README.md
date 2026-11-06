# pcb-srad-v4

Computadora de vuelo **SRAD v4** de Monterrey Rocket Lab, diseñada en Altium Designer.

---

## Requisitos (Windows)

1. **Altium Designer**
2. **Git for Windows** (ya incluye Git LFS). En PowerShell:
   ```powershell
   winget install --id Git.Git -e
   ```
3. Configuración inicial, una sola vez:
   ```powershell
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu-correo-de-github@ejemplo.com"
   git config --global core.autocrlf true
   git lfs install
   ```
4. Clonar el repo (usa una ruta corta, **no** dentro de OneDrive):
   ```powershell
   mkdir C:\MRL
   cd C:\MRL
   git clone https://github.com/MTY-RocketLab/pcb-srad-v4.git
   ```

> ⚠️ Si no corres `git lfs install`, los archivos de Altium no se suben bien.

---

## Flujo de trabajo

Los archivos de Altium son binarios: **Git no puede fusionar cambios de dos personas en el mismo archivo.** Por eso cada archivo se bloquea antes de editarlo.

1. **Toma un issue** del tablero, asígnatelo y muévelo a *In progress*.
2. **Actualiza y crea tu rama:**
   ```powershell
   git checkout main
   git pull
   git checkout -b schematic/12-barometro
   ```
   Formato: `<tipo>/<#issue>-<descripcion>` (tipo = `schematic`, `layout`, `library`, `docs`).
3. **Bloquea los archivos que vas a editar:**
   ```powershell
   git lfs lock Schematics/03_Sensors.SchDoc
   ```
   Los archivos `.SchDoc`, `.PcbDoc`, `.SchLib`, `.PcbLib` y `.PrjPcb` son de solo lectura hasta que los bloqueas. Si Altium los abre como *read-only*, te faltó este paso.
   Para ver quién tiene qué bloqueado: `git lfs locks`
4. **Trabaja en Altium.** Guarda seguido.
5. **Commit y push:**
   ```powershell
   git add .
   git commit -m "Add barometer schematic"
   git push -u origin schematic/12-barometro
   ```
6. **Abre un Pull Request** en GitHub. En la descripción pon `Closes #12`.
7. **Review:** Nadie puede mergear a `main` sin aprobación.
8. **Después del merge, libera los archivos:**
   ```powershell
   git lfs unlock Schematics/03_Sensors.SchDoc
   ```

> Antes de hacer `git pull` o cambiar de rama, **cierra el proyecto en Altium.**

---

## Estructura

```
pcb-srad-v4/
├── SRAD_v4.PrjPcb     Proyecto de Altium
├── Schematics/        Hojas de esquemático (.SchDoc)
├── PCB/               Layout (.PcbDoc)
├── Libraries/         Símbolos (.SchLib) y footprints (.PcbLib)
└── Docs/              Notas de diseño, cálculos, imágenes
```

Gerbers, BOM y PDFs de cada versión fabricada se publican en **Releases**, no en el repo.

---

## Reglas

- Nada se sube directo a `main`: todo pasa por Pull Request.
- Un issue = una rama = un PR.
- Antes de abrir el PR, corre **ERC** (esquemático) o **DRC** (PCB) sin errores.
- No edites un archivo bloqueado por otra persona.
