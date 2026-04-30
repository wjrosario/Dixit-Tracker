# 📖 Guía Completa de Deploy en GitHub Pages

## ✅ Checklist Rápido

- [ ] .NET 8 SDK instalado (`dotnet --version` → 8.0.x)
- [ ] Repo creado en GitHub (público)
- [ ] Archivos pusheados a `main` branch
- [ ] Settings → Pages → GitHub Actions activado
- [ ] Workflow ejecutado (pestaña Actions → ✅ verde)
- [ ] Sitio live en `https://usuario.github.io/dixit-tracker/`

---

## 🔧 Instalación Local Paso a Paso

### 1. Instalar .NET 8

#### Windows:
- Descargá de https://dotnet.microsoft.com/download/dotnet/8.0
- Ejecutá el instalador
- Reiniciá la terminal/CMD

#### macOS:
```bash
brew install dotnet@8
```

#### Linux (Ubuntu/Debian):
```bash
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0
```

**Verificar:**
```bash
dotnet --version
# Debe mostrar: 8.0.x
```

---

## 🚀 Deploy a GitHub Pages

### Opción A: Usando GitHub Web UI (más fácil)

**1. Crear repo:**
- https://github.com/new
- Name: `dixit-tracker`
- Public ✓
- Create

**2. Suban los archivos:**
```bash
# Clone el repo vacío
git clone https://github.com/TU_USER/dixit-tracker.git
cd dixit-tracker

# Copien los 14 archivos del proyecto aquí

# Suban
git add .
git commit -m "Initial: Dixit Tracker v1"
git push origin main
```

**3. Habilitar Pages:**
- Repo → Settings
- Pages (en el menú izquierdo)
- Source: **GitHub Actions** (NO "Deploy from branch")
- Save

**4. Esperar:**
- Pestaña "Actions"
- Debería ver "Deploy Dixit Tracker..." ejecutándose
- Esperar ~3 min a que termine (checkmark verde ✅)

**5. Acceder:**
```
https://TU_USER.github.io/dixit-tracker/
```

---

## 🐛 Problemas Comunes y Soluciones

### Problema 1: "404 Not Found" en GitHub Pages

**Síntomas:**
- Entro a la URL del sitio y veo error 404
- Pero veo el workflow ejecutado (Actions → ✅)

**Causas posibles:**
1. GitHub Pages apunta a rama incorrecta
2. Caché del navegador
3. El workflow terminó con error silencioso

**Soluciones:**

**A) Verificar configuración de Pages:**
```
Repo → Settings → Pages
- Source DEBE ser: "GitHub Actions" (no "Deploy from branch")
- Si dice otra cosa, cámbialo y Guarda
```

**B) Limpiar caché del navegador:**
- Windows: `Ctrl + Shift + Delete`
- Mac: `Cmd + Shift + Delete`
- O Abrí en navegación privada (Incognito)

**C) Ver logs del workflow:**
```
Repo → Actions → "Deploy Dixit..." → Click
Ver la salida del job "Deploy Dixit Tracker to GitHub Pages"
```

**D) Forzar re-deploy:**
```bash
git commit --allow-empty -m "Trigger redeploy"
git push origin main
```

---

### Problema 2: "Archivo no encontrado" o "recursos no cargan"

**Síntomas:**
- La página carga pero están rotos: CSS, JS, imágenes
- Consola (F12) muestra 404 para archivos `.css`, `.js`

**Causa:**
- El `<base href>` en `index.html` es incorrecto para tu setup de GitHub Pages

**Soluciones:**

**Si el repo se llama `dixit-tracker`:**

En `wwwroot/index.html`, línea 9, debe estar:
```html
<base href="/" />
```

El workflow automáticamente ajusta esto para GitHub Pages si es necesario.

**Si customizaste el nombre del repo:**
- Abrí `wwwroot/index.html`
- Buscá `<base href=`
- Debe ser `/` (porque GitHub Pages se configura en user repo settings)

---

### Problema 3: "El sitio carga pero no me deja marcar mazos"

**Síntomas:**
- Veo los mazos
- Hago click pero nada pasa
- Consola (F12) muestra errores de Blazor

**Causas:**
- Blazor no se cargó correctamente
- Problema con `_framework/` archivos

**Soluciones:**

1. **Recargá la página:** `Ctrl + Shift + R` (hard refresh)
2. **Borrá datos locales:**
   - F12 → Application → Local Storage → Borrar todo
   - Recargá
3. **Verificá que `_framework/` existe:**
   - En tu navegador, abrí: `https://usuario.github.io/dixit-tracker/_framework/`
   - Debería listar archivos `.wasm`, `.json`, etc.
   - Si ves 404 → el workflow no publicó correctamente

---

### Problema 4: "localStorage no guarda mi colección"

**Síntomas:**
- Marcó mazos
- Cierro la pestaña
- Vuelvo y perdí la selección

**Causas:**
1. Navegador en modo incógnito/privado
2. localStorage bloqueado (extensión o configuración)
3. Sitio en lista de bloqueados

**Soluciones:**

1. **Modo normal:** No incógnito/privado
2. **Deshabilitar extensiones:** Navegador → Extensiones → Deshabilitar todas → Recargá
3. **Limpiar datos:** F12 → Application → Clear all → Recargá
4. **Verificar en consola:**
   ```javascript
   // F12 → Console
   localStorage.setItem('test', 'funciona');
   localStorage.getItem('test');
   // Debería mostrar: "funciona"
   ```

---

## 🔍 Debugging

### Ver logs del workflow:

```
https://github.com/TU_USER/dixit-tracker/actions
```

Haz click en el último workflow → ver cada paso.

**Qué buscar:**
- ✅ "Setup .NET 8" — debe estar verde
- ✅ "Restore dependencies" — debe estar verde
- ✅ "Publish Blazor app" — debe estar verde
- ✅ "Upload artifact" — debe estar verde
- ✅ "Deploy" — debe estar verde

Si alguno está rojo ❌, expande para ver el error.

---

### Ver si los archivos llegaron a Pages:

```bash
# En tu máquina local
cd dixit-tracker
dotnet publish -c Release -o release
ls release/wwwroot/

# Debería listar:
# _framework/
# _app.bundle.{hash}.js
# app.css
# index.html
# 404.html
# ...
```

---

## 💡 Tips Avanzados

### Cambiar URL del sitio

Si quieres que el sitio esté en otra URL (ej: subdominio), deberías usar GitHub User/Org Pages en lugar de Project Pages. Es más complejo — por ahora mantenemos `/dixit-tracker/`.

### Versionar cambios

```bash
# Después de editar algo (ej: agregar un mazo)
git add .
git commit -m "Add Dixit: NewMaze expansion"
git push origin main
# El workflow se ejecuta automáticamente
```

### Probar localmente antes de subir

```bash
dotnet run
# Abrí https://localhost:5000
# Probá los cambios
# Si todo anda bien, hacé push
```

---

## 📞 Contacto / Más Ayuda

Si nada funciona:

1. Verificá que **Settings → Pages → Source** esté en **GitHub Actions**
2. Borrá el caché del navegador (`Ctrl+Shift+Delete`)
3. Esperá 5 minutos
4. Intentá en otra navegador (Chrome, Firefox, etc.)
5. Verificá que el workflow en Actions esté ✅ (verde)

---

**¡Listo! Tu sitio debería estar funcionando.** 🎉
