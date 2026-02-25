# Resumen de Cambios - Migración a Multi-Tenant

## Archivos Modificados
- ✅ `index.html`
- ✅ `admin.html`
- ✅ `tv.html`

---

## Cambios Realizados por Archivo

### 1. **index.html**

#### Cambio 1: Firebase Config
- **Antes:** Proyecto `rocola-crowdplay2`
- **Después:** Proyecto `crowdplayapp-641b0`
- **Línea:** ~2184

Nuevo config:
```javascript
const manualFirebaseConfig = {
    apiKey: "AIzaSyBZLKKiAPihhPFIiNYXVxu70pYdBEso7Xo",
    authDomain: "crowdplayapp-641b0.firebaseapp.com",
    projectId: "crowdplayapp-641b0",
    storageBucket: "crowdplayapp-641b0.firebasestorage.app",
    messagingSenderId: "710678103774",
    appId: "1:710678103774:web:b0fd20fffe4c19c67aa44f"
};
```

#### Cambio 2: App ID desde parámetro URL
- **Antes:** `let appId = typeof __app_id !== 'undefined' ? __app_id : 'rocola-crowdplay2-v1';`
- **Después:** Lee de parámetro `venue` en la URL, con fallback a `'demo'`
- **Línea:** ~2200

```javascript
const _urlVenue = new URLSearchParams(window.location.search).get('venue');
let appId = _urlVenue || (typeof __app_id !== 'undefined' ? __app_id : 'demo');
```

#### Cambio 3: Rutas de Firestore
- **Antes:** `artifacts/${appId}/public/data`
- **Después:** `venues/${appId}/data`
- **Ocurrencias:** Todas las referencias a Firestore actualizadas

---

### 2. **admin.html**

#### Cambio 1: Firebase Config
- **Igual que index.html**
- **Línea:** ~2554

#### Cambio 2: App ID desde parámetro URL
- **Igual que index.html**
- **Línea:** ~2569

#### Cambio 3: Rutas de Firestore
- **Antes:** `artifacts/${appId}/public/data`
- **Después:** `venues/${appId}/data`
- **Ocurrencias:** Todas las referencias actualizadas

---

### 3. **tv.html**

#### Cambio 1: Firebase Config
- **Igual que los anteriores** (mismo proyecto `crowdplayapp-641b0`)
- **Línea:** ~278
- **Variable:** `firebaseConfig` (no `manualFirebaseConfig`)

#### Cambio 2: Venue ID desde parámetro URL
- **Antes:** `const firestoreAppId = 'rocola-crowdplay2-v1';`
- **Después:** Lee de parámetro `venue` en la URL, con fallback a `'demo'`
- **Línea:** ~292

```javascript
const firestoreAppId = new URLSearchParams(window.location.search).get('venue') || 'demo';
```

#### Cambio 3: Rutas de Firestore
- **Antes:** `artifacts/${firestoreAppId}/public/data`
- **Después:** `venues/${firestoreAppId}/data`
- **Ocurrencias:** Todas actualizadas

---

## Estructura de Firestore

La aplicación ahora espera la siguiente estructura:

```
venues/
├── <venue-id>/
│   └── data/
│       ├── rocola_queue/
│       ├── client_profiles/
│       ├── staff_profiles/
│       ├── waiter_calls/
│       ├── active_tables/
│       ├── chat_messages/
│       └── banners/

superadmin/
├── (colecciones de administración)
```

---

## Cómo Usar

### Para acceder a un venue específico:

```
https://tudominio.com/index.html?venue=venue-123
https://tudominio.com/admin.html?venue=venue-456
https://tudominio.com/tv.html?venue=venue-789
```

### Sin parámetro (modo demo):
- Se usará `appId = 'demo'` o `firestoreAppId = 'demo'`

---

## Firestore Rules

Las reglas de Firestore deben ser actualizadas en Firebase Console. Ver archivo `FIRESTORE_RULES.md` para detalles.

**Resumen:**
- ✅ Lectura pública para `venues/{venueId}/**`
- ✅ Escritura solo para usuarios autenticados en `venues/{venueId}/**`
- ✅ Acceso completo (lectura/escritura) para `superadmin/**` si está autenticado

---

## Próximos Pasos

1. ✅ Actualizar código en los 3 archivos HTML
2. ⏳ **Próximo:** Actualizar Firestore Rules en Firebase Console
3. ⏳ **Próximo:** Crear estructura de datos en Firestore para cada venue
4. ⏳ **Próximo:** Probar con parámetros `?venue=test-venue-1`

---

**Fecha:** 25 de febrero de 2026
**Proyecto:** CrowdPlayApp - Multi-Tenant
**Firebase Project:** crowdplayapp-641b0
