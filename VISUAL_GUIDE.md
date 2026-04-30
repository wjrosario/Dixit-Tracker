# 📸 Guía Visual — Paso a Paso

## 🎯 Objetivo Final

Después de seguir esta guía, tendrás:

```
✅ Tu tracker personal de colección Dixit
✅ Alojado en GitHub Pages (gratis, siempre online)
✅ Con almacenamiento automático en tu navegador
✅ Puedes compartir tu colección con un link
```

URL final: `https://TU_USUARIO.github.io/dixit-tracker/`

---

## ⏱️ Tiempo Total: 15 minutos

- Setup inicial: 5 min
- Deploy: 5 min
- Esperar GH Actions: 3 min

---

## Paso 1️⃣: Descargar el Proyecto (2 min)

### Opción A: Download ZIP

1. Descargá `dixit-tracker.zip` desde tu correo/drive
2. Descomprimí en una carpeta
3. Abrí terminal/CMD en esa carpeta

```bash
cd dixit-tracker
```

### Opción B: Clone from Git (si tenés Git instalado)

```bash
git clone https://github.com/TU_USUARIO/dixit-tracker.git
cd dixit-tracker
```

---

## Paso 2️⃣: Instalar .NET 8 (3 min)

### Windows

1. Descargá: https://dotnet.microsoft.com/download/dotnet/8.0
2. Haz click en el instalador `.exe`
3. Sigue el wizard
4. **Reinicia tu máquina**

### macOS

```bash
brew install dotnet@8
```

### Linux (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
```

### Verificar instalación

Abrí terminal y ejecutá:

```bash
dotnet --version
```

**Debe mostrar:** `8.0.x` o similar ✅

---

## Paso 3️⃣: Correr Localmente (2 min)

En la terminal, dentro de la carpeta `dixit-tracker`:

```bash
dotnet run
```

Esperá a que salga algo como:

```
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:5000
      Application started.
```

Abrí en tu navegador: `https://localhost:5000`

**Deberías ver:**
```
🎴 Colección Dixit
   [barra de progreso]
   [tarjetas de mazos]
```

Probá marcar un mazo → debe quedar verde ✅

---

## Paso 4️⃣: Subir a GitHub (3 min)

### 4a. Crear repo en GitHub

1. Entrá a https://github.com/new
2. **Repository name:** `dixit-tracker`
3. **Public** ✓
4. Click **"Create repository"**

Verás una pantalla con instrucciones. Copia el URL del repo (ej: `https://github.com/tu-usuario/dixit-tracker.git`)

### 4b. Hacer push desde tu máquina

En tu terminal (misma carpeta):

```bash
git init
git add .
git commit -m "Initial: Dixit Tracker"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/dixit-tracker.git
git push -u origin main
```

(Reemplazá `TU_USUARIO` con tu usuario real de GitHub)

**Resultado:** Los archivos están ahora en GitHub ✅

---

## Paso 5️⃣: Activar GitHub Pages (2 min)

1. Entrá a tu repo en GitHub
2. Click en **Settings** (arriba a la derecha)
3. En el menú izquierdo, click en **Pages**
4. En **Source**, seleccioná **GitHub Actions** (no "Deploy from branch")
5. Click **Save**

**Resultado:** El workflow se ejecuta automáticamente

---

## Paso 6️⃣: Esperar Deploy (3-5 min)

1. En tu repo, click en la pestaña **Actions**
2. Deberías ver un workflow corriendo: "Deploy Dixit Tracker..."
3. **Esperá a que tenga un checkmark verde ✅**

El workflow hace:
- ✅ Descargar .NET 8
- ✅ Compilar el código C#
- ✅ Publicar archivos estáticos
- ✅ Subir a GitHub Pages

---

## Paso 7️⃣: ¡LISTO! Acceder a tu Sitio 🎉

URL: `https://TU_USUARIO.github.io/dixit-tracker/`

(Reemplazá `TU_USUARIO` con tu usuario GitHub)

**Debería verse igual que cuando corriste localmente,**
**pero ahora es público y accesible desde cualquier navegador.**

