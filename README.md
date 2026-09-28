# Práctica: Wi-Fi Segura 🔐

Informe de auditoría de una red Wi-Fi insegura — práctica del módulo de seguridad en redes.

---

## 1. Introducción

En esta práctica analicé qué pasa cuando visito un sitio web que usa **HTTP** (sin la "S" de seguro) y qué información queda a la vista de cualquiera que esté conectado a la misma red Wi-Fi que yo.

La idea es entender por qué conectarse a una Wi-Fi pública (de un bar, un aeropuerto o un shopping) puede ser riesgoso, y qué podemos hacer para protegernos.

---

## 2. Sitio analizado

- **Sitio:** http://neverssl.com
- **Herramienta usada:** herramientas de desarrollador del navegador (tecla F12), pestaña **Network (Red)**.

### Pasos que seguí

1. Entré a `http://neverssl.com`.
2. Abrí las herramientas de desarrollador con **F12**.
3. Fui a la pestaña **Network**.
4. Recargué la página.
5. Hice clic en la primera solicitud de la lista para ver sus detalles.

---

## 3. Evidencia observada

Estos son los datos que pude ver en la primera solicitud:

| Dato | Lo que observé |
|------|----------------|
| **URL solicitada** | `http://neverssl.com/` |
| **Método HTTP** | `GET` |
| **Host** | `neverssl.com` |
| **Protocolo** | `HTTP/1.1` (sin cifrar) |
| **User-Agent** | Mi navegador y sistema operativo (por ejemplo: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) ... Chrome/...`) |

### Headers enviados (algunos ejemplos)

- `Host: neverssl.com`
- `User-Agent:` información de mi navegador y sistema operativo
- `Accept:` qué tipo de contenido acepta mi navegador
- `Accept-Language:` el idioma de mi navegador (por ejemplo `es-AR`)
- `Connection: keep-alive`

### ¿Qué protocolo utiliza el sitio?

El sitio usa **HTTP**, no HTTPS. Me di cuenta porque:

- La dirección empieza con `http://` y no con `https://`.
- El navegador muestra el aviso de **"No seguro"** al lado de la dirección.
- En la pestaña Network el protocolo figura como `HTTP/1.1`.

Esto significa que la información viaja **sin cifrar**, o sea, "en texto plano".

### ¿Qué información puede observarse durante la solicitud?

Pude ver al menos estos elementos, y cualquiera en la misma red podría verlos también:

1. **El Host** → a qué sitio me estoy conectando.
2. **La URL completa** → qué página específica estoy visitando.
3. **El método GET** → que estoy pidiendo ver una página.
4. **El User-Agent** → qué navegador y sistema operativo uso.
5. **Los headers** → datos como mi idioma y otras características de mi navegador.

---

## 4. Riesgos encontrados

Como aprendí en el curso, **HTTP es como mandar una postal**: cualquiera que la agarre en el camino puede leer lo que dice. En una Wi-Fi pública, estoy compartiendo la red con muchas personas que no conozco.

Estos son los riesgos que encontré:

- **Intercepción de datos (Sniffing):** alguien conectado a la misma red puede usar programas como **Wireshark** para "escuchar" lo que mi computadora manda y ver las páginas que visito.
- **Robo de datos personales:** si en un sitio HTTP escribo un usuario, una contraseña o completo un formulario, esos datos viajan sin protección y alguien podría leerlos.
- **Robo de sesión:** un atacante podría capturar las *cookies* de una sesión abierta y hacerse pasar por mí en ese sitio.
- **Ataque de intermediario (Man-in-the-Middle):** el atacante se hace pasar por el router y toda mi información pasa primero por su computadora. Podría leerla o incluso modificarla.
- **Redes gemelas malignas (Evil Twin):** puedo conectarme a una red con un nombre que parece del lugar (por ejemplo "Cafe_Gratis"), pero que en realidad fue creada por un atacante.

**Algo que me sorprendió:** que una Wi-Fi tenga contraseña no la hace segura si esa contraseña se la dan a todos los clientes. Todos tienen la misma "llave".

---

## 5. Cómo ayuda una VPN

Una **VPN** (Red Privada Virtual) funciona como un **túnel blindado** que se arma dentro de la red pública.

Así cambiaría el escenario usando una VPN:

- **Cifrado:** antes de salir de mi dispositivo, toda la información se "codifica" para que no se pueda leer. Si alguien la captura, solo ve datos sin sentido (ruido cifrado).
- **Túnel seguro:** la VPN crea un **túnel** entre mi dispositivo y el servidor de la VPN. Mis datos viajan por adentro de ese túnel, protegidos de los demás usuarios de la Wi-Fi.
- **Encapsulamiento:** la VPN "envuelve" mis paquetes de datos dentro de otros paquetes cifrados. Es como meter la postal adentro de una caja fuerte antes de mandarla.
- **Protección del tráfico:** aunque alguien en el bar use Wireshark, no va a poder ver qué sitios visito ni qué datos envío. Solo va a ver que estoy conectada a un servidor VPN.
- **Privacidad:** el sitio que visito ve la dirección IP del servidor VPN y no la mía, y la red pública no puede ver mi actividad.

> **Aclaración:** la VPN protege el camino entre mi dispositivo y el servidor de la VPN. Desde ahí hasta el sitio web, si el sitio usa HTTP, la información vuelve a viajar sin cifrar. Por eso lo ideal es combinar **VPN + sitios HTTPS**.

---

## 6. Mis 3 Reglas de Oro para navegar en Wi-Fi públicas 🏆

1. **Solo entrar a sitios con HTTPS (candado 🔒).** Si el navegador dice "No seguro", no escribo contraseñas, datos personales ni de tarjetas.

2. **Usar una VPN confiable siempre que me conecte a una Wi-Fi pública.** Así mi tráfico viaja cifrado dentro de un túnel y nadie en la red puede leerlo.

3. **Desconfiar de la red y confirmar su nombre con el lugar.** Antes de conectarme pregunto al personal el nombre exacto de la red, desactivo la conexión automática a redes abiertas y evito hacer cosas sensibles (como entrar al home banking) desde una red pública.

---

## Conclusión

Con esta práctica entendí que cuando un sitio usa HTTP, mucha información queda a la vista de cualquiera que esté en la misma red. En una Wi-Fi pública ese riesgo es mayor porque no sé quién más está conectado. Usar HTTPS, una VPN y un poco de sentido común hace una gran diferencia para proteger mi privacidad.
