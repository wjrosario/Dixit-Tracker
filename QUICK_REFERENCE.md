# 🎴 Dixit Tracker — Referencia Rápida

## 📊 Estadísticas del Proyecto

| Métrica | Valor |
|---------|-------|
| **Mazos soportados** | 15 (4 base + 8 exp + 3 especiales) |
| **Líneas de código** | ~600 C# + Razor |
| **Líneas de CSS** | ~380 |
| **Archivos totales** | 18 |
| **Tamaño ZIP** | 26 KB |
| **Runtime WASM** | ~2 MB (500 KB gzipped) |
| **Dependencias externas** | 0 (solo .NET SDK) |

---

## 📁 Archivos Clave

### Core Lógica
```
✓ Services/CollectionService.cs       170 líneas | Persistencia + Share tokens
✓ Models/DixitGame.cs                 15 líneas  | Modelo de datos
✓ Pages/Index.razor                   198 líneas | UI completa
```

### UI / Estilos
```
✓ wwwroot/css/app.css                 380 líneas | Diseño responsive
✓ wwwroot/index.html                  58 líneas  | Host page
✓ App.razor + MainLayout.razor        Layouts
```

### Configuración
```
✓ dixit-tracker.csproj                .NET setup
✓ .github/workflows/deploy.yml        CI/CD a GitHub Pages
✓ Properties/launchSettings.json      Local dev config
```

### Documentación
```
✓ README.md                           Principal + instrucciones
✓ SETUP.md                            Setup rápido (5 min)
✓ DEPLOY_GUIDE.md                     Troubleshooting detallado
✓ ARCHITECTURE.md                     Diagramas + tech details
```

---

## 🚀 Quick Start (Copy-Paste)

```bash
# 1. Descargar ZIP y descomprimir
unzip dixit-tracker.zip
cd dixit-tracker

# 2. Instalar .NET 8 (si no lo tenés)
# Descargá de: https://dotnet.microsoft.com/download/dotnet/8.0

# 3. Correr localmente
dotnet run

# 4. Abrir navegador
# https://localhost:5000

# 5. Si todo anda bien, subir a GitHub
git init
git add .
git commit -m "Dixit Tracker"
git remote add origin https://github.com/TU_USER/dixit-tracker.git
git branch -M main
git push -u origin main

# 6. En GitHub: Settings → Pages → GitHub Actions → Save
# 7. En ~3 min estará en: https://tu-user.github.io/dixit-tracker/
```

---

## 🎯 15 Mazos Incluidos

### 🎲 Base (4)
- [x] **Dixit** (2008) — El clásico, Marie Cardouat
- [x] **Odyssey** (2011) — Hasta 12 jugadores
- [x] **Stella: Dixit Universe** (2021) — Mecánica invertida
- [x] **Cérémonie** (2024) — Portadas de juegos de mesa

### 📦 Expansiones (8)
- [x] **Quest** (2010) — Marie Cardouat
- [x] **Journey** (2012) — Xavier Collette
- [x] **Origins** (2013) — Clément Lefèvre
- [x] **Daydreams** (2014) — Franck Dion
- [x] **Memories** (2015) — Hinder & Pélissier
- [x] **Revelations** (2016) — Marina Coudray
- [x] **Harmonies** (2017) — Paul Echegoyen
- [x] **Mirrors** (2020) — Sébastien Telleschi

### ⭐ Especiales (3)
- [x] **Anniversary** (2018) — 10° aniversario
- [x] **Disney Edition** (2023) — Personajes Disney
- [x] **MNK** (2023) — Museo Nacional Cracovia

---

## 💾 Cómo Guarda la Colección

```javascript
// En tu navegador → localStorage
localStorage['dixit_collection_v3'] = '["base", "quest", "mirrors"]'
```

✅ Persiste entre visitas
✅ No requiere servidor
✅ Se sincroniza automáticamente

---

