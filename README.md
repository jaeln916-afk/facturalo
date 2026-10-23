# Facturalo: guía completa de publicación y cobros

Facturalo es un generador de facturas online con tres planes (Gratis, Pro y Business). Es una web estática: no necesita servidor y se publica gratis en GitHub Pages. Esta guía te lleva de cero a cobrar, en orden.

**Archivos del proyecto**
- `index.html`: la app completa.
- `config.js`: el único archivo que debes editar (enlaces de pago y contacto).
- `robots.txt` y `sitemap.xml`: ayudan a que Google la encuentre.

---

## Parte 1. Publicar la web en GitHub

**Paso 1. Crea tu cuenta.** Entra a github.com y regístrate con tu correo.

**Paso 2. Crea el repositorio.** Pulsa el botón verde **New**. En *Repository name* escribe `facturalo`, marca **Public** y pulsa **Create repository**.

**Paso 3. Sube los archivos.** En la página del repositorio pulsa **uploading an existing file**. Descomprime el zip en tu computadora, arrastra los 5 archivos a la ventana y pulsa **Commit changes**.

**Paso 4. Activa GitHub Pages.** Ve a **Settings → Pages**. En *Source* elige **Deploy from a branch**. En *Branch* selecciona `main` y la carpeta `/ (root)`, y pulsa **Save**.

**Paso 5. Abre tu web.** Tras uno o dos minutos aparecerá arriba tu dirección: `https://TU-USUARIO.github.io/facturalo/`. Cópiala, la necesitarás en la Parte 2.

**Paso 6. Ajusta tu dirección en los archivos.** En GitHub abre `index.html`, pulsa el lápiz (*Edit*), busca `TU-USUARIO.github.io/TU-REPO` (aparece dos veces) y cámbialo por tu dirección real. Haz lo mismo en `robots.txt` y `sitemap.xml`. Guarda cada cambio con **Commit changes**.

---

## Parte 2. Configurar los cobros con Stripe

**Paso 7. Crea tu cuenta de Stripe.** Regístrate en stripe.com. Mientras pruebas, deja activado el interruptor **Test mode** (modo de prueba): así no se mueve dinero real.

**Paso 8. Crea los dos productos.** Ve a **Product catalog → Add product**.
- Producto 1: nombre *Facturalo Pro*, precio **7 USD**, tipo **Recurrente**, cada **mes**.
- Producto 2: nombre *Facturalo Business*, precio **15 USD**, tipo **Recurrente**, cada **mes**.

**Paso 9. Crea un enlace de pago por producto.** Con el producto abierto pulsa **Create payment link**. En la sección *After payment* elige **Don't show confirmation page** y pega esta dirección de retorno:
- Pro: `https://TU-USUARIO.github.io/facturalo/?plan=pro&session_id={CHECKOUT_SESSION_ID}`
- Business: `https://TU-USUARIO.github.io/facturalo/?plan=business&session_id={CHECKOUT_SESSION_ID}`

Deja `{CHECKOUT_SESSION_ID}` tal cual, Stripe lo reemplaza solo. Pulsa **Create link** y copia la dirección que empieza con `https://buy.stripe.com/`.

**Paso 10. Pega los enlaces en tu web.** En GitHub abre `config.js`, pulsa el lápiz y pega cada enlace entre las comillas de `pro` y `business`. Pulsa **Commit changes**.

**Paso 11. Activa el portal de clientes.** En Stripe ve a **Settings → Billing → Customer portal**, actívalo, copia su enlace y pégalo en `PORTAL_LINK` de `config.js`. Así tus clientes cancelan o cambian de plan sin escribirte. Opcional: pon tu correo en `SUPPORT_EMAIL`.

**Paso 12. Prueba una compra falsa.** Abre tu web, pulsa **Elegir Pro** y paga con la tarjeta `4242 4242 4242 4242`, cualquier fecha futura y cualquier CVC. Al terminar volverás a tu web con el plan Pro activo. Repite con Business.

**Paso 13. Pasa a modo real.** Cuando todo funcione, completa la verificación de tu cuenta en Stripe, apaga **Test mode**, repite los pasos 8, 9 y 11 en modo real y reemplaza los enlaces en `config.js`.

---

## Parte 3. Que Google te encuentre

**Paso 14.** Entra a search.google.com/search-console, añade tu dirección y envía `sitemap.xml`. Con un dominio propio (por ejemplo `facturalo.com`, que puedes conectar en *Settings → Pages → Custom domain*) posicionarás mucho mejor que con `github.io`.

---

## Cómo funciona el guardado
Los datos de la factura, tu negocio y tu plan se guardan automáticamente en el navegador de cada persona y reaparecen al volver. Si borra los datos del navegador o cambia de dispositivo, no estarán. El botón **Nueva factura** limpia al cliente y sube el número, pero conserva los datos de tu negocio.

## Límites de esta versión
Como no hay servidor, el plan también se guarda en el navegador. Alguien con conocimientos técnicos puede activarlo sin pagar, y si un cliente cancela, su plan no se desactiva solo. Para cobros blindados necesitas cuentas de usuario y un webhook de Stripe (por ejemplo con Supabase). Es el siguiente paso cuando tengas clientes.

## Antes de cobrar
Publica política de privacidad, términos y reembolsos (Stripe suele pedirlos) y confirma con un contador las obligaciones fiscales de tu país.
