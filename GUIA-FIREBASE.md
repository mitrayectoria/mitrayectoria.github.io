# Guía: poner Mi Trayectoria en línea con Firebase

La página ya está preparada para usar Firebase, que es un servicio gratuito de Google. Faltan unos pasos que tenés que hacer vos con tu cuenta de Google, porque Firebase no deja crear el proyecto en nombre de otra persona. Lleva unos 20 minutos.

Mientras falten estos pasos, la página muestra el aviso: *"La página todavía no está conectada a la base de datos"*.

---

## 1. Crear el proyecto

1. Entrá a https://console.firebase.google.com con tu cuenta de Google.
2. Tocá **Crear un proyecto** (o **Agregar proyecto**).
3. Nombre: `mi-trayectoria` (o el que quieras).
4. Cuando pregunte por **Google Analytics**, desactivalo. No hace falta.
5. Esperá a que termine y tocá **Continuar**.

## 2. Registrar la página web y copiar los datos

1. En la pantalla principal del proyecto, tocá el ícono **`</>`** (Web).
2. Apodo de la app: `mi-trayectoria-web`. **No** marques "Firebase Hosting".
3. Tocá **Registrar app**.
4. Aparece un bloque de código con algo así:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "mi-trayectoria-xxxx.firebaseapp.com",
     projectId: "mi-trayectoria-xxxx",
     storageBucket: "mi-trayectoria-xxxx.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abcdef"
   };
   ```

5. Copiá esos seis valores. Hay dos opciones:
   - **Mandámelos a mí** y yo los pego en `index.html`, o
   - Pegalos vos en `index.html`, cerca del principio del código, reemplazando cada `"PEGAR_AQUI"`.

> Estos datos **no son secretos**: cualquier página con Firebase los muestra. Lo que protege la información son las reglas del paso 5.

## 3. Activar las formas de entrar

1. Menú de la izquierda → **Compilación** → **Authentication** → **Comenzar**.
2. Pestaña **Método de acceso** → **Google** → activar → elegí tu email como "correo de asistencia" → **Guardar**.
3. En la misma pestaña, **Agregar proveedor nuevo** → **Correo electrónico/contraseña** → activá solo el primer interruptor (**Correo electrónico/contraseña**; el de "vínculo de correo electrónico" dejalo apagado) → **Guardar**.
4. Opcional: en la pestaña **Plantillas**, podés revisar los mails que reciben las personas (confirmar correo y cambiar contraseña). La página los pide en español.

## 4. Crear la base de datos

1. Menú de la izquierda → **Compilación** → **Firestore Database** → **Crear base de datos**.
2. Ubicación: **`southamerica-east1` (São Paulo)**, la más cercana a Argentina. *Esto no se puede cambiar después.*
3. Elegí **Comenzar en modo de producción** → **Crear**.

## 5. Pegar las reglas de seguridad

1. En Firestore Database, abrí la pestaña **Reglas**.
2. Borrá todo lo que hay.
3. Copiá y pegá el contenido completo del archivo [`firestore.rules`](firestore.rules) de este repositorio.
4. Tocá **Publicar**.

Las reglas hacen esto:
- Cualquiera puede **ver** los perfiles, sin cuenta.
- Para **publicar** hay que entrar con Google, y cada persona solo puede crear, editar o borrar **su propio** perfil.
- Se rechazan datos raros: textos demasiado largos, fotos muy pesadas o campos inventados.
- Solo la administradora puede quitar perfiles de otras personas.

## 6. Publicar la página en internet (GitHub Pages)

> Ya está hecho: la página está publicada en **https://mitrayectoria.github.io/** desde el repositorio `mitrayectoria/mitrayectoria.github.io`. Estos pasos quedan como referencia.

1. En GitHub, entrá al repositorio → **Settings** → **Pages**.
2. En **Source** elegí **Deploy from a branch**.
3. Branch: **main**, carpeta **/ (root)** → **Save**.
4. En uno o dos minutos aparece la dirección, algo como:
   `https://mitrayectoria.github.io/` (porque el repositorio se llama `mitrayectoria.github.io`)

> En la cuenta gratuita de GitHub, Pages solo funciona si el repositorio es **público**. Si es privado, hay que hacerlo público o usar otro hosting, como Netlify.

## 7. Autorizar esa dirección en Firebase

Sin este paso, el botón "Entrar con Google" da error.

