# Firestore Rules para crowdplayapp-641b0

En Firebase Console del proyecto `crowdplayapp-641b0`, ve a **Firestore → Rules** y reemplaza el contenido con lo siguiente:

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

## Pasos:
1. Abre Firebase Console en https://console.firebase.google.com
2. Selecciona el proyecto **crowdplayapp-641b0**
3. Ve a **Firestore Database** → **Rules** (en la pestaña superior)
4. Reemplaza todo el contenido con el código anterior
5. Haz clic en **Publicar**

## Estructura de Firestore esperada:
```
venues/
├── venue-id-1/
│   └── data/
│       ├── rocola_queue/
│       ├── client_profiles/
│       ├── staff_profiles/
│       ├── waiter_calls/
│       ├── active_tables/
│       ├── chat_messages/
│       └── banners/
├── venue-id-2/
│   └── data/ ...
superadmin/
└── (colecciones de administración)
```
