# 🏗️ Arquitectura del Proyecto

## Diagrama de Flujo

```
┌─────────────────────────────────────────────────────────────┐
│                   USUARIO EN NAVEGADOR                       │
│                 https://usuario.github.io/...                │
└────────────────┬────────────────────────────────────────────┘
                 │
                 ▼
         ┌──────────────────┐
         │ GitHub Pages     │
         │ (Hosting)        │
         └────────┬─────────┘
                  │
         ┌────────▼────────────────────────────────┐
         │ wwwroot/index.html                      │
         │ └─ Blazor WebAssembly SPA               │
         └─────────┬──────────────────────────────┘
                   │
         ┌─────────▼──────────────────────────────┐
         │ _framework/ (DLL, WASM, JS)            │
         │ └─ Runtime de Blazor WASM              │
         └─────────┬──────────────────────────────┘
                   │
         ┌─────────▼──────────────────────────────┐
         │ C# .NET Código (compilado a WASM)      │
         │ ├─ Pages/Index.razor                   │
         │ ├─ Services/CollectionService.cs       │
         │ └─ Models/DixitGame.cs                 │
         └─────────┬──────────────────────────────┘
                   │
         ┌─────────▼──────────────────────────────┐
         │ browser localStorage                   │
         │ └─ Guardado de colección               │
         └────────────────────────────────────────┘
```

---

## Capas de la Aplicación

### 1️⃣ **Presentación** (UI)
- **Archivo:** `Pages/Index.razor` (198 líneas)
- **Componentes:**
  - Header con logo y botón Compartir
  - Stats: Total, Tengo, Me faltan, Progreso
  - Grid de tarjetas (games)
  - Footer con botón Reset
- **Interactividad:** Click en tarjetas → toggle `owned`

### 2️⃣ **Lógica de Negocio**
- **Archivo:** `Services/CollectionService.cs` (170 líneas)
- **Responsabilidades:**
  - Listar todos los mazos (`AllGames`)
  - Gestionar estado de "poseídos" (HashSet)
  - Persistencia: `localStorage.getItem/setItem`
  - Generador de share token (URL)
  - Parseo de share token (URL → colección)

### 3️⃣ **Modelos de Datos**
- **Archivo:** `Models/DixitGame.cs` (15 líneas)
- **Propiedades:**
  - `Id`: "base", "quest", "mirrors", etc.
  - `Name`: "Dixit", "Dixit: Quest", etc.
  - `Artist`: Nombre del ilustrador
  - `Year`: Año de lanzamiento
  - `Type`: Base | Expansion | Special
  - `ImageUrl`: URL de la caja (store.asmodee.com)
  - `FallbackEmoji`: Emoji si la imagen falla
  - `Description`: Descripción breve

### 4️⃣ **Estilos (CSS)**
- **Archivo:** `wwwroot/css/app.css` (383 líneas)
- **Sistema:**
  - CSS Variables (`--clr-*`, `--rad-*`, `--shadow-*`)
  - Mobile-first responsive
  - Dark mode ready (structure)
  - Transiciones suaves
  - Accesibilidad básica

---

## Flujo de Datos

### Inicialización

```
App.razor
  ↓
Program.cs (DI setup)
  ↓
Index.razor (OnInitializedAsync)
  ↓
CollectionService.InitAsync()
  ↓
localStorage.getItem('dixit_collection_v3')
  ↓
Renderizar Interface
```

### Marcar/Desmarcar Mazo

```
Usuario hace click en tarjeta
  ↓
Index.razor: ToggleGame(id)
  ↓
CollectionService.ToggleAsync(id)
  ↓
HashSet.Add() o .Remove()
  ↓
PersistAsync() → localStorage.setItem()
  ↓
StateHasChanged() → re-render
```

### Compartir Colección

```
Usuario hace click en "Compartir"
  ↓
ShareCollection()
  ↓
CollectionService.GetShareToken()
  ↓
Array.join(",") → "base,quest,mirrors"
  ↓
Generar URL: {baseUrl}/{token}
  ↓
navigator.clipboard.writeText(url)
  ↓
Toast: "✅ Copiado!"
```

### Ver Colección Compartida

```
URL: https://usuario.github.io/dixit-tracker/base,quest,mirrors
  ↓
Index.razor: @parameter ShareToken
  ↓
OnInitializedAsync() detecta ShareToken
  ↓
_isShareView = true
  ↓
CollectionService.LoadFromTokenAsync()
  ↓
"base,quest,mirrors".split(",")
  ↓
CargarHashSet
  ↓
Renderizar con modo "readonly"
```

---

## Almacenamiento

### localStorage

**Clave:** `dixit_collection_v3`
**Valor:** JSON array de IDs
**Tamaño:** ~50 bytes típico

```json
["base", "quest", "harmonies", "mirrors"]
```

**Ventajas:**
- Persiste entre sesiones
- No requiere servidor
- Rápido de leer/escribir
- Sincrónico

**Limitaciones:**
- Máximo ~5-10 MB por dominio
- No compartible entre tabs automáticamente
- Se borra si limpias datos de navegador

