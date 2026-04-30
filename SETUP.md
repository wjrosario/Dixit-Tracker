# ⚡ Setup Rápido (5 minutos)

## Paso 1️⃣: Cloná el Proyecto

```bash
git clone https://github.com/TU_USUARIO/dixit-tracker.git
cd dixit-tracker
```

## Paso 2️⃣: Instalá .NET 8

📥 Descargá desde: https://dotnet.microsoft.com/download/dotnet/8.0

**Verificá:**
```bash
dotnet --version
```

Debe mostrar: `8.0.x`

## Paso 3️⃣: Correlo Localmente

```bash
dotnet run
```

Abrí en el navegador: `https://localhost:5000`

## Paso 4️⃣: Hacé cambios (opcional)

Editá `/Services/CollectionService.cs` para agregar más mazos.

## Paso 5️⃣: Subilo a GitHub

```bash
git add .
git commit -m "Mi versión de Dixit Tracker"
git push origin main
```

## Paso 6️⃣: Habilitar GitHub Pages

1. Repo → **Settings**
2. **Pages** (menú izquierdo)
3. Source: **GitHub Actions**
4. **Save**

## Paso 7️⃣: Esperá 3 minutos

El workflow se ejecuta automáticamente. Ver en **Actions** tab.

## 🎉 ¡LISTO!

Tu sitio está en: `https://TU_USUARIO.github.io/dixit-tracker/`

---

## 🆘 Problemas?

Ver `DEPLOY_GUIDE.md` para troubleshooting detallado.