1. Firebase → **Authentication** → **Settings** (Configuración) → **Dominios autorizados**.
2. **Agregar dominio** → escribí solo `mitrayectoria.github.io` (sin `https://` ni barras).

## 8. Hacerte administradora

Esto te permite quitar perfiles de otras personas, por ejemplo uno inapropiado.

1. Abrí la página publicada y entrá con **tu** cuenta de Google. No hace falta publicar perfil.
2. En Firebase → **Authentication** → **Users**, copiá tu **UID** de usuario (una cadena larga de letras y números).
3. Firebase → **Firestore Database** → **Datos** → **Iniciar colección**.
   - ID de la colección: `admins`
   - ID del documento: pegá tu UID
   - Agregá un campo cualquiera, por ejemplo `rol` = `admin` → **Guardar**.
4. Recargá la página. En los perfiles de otras personas vas a ver el botón **"Quitar este perfil"**.

## 9. Probar

- Abrí la página en el celular, entrá con Google, publicá un perfil de prueba y fijate que aparezca en el listado.
- Abrila en otro celular o en una ventana de incógnito y fijate que el perfil se vea sin entrar.
- Borrá el perfil de prueba.

---

## Cosas a tener en cuenta

- **Costo**: el plan gratuito (Spark) de Firebase alcanza para miles de perfiles y visitas por día. No hace falta cargar tarjeta.
- **WhatsApp, Instagram y Facebook**: si alguien abre el link desde adentro de esas apps, Google a veces bloquea el ingreso. La página avisa que la abran en Chrome.
- **Privacidad**: la página publica nombre, localidad y foto. Antes de difundirla conviene sumar un texto corto de privacidad que explique qué datos se guardan y cómo pedir que se borren (Ley 25.326 de Protección de Datos Personales).

---

## Uso diario (administradora)

Cómo funciona la página:

- Para ver perfiles hay que **entrar con una cuenta**: Google, o cualquier correo con contraseña. Las cuentas de correo tienen que confirmar el mail antes de poder ver o publicar nada.
- Cada cuenta elige una opción: **busco trabajo** (publica su perfil) o **busco a alguien para mi casa** (carga un perfil corto de su familia).
- Las familias tocan **Pedir una entrevista** en un perfil y pueden dejar un mensaje. La persona ve el pedido la próxima vez que entra, junto con los datos de la familia, y elige **Aceptar la entrevista** o **No, gracias**. Si acepta, cada una ve el WhatsApp de la otra.
- Cada persona que busca trabajo elige cómo la contactan: **WhatsApp directo** (las familias registradas ven su número y le escriben) o **pedido de entrevista** (su número solo lo ve la familia cuyo pedido acepta). El WhatsApp nunca aparece escrito en la ficha.

Entrando con tu cuenta, ves la sección **Administración**:

- **Pedidos sin responder:** la página no le puede mandar avisos a la persona. Si un pedido lleva días sin respuesta, usá **Avisarle por WhatsApp**: abre un mensaje ya escrito.
- **Estadísticas:** totales (perfiles, familias, contactos por WhatsApp, pedidos, porcentaje de aceptación y tiempo de respuesta), actividad de las últimas 4 semanas, y oferta y demanda por localidad, tipo de trabajo y modalidad. La actividad por semana se cuenta desde que empezó el registro de acciones.
- **Familias registradas:** podés escribirles, **bloquearlas** (pueden ver, pero no mandar pedidos ni cambiar sus datos) o **quitarlas**.
- **Cuentas bloqueadas:** todas las cuentas que bloqueaste, aunque ya no tengan perfil, con el botón **Desbloquear**.
- **Quitar perfil vs. Bloquear:** "Quitar" borra la ficha, pero la persona puede volver a armarla. "Bloquear" le impide publicar y contactar, pero deja la ficha. Para un perfil falso u ofensivo, usá las dos: primero Bloquear y después Quitar.
- **Borrar una cuenta completa** (si alguien lo pide): quitá su perfil desde la página y después, en Firebase → **Authentication** → **Users**, buscá su correo y, en los tres puntitos, tocá **Borrar cuenta**.
- En los **perfiles** también tenés **Quitar perfil** y **Bloquear**.

> Cada vez que se cambia `firestore.rules` en el repositorio, hay que volver a pegarlo en Firebase (Firestore Database → Reglas → **Publicar**).