---

## Estructura de Archivos

```
dixit-tracker/
│
├── 📂 .github/workflows/
│   └── deploy.yml                      # CI/CD: Compilar + Publicar
│
├── 📂 Models/
│   └── DixitGame.cs                    # Modelo de datos
│
├── 📂 Pages/
│   └── Index.razor                     # Componente principal
│
├── 📂 Services/
│   └── CollectionService.cs            # Lógica de negocio
│
├── 📂 Properties/
│   └── launchSettings.json             # Config de ejecución local
│
├── 📂 wwwroot/
│   ├── index.html                      # Host page
│   ├── 404.html                        # SPA routing (GH Pages)
│   ├── web.config                      # Config para IIS (opcional)
│   └── css/
│       └── app.css                     # Estilos
│
├── App.razor                           # Componente raíz
├── MainLayout.razor                    # Layout
├── Program.cs                          # Entry point
├── _Imports.razor                      # Global usings
│
├── dixit-tracker.csproj                # Configuración .NET
├── .gitignore                          # Git ignoring
│
├── README.md                           # Documentación principal
├── SETUP.md                            # Setup rápido
├── DEPLOY_GUIDE.md                     # Troubleshooting
└── ARCHITECTURE.md                     # Este archivo
```

---

## Dependencias

### Runtime
- **Microsoft.AspNetCore.Components.WebAssembly** 8.0.0
- **Microsoft.AspNetCore.Components.WebAssembly.DevServer** 8.0.0
- **Microsoft.JSInterop** 8.0.0

### Herramientas
- **.NET SDK** 8.0.x
- **GitHub Pages** (hosting)
- **GitHub Actions** (CI/CD)

### Sin dependencias de:
- ❌ Bootstrap, Tailwind, Material UI
- ❌ jQuery, React, Vue
- ❌ Librerías externas de persistencia
- ❌ Base de datos o servidor

---

## Performance

### Tamaños

| Recurso | Tamaño |
|---------|--------|
| `app.css` | ~12 KB |
| `_framework/` (WASM runtime) | ~1.5 MB |
| `*.wasm` compiled C# | ~500 KB |
| **Total (gzipped)** | ~500 KB |
| **Primer load** | ~1-2 seg (depende de conexión) |
| **Subsequent loads** | ~100 ms (caché) |

### Optimization

- ✅ CSS puro (sin build step)
- ✅ Imágenes externas (CDN store.asmodee.com)
- ✅ Fallbacks (emojis si fallan imágenes)
- ✅ Lazy loading (images con `loading="lazy"`)
- ✅ localStorage (no necesita fetch cada vez)

---

## Escalabilidad Futura

### Fácil de agregar:

**Nuevos mazos:**
```csharp
// Services/CollectionService.cs → AllGames
new DixitGame { ... }
```

**Nuevos campos:**
```csharp
// Models/DixitGame.cs
public string Rarity { get; set; }
```

**Nuevo filtro/búsqueda:**
```csharp
// Services/CollectionService.cs
public IReadOnlyList<DixitGame> FilterByType(GameType type) { ... }

// Pages/Index.razor
var filtered = Collection.FilterByType(type);
```

### Más complicado (requiere cambios arquitectónicos):

- ❌ Backend (BDD, API REST)
- ❌ Autenticación (sin servidor, imposible)
- ❌ Sincronización en la nube
- ❌ Multi-usuario

Para eso, necesitarías Blazor Server o ASP.NET Core Backend.

---

## Testing

### Componente: `CollectionService`

```csharp
[TestClass]
public class CollectionServiceTests
{
    [TestMethod]
    public void ToggleAsync_AddsToOwned() { }
    
    [TestMethod]
    public void GetShareToken_ReturnsIds() { }
    
    [TestMethod]
    public void LoadFromToken_ParsesCorrectly() { }
}
```

### UI: `Index.razor`

**Manual tests:**
- Click en tarjetas → estado cambia ✓
- Refresh → persiste ✓
- Compartir → copia URL ✓
- URL con token → carga colección ✓

---

## Seguridad

### ✅ Seguro:
- Token es solo IDs públicos (no secreto)
- localStorage es local (no enviado a servidor)
- Sin input del usuario que se guarde (solo IDs predefinidos)
- HTTPS por defecto (GitHub Pages)

### ⚠️ Consideraciones:
- Cualquiera con el URL puede ver la colección (intención)
- localStorage se borra con Clear Data del navegador
- No encriptado (datos públicos)

---

## Deployment Pipeline

```
GitHub (main branch push)
  ↓
GitHub Actions Workflow Start
  ↓
1. Checkout código
  ↓
2. Setup .NET 8
  ↓
3. dotnet restore
  ↓
4. dotnet publish -c Release
  ↓
5. Fix SPA routing (.nojekyll, 404.html)
  ↓
6. Upload artifact
  ↓
7. Deploy a GitHub Pages
  ↓
✅ Live en 2-3 minutos
```

---

¡Listo! Ahora comprendés toda la arquitectura. 🎯
