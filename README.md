# 🎴 Dixit Tracker

Tracker de colección de mazos **Dixit** construido con **Blazor WebAssembly (.NET 8)**.

Marcá los mazos que tenés, guardá tu progreso automáticamente en `localStorage` y compartí tu colección con un link único.

---

## ✨ Funcionalidades

- ✅ **15 mazos oficiales**: bases, expansiones y ediciones especiales
- 🖼️ **Imagen real** de la caja de cada mazo
- 💾 **Guardado automático** en `localStorage` — persiste entre visitas
- 🔗 **Botón Compartir** → genera un link único con tu colección (modo solo lectura)
- 📊 **Estadísticas en vivo**: cuántos tenés / cuántos te faltan / barra de progreso
- 📱 **Responsive**: funciona perfectamente en celular y escritorio
- 🚀 **Deploy estático** en GitHub Pages (sin servidor necesario)

---

## 🛠️ Tech Stack

| Componente | Tecnología |
|-----------|-----------|
| **Framework** | Blazor WebAssembly (.NET 8) |
| **Estilos** | CSS3 puro (sin Bootstrap/Tailwind) |
| **Persistencia** | `localStorage` via JSInterop |
| **Hosting** | GitHub Pages (estático) |
| **CI/CD** | GitHub Actions |

---

## 🚀 Publicar en GitHub Pages en 5 minutos

### Paso 1: Crear repositorio en GitHub

1. Ve a [github.com/new](https://github.com/new)
2. Nombre: `dixit-tracker`
3. Marcá **Public**
4. Click en "Create repository"

### Paso 2: Clonar y subir el código

```bash
# Cloná el repo que creaste
git clone https://github.com/TU_USUARIO/dixit-tracker.git
cd dixit-tracker

# Copiá todos los archivos del proyecto aquí
# (los 13 archivos .cs, .razor, .yml, .html, .css, etc.)

# Subí a GitHub
git add .
git commit -m "Initial commit: Dixit tracker"
git push origin main
```

### Paso 3: Habilitar GitHub Pages

1. Entrá al repo en GitHub
2. **Settings → Pages**
3. En **Source**, seleccioná **GitHub Actions**
4. Click en **Save**

### Paso 4: Ver el deploy

1. Pestaña **Actions** en tu repo
2. El workflow `Deploy Dixit Tracker to GitHub Pages` se ejecuta automáticamente
3. En ~2-3 minutos ves: ✅ (verde)

¡Tu sitio está LIVE en: `https://TU_USUARIO.github.io/dixit-tracker/`

---

## 💻 Desarrollo Local

### Requisitos

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)

### Correr localmente

```bash
dotnet run
```

Abrí `https://localhost:5001` en el navegador.

### Publicar manualmente

```bash
dotnet publish -c Release -o release
# Los archivos estáticos quedan en: release/wwwroot/
```

---

## 📁 Estructura del Proyecto

```
dixit-tracker/
├── .github/
│   └── workflows/
│       └── deploy.yml              # ⚡ CI/CD automático a GitHub Pages
│
├── Models/
│   └── DixitGame.cs                # 📊 Modelo: Id, Name, Artist, etc.
│
├── Services/
│   └── CollectionService.cs        # 💾 Lógica + localStorage + share token
│
├── Pages/
│   └── Index.razor                 # 🎨 UI completa (markup + C#)
│
├── wwwroot/
│   ├── index.html                  # 📄 Host page
│   ├── 404.html                    # 🔀 SPA routing para GitHub Pages
│   └── css/
│       └── app.css                 # 🎨 Estilos (~400 líneas)
│
├── Program.cs                      # 🚀 Entry point
├── App.razor                       # 📱 Router root component
├── MainLayout.razor                # 📦 Layout wrapper
├── _Imports.razor                  # 📚 Global usings
├── dixit-tracker.csproj            # ⚙️ Configuración .NET
├── .gitignore                      # 🚫 Archivos ignorados
└── README.md                       # 📖 Este archivo
```

---

## 🔗 Cómo Compartir tu Colección

### Para ti:

1. Marcá los mazos que tenés (hacé click en las tarjetas)
2. Click en el botón **Compartir** (arriba a la derecha)
3. ¡Se copia el link al portapapeles!

### El link que se genera:

```
https://tu-usuario.github.io/dixit-tracker/base,quest,harmonies,mirrors
```

El query string contiene los IDs de los mazos que marcaste.

### Quién recibe el link:

- Ve tu colección en **modo solo lectura** (no puede editar)
- Ve el aviso: "👀 Estás viendo la colección de alguien más"
- Botón para crear la propia

---

## ➕ Agregar un Mazo Nuevo

Editá `Services/CollectionService.cs` y agregá un item a `AllGames`:

```csharp
new DixitGame {
    Id = "mi-mazo",
    Name = "Dixit: Mi Mazo 2025",
    Artist = "Nombre del Artista",
    Year = "2025",
    Type = GameType.Expansion,        // Base | Expansion | Special
    FallbackEmoji = "🎨",
    ImageUrl = "https://url-de-imagen.jpg",
    Description = "Descripción breve del mazo."
},
```

**Luego:**
1. Hacé push a `main`
2. El CI compila y deploya automáticamente en ~2 min
3. ¡Listo!

---

## 🐛 Troubleshooting

### "Veo 404 cuando entro a GitHub Pages"

✅ **Solución:**
- Verificá en **Settings → Pages** que esté en **GitHub Actions** (no "Deploy from a branch")
- Esperá 3-5 min a que el primer workflow termine (pestaña Actions)
- Recargá con Ctrl+Shift+R (cache buster)

### "No veo mis imágenes de los mazos"

✅ **Solución:**
- Las URLs de imágenes vienen de `store.asmodee.com` — a veces están lentas
- Abrí la consola (F12) y buscá errores CORS
- Las imágenes tienen fallbacks (emojis) si algo falla

### "El localStorage no persiste"

✅ **Solución:**
- ¿Navegador en modo incógnito/privado? Necesita modo normal
- Verificá que `localStorage` no esté bloqueado en tu navegador
- Borrá cookies y volvé a entrar

---

## 📝 Notas sobre el .NET 8 / Blazor WASM

### Por qué Blazor WASM:

- ✅ Compila a **archivos estáticos** → perfecto para GitHub Pages
- ✅ **Sin servidor backend** necesario
- ✅ Completa en **~2 MB** (gzipped ~500 KB)
- ✅ Rápida después del primer load
- ✅ Acceso nativo a DOM via `JSInterop`

### Cómo funciona el routing en GitHub Pages:

1. Alguien accede a `/dixit-tracker/base,quest,mirrors`
2. GitHub Pages no encuentra esa ruta → sirve `404.html`
3. `404.html` redirige a `index.html` preservando la URL
4. Blazor carga y el componente `Index.razor` parsea el `@parameter ShareToken`
5. Se restaura la colección del URL

---

## 🎯 Próximas Mejoras Opcionales

- [ ] Exportar colección como JSON / CSV
- [ ] Modo dark theme
- [ ] Buscar/filtrar mazos
- [ ] Notificaciones de nuevos mazos lanzados
- [ ] PWA (funciona sin internet después del primer load)
- [ ] Sincronizar colección vía GitHub Gist

---

## 📄 Licencia

MIT — libre para usar, modificar, distribuir y comercializar.

---

## 🙏 Créditos

- **Imágenes:** Libellud / Asmodee (store.asmodee.com)
- **Juego original:** Libellud
- **Construcción:** Blazor WebAssembly + GitHub Pages

¡Disfrutá tu colección de Dixit! 🎴✨
