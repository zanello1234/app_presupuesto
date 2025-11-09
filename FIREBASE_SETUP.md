# 🔥 Configuración de Firebase para Presupuestos

## Paso 1: Crear proyecto en Firebase

1. Ve a [Firebase Console](https://console.firebase.google.com/)
2. Click en **"Agregar proyecto"**
3. Nombre del proyecto: `presupuestos-app` (o el que prefieras)
4. Desactiva Google Analytics (opcional para este proyecto)
5. Click en **"Crear proyecto"**

## Paso 2: Configurar Firestore Database

1. En el menú lateral, click en **"Firestore Database"**
2. Click en **"Crear base de datos"**
3. Selecciona **"Modo de prueba"** (para empezar)
4. Elige la ubicación: **`southamerica-east1` (São Paulo)** para mejor latencia desde Argentina
5. Click en **"Habilitar"**

## Paso 3: Configurar reglas de seguridad

En la pestaña **"Reglas"** de Firestore, reemplaza con:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /presupuestos/{document=**} {
      allow read, write: if true;  // ⚠️ Solo para desarrollo
    }
  }
}
```

**⚠️ IMPORTANTE**: Estas reglas permiten lectura/escritura a todos. Para producción, implementa autenticación.

## Paso 4: Obtener configuración del proyecto

1. En Firebase Console, click en el ícono de **⚙️ (Configuración)**
2. Scroll down hasta **"Tus apps"**
3. Click en **`</> Web`**
4. Registra la app con un nombre (ej: "Presupuestos Web")
5. **NO** marques "Firebase Hosting"
6. Click en **"Registrar app"**

Verás un código similar a:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyC1234567890abcdefGHIJKLMNOP",
  authDomain: "tu-proyecto.firebaseapp.com",
  projectId: "tu-proyecto-id",
  storageBucket: "tu-proyecto.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456"
};
```

## Paso 5: Actualizar el archivo HTML

Abre `presupuesto.html` y busca esta sección (cerca del final del archivo):

```javascript
// Configuración de Firebase (usa tu propia configuración)
const firebaseConfig = {
    apiKey: "AIzaSyDEMO_KEY_REEMPLAZAR",  // ⚠️ REEMPLAZAR
    authDomain: "presupuesto-demo.firebaseapp.com",
    projectId: "presupuesto-demo",
    storageBucket: "presupuesto-demo.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:abcdef123456"
};
```

**Reemplaza TODO el objeto `firebaseConfig`** con el que copiaste de Firebase Console.

## Paso 6: Probar la conexión

1. Abre `presupuesto.html` en tu navegador
2. Abre la Consola del desarrollador (F12)
3. Deberías ver: `🔥 Firebase inicializado correctamente`
4. Crea un presupuesto y haz click en **"💾 Guardar"**
5. Si funciona, verás: `✅ Presupuesto guardado en la nube`

## Paso 7: Verificar en Firebase

1. Ve a Firebase Console → Firestore Database
2. Deberías ver una colección llamada **`presupuestos`**
3. Dentro verás tus presupuestos guardados con todos los datos

## 🎉 Beneficios

- ✅ **Sincronización automática** entre dispositivos
- ✅ **Backup en la nube** - nunca pierdas tus datos
- ✅ **Acceso desde cualquier lugar** con internet
- ✅ **Gratis** hasta 1GB de datos y 50k lecturas/día
- ✅ **Fallback automático** a localStorage si Firebase falla

## 🔒 Seguridad para Producción (Opcional)

Para uso profesional, implementa Firebase Authentication:

1. Habilita **Email/Password** en Authentication
2. Actualiza las reglas de Firestore:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /presupuestos/{presupuestoId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## 📊 Iconos en la lista de presupuestos

- **☁️** = Guardado en Firebase (nube)
- **💾** = Guardado en localStorage (local)

## ⚠️ Notas Importantes

- La API Key de Firebase **puede ser pública** (está diseñada para frontend)
- La seguridad se maneja con las **Reglas de Firestore**
- Para producción, **siempre** usa autenticación
- Los datos en localStorage son solo backup local
