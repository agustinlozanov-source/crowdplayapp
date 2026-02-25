# Instrucciones Finales - Migración Multi-Tenant Completada ✅

## Cambios Implementados

### 1. **Código de Aplicaciones (COMPLETADO)**

Se han actualizado los 3 archivos principales:
- ✅ `index.html` - Cliente
- ✅ `admin.html` - Panel de administración
- ✅ `tv.html` - Pantalla de TV

**Cambios:**
1. Firebase Config actualizado al proyecto `crowdplayapp-641b0`
2. App ID ahora se lee desde parámetro URL `?venue=venue-id`
3. Rutas de Firestore cambiadas de `artifacts/${appId}/public/data` a `venues/${appId}/data`

---

## PRÓXIMOS PASOS EN FIREBASE CONSOLE

### 2. **Firestore Rules (PENDIENTE)**

**Acción:** Actualizar las reglas de seguridad de Firestore

**Pasos:**
1. Abre [Firebase Console](https://console.firebase.google.com)
2. Selecciona proyecto **crowdplayapp-641b0**
3. Navega a **Firestore Database** → Tab **Rules**
4. Reemplaza todo con esto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /venues/{venueId}/{document=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /superadmin/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

5. Haz clic en **Publish**

---

## 3. **Estructura de Firestore (INFORMACIÓN)**

La aplicación espera esta estructura en Firestore:

```
venues/
├── venue-demo/
│   └── data/
│       ├── rocola_queue/          (Canciones en cola)
│       ├── client_profiles/       (Perfiles de clientes)
│       ├── staff_profiles/        (Perfiles de personal)
│       ├── waiter_calls/          (Llamadas de mesero)
│       ├── active_tables/         (Mesas activas)
│       ├── chat_messages/         (Mensajes de chat)
│       └── banners/               (Banners publicitarios)
│
├── venue-restaurant-xyz/
│   └── data/ ...
│
└── venue-bar-abc/
    └── data/ ...

superadmin/
└── (Colecciones de administración)
```

**Nota:** Las colecciones se crean automáticamente cuando la app escribe datos.

---

## 4. **Cómo Usar la Aplicación**

### URLs de Acceso:

```
# Cliente
https://tudominio.com/index.html?venue=restaurant-xyz

# Admin/DJ
https://tudominio.com/admin.html?venue=restaurant-xyz

# Pantalla TV
https://tudominio.com/tv.html?venue=restaurant-xyz

# Modo Demo (sin venue)
https://tudominio.com/index.html?venue=demo
```

### Parámetro URL:
- `?venue=venue-id` → Especifica qué venue usar
- Si no se proporciona → Usa fallback `'demo'`

---

## 5. **Testing**

### Verificar que todo funciona:

1. **Test Acceso a Cliente:**
   ```
   https://tudominio.com/index.html?venue=test-venue-1
   ```
   - Debe cargar la interfaz de cliente
   - Debe conectar a Firestore

2. **Test Acceso a Admin:**
   ```
   https://tudominio.com/admin.html?venue=test-venue-1
   ```
   - Debe mostrar login
   - Después autenticación, debe acceder a `venues/test-venue-1/data`

3. **Test Pantalla TV:**
   ```
   https://tudominio.com/tv.html?venue=test-venue-1
   ```
   - Debe cargar la pantalla de TV fullscreen
   - Debe reproducir videos de la cola

---

## 6. **Archivos de Documentación**

Se han creado dos archivos:
- 📄 `CHANGELOG.md` - Detalles de todos los cambios
- 📄 `FIRESTORE_RULES.md` - Instrucciones de Firestore Rules

---

## ✅ Checklist Final

- [x] Firebase config actualizado en 3 archivos
- [x] App ID dinámico desde parámetro URL
- [x] Rutas de Firestore migradas a estructura multi-tenant
- [ ] Firestore Rules publicadas en Firebase Console
- [ ] Probado acceso a múltiples venues
- [ ] Datos de venues creados en Firestore

---

## Problemas Comunes

### Error: "Permission denied" en Firestore
**Solución:** Verifica que Firestore Rules estén publicadas correctamente

### Error: "venues/undefined/data"
**Solución:** Asegúrate de pasar `?venue=nombre` en la URL

### No carga datos de Firestore
**Solución:** Verifica que:
1. Firestore Rules permiten lectura
2. El venue existe en Firestore
3. Los datos están en `venues/{venueId}/data`

---

**Migración completada:** 25 de febrero de 2026
**Proyecto Firebase:** crowdplayapp-641b0
**Estado:** Listo para publicar (después de Firestore Rules)