## 🔗 Compartir tu Colección

### Generar URL:
1. Marcá los mazos que tenés
2. Click **Compartir**
3. Se copia: `https://tuusuario.github.io/dixit-tracker/base,quest,harmonies,mirrors`

### Quién recibe el link:
- Ve tu colección en **modo lectura**
- No puede editar
- Puede crear la propia

---

## 🛠️ Agregar un Mazo Nuevo

**Archivo:** `Services/CollectionService.cs` → `AllGames`

```csharp
new DixitGame {
    Id = "nuevo-id",
    Name = "Dixit: Nuevo Mazo",
    Artist = "Nombre",
    Year = "2025",
    Type = GameType.Expansion,
    FallbackEmoji = "🎨",
    ImageUrl = "https://url-de-imagen.jpg",
    Description = "Descripción."
},
```

**Luego:**
```bash
git add .
git commit -m "Add new expansion"
git push origin main
# ✅ Deploy automático en 3 min
```

---

## 📋 Checklist de Deploy

- [ ] .NET 8 instalado (`dotnet --version`)
- [ ] Repo creado en GitHub
- [ ] Archivos pusheados a `main`
- [ ] Settings → Pages → **GitHub Actions** activado
- [ ] Workflow ejecutado (Actions tab → ✅)
- [ ] Sitio live en `https://usuario.github.io/dixit-tracker/`

---

## 🐛 Si algo no funciona

1. **Página muestra 404:**
   - Settings → Pages → GitHub Actions activado?
   - Esperaste 3 minutos?
   - Ctrl+Shift+R (hard refresh)?

2. **No puedo marcar mazos:**
   - Abrí F12 (Developer Tools)
   - Ves errores de Blazor?
   - Probá en navegador privado?

3. **localStorage no guarda:**
   - Modo privado/incógnito apagado?
   - `localStorage` no está bloqueado?
   - Probá: `localStorage.setItem('test','x')` en console

Ver `DEPLOY_GUIDE.md` para más detalles.

---

## 📊 Tech Stack

```
┌─────────────────────────────────────────────┐
│ Frontend: Blazor WebAssembly (.NET 8)       │
├─────────────────────────────────────────────┤
│ Styling: CSS3 puro                          │
├─────────────────────────────────────────────┤
│ Storage: localStorage (browser)             │
├─────────────────────────────────────────────┤
│ Hosting: GitHub Pages (estático)            │
├─────────────────────────────────────────────┤
│ CI/CD: GitHub Actions                       │
└─────────────────────────────────────────────┘
```

**No necesita:**
- ❌ Backend / Base de datos
- ❌ Node.js / npm
- ❌ Docker / Kubernetes
- ❌ Autenticación

---

## 🔐 Privacidad & Seguridad

- ✅ Todo en el navegador (no envías datos a servidor)
- ✅ HTTPS por defecto (GitHub Pages)
- ✅ Código abierto y auditable
- ✅ Share tokens son públicos (intención: que se vea la colección)
- ✅ Sin tracking, cookies ni anuncios

---

## 📈 Próximas Mejoras (Ideas)

```
[ ] Exportar colección como JSON/CSV
[ ] Tema oscuro (dark mode)
[ ] Búsqueda y filtrado
[ ] Notificaciones de nuevos mazos
[ ] PWA (offline-first)
[ ] Integración GitHub Gist (sync)
[ ] Mobile app (React Native)
```

---

## 🙏 Créditos

- **Juego:** Libellud
- **Imágenes:** store.asmodee.com
- **Framework:** Microsoft Blazor
- **Hosting:** GitHub Pages

---

## 📞 Soporte

1. Revisá `DEPLOY_GUIDE.md` (troubleshooting completo)
2. Verificá logs en Actions tab
3. Probá en navegador diferente
4. Limpiar caché: Ctrl+Shift+Delete

---

**¡Disfrutá tu colección de Dixit! 🎴✨**