---

## ✨ Características que Ya Funcionan

### 🖼️ Imágenes de Cajas

Cada mazo muestra la imagen real de la caja (desde store.asmodee.com)

### 💾 Guardado Automático

- Marcá un mazo
- Cierra el navegador
- Vuelve después
- **¡Tu selección está ahí!** ← Guardada en localStorage

### 📊 Estadísticas

- Total de mazos (15)
- Cuántos tenés
- Cuántos te faltan
- Barra de progreso

### 🔗 Compartir Colección

1. Click en **"Compartir"** (arriba a la derecha)
2. Se copia automáticamente un link como:
   ```
   https://tu-usuario.github.io/dixit-tracker/base,quest,harmonies,mirrors
   ```
3. Envialo a alguien
4. Esa persona ve tu colección en **modo lectura** (no puede editar)

---

## 🛠️ Hacer Cambios / Agregar Mazos

**Quiero agregar un mazo nuevo...**

### 1. Editar el archivo

Abrí: `Services/CollectionService.cs`

Encontrá la sección `AllGames` y agregá:

```csharp
new DixitGame {
    Id = "mi-nuevo-mazo",
    Name = "Dixit: Mi Nuevo Mazo",
    Artist = "Nombre del Artista",
    Year = "2025",
    Type = GameType.Expansion,  // o Base o Special
    FallbackEmoji = "🎨",
    ImageUrl = "https://url-de-imagen-de-la-caja.jpg",
    Description = "Breve descripción del mazo."
},
```

### 2. Probá localmente

```bash
dotnet run
# https://localhost:5000
```

Deberías ver el nuevo mazo ✅

### 3. Subí a GitHub

```bash
git add .
git commit -m "Add new expansion"
git push origin main
```

**En ~3 minutos:**
- GitHub Actions compila
- Publica en GitHub Pages
- ¡Tu sitio tiene el nuevo mazo! 🎉

---

## 🐛 Si Algo No Funciona

### Problema: "Veo 404 cuando entro a GitHub Pages"

**Soluciones:**

1. ¿Esperaste 3-5 minutos?
   - Sí → Continúa

2. ¿Settings → Pages → GitHub Actions activado?
   - No → Activalo y espera 5 min más
   - Sí → Continúa

3. ¿El workflow tiene checkmark verde?
   - Actions tab → mira el workflow
   - No verde → Haz click para ver los errores
   - Sí → Continúa

4. Probá con hard refresh:
   - Windows: `Ctrl + Shift + R`
   - Mac: `Cmd + Shift + R`

5. Probá en navegador privado (Incógnito)

---

### Problema: "Veo la página pero los mazos no funcionan"

**Síntomas:**
- Puedo ver las tarjetas
- Pero clickear no cambia nada

**Soluciones:**

1. Abrí la consola (F12 o Ctrl+Shift+I)
2. Ves errores rojos?
   - Sí → Captura pantalla y reporta
   - No → Continúa

3. Probá: `localStorage.setItem('test', 'funciona')`
   - Si dice "undefined" → localStorage está bloqueado
   - Desactiva extensiones que bloqueen storage

4. Probá en navegador privado

---

### Problema: "Mi colección no se guarda después de cerrar"

**Causa:** Probablemente estés en modo incógnito/privado

**Solución:**
- Usa modo normal (no incógnito)
- localStorage no funciona bien en privado

---

## 📞 Más Ayuda

1. Revisá `DEPLOY_GUIDE.md` (incluido en el ZIP)
2. Revisá `ARCHITECTURE.md` (para entender cómo funciona)
3. Ver logs en GitHub Actions tab
4. Probá en otro navegador

---

## 🎉 ¡Lo Hiciste!

Ahora tenés tu **tracker personal de colección Dixit**:

✅ Online y compartible
✅ Almacenamiento automático
✅ Código abierto (puedes modificarlo)
✅ 0% de costo (GitHub Pages es gratis)
✅ Mantenible en .NET 8 (language + framework estándares)

---

**¡A disfrutar de tu colección! 🎴✨**
