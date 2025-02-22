📜 Guion para Teleprónter – Firma Digital de Documentos en Kali Linux

(🏁 Comienzo – Saludo y Presentación)
"¡Hola a todos! Bienvenidos a este laboratorio práctico donde aprenderemos cómo firmar digitalmente un documento PDF utilizando Kali Linux.

Hoy vamos a explorar no solo el proceso técnico, sino también la importancia de la firma digital en términos de seguridad, autenticidad e integridad de la información.

Así que si te interesa la criptografía aplicada y la protección de documentos digitales, quédate hasta el final porque vamos a hacer una demostración completa, paso a paso."

(📌 Introducción a la Firma Digital)
"Antes de entrar en la práctica, respondamos una pregunta clave: ¿qué es y para qué sirve una firma digital?

Cuando hablamos de firmar digitalmente un documento, nos referimos a aplicar un mecanismo criptográfico que garantiza tres aspectos esenciales:

✅ Autenticidad: Confirma quién firmó el documento.
✅ Integridad: Asegura que el contenido del documento no ha sido alterado desde su firma.
✅ No repudio: Evita que el firmante pueda negar haber firmado el documento.

Para lograr esto, utilizaremos GPG (GNU Privacy Guard), una herramienta de cifrado basada en estándares de clave pública y privada."

(⚙️ Configuración e Instalación de Herramientas)
"Para esta práctica, necesitamos instalar algunas dependencias esenciales en nuestro sistema Kali Linux.

Si aún no las tienes, puedes instalarlas con estos comandos:

👉 Primero, actualizamos la lista de paquetes:
sudo apt update && sudo apt upgrade -y

👉 Luego, instalamos GPG, que nos permitirá firmar los documentos:
sudo apt install gnupg -y

👉 También instalaremos LibreOffice para generar documentos en PDF, aunque si no está disponible en los repositorios, podemos usar Flatpak o descargarlo manualmente.

Y con eso, ya estamos listos para comenzar la práctica."

(🛠️ Paso 1: Generación de Claves GPG)
"Para firmar un documento, necesitamos una clave privada y su correspondiente clave pública.

Ejecutamos el siguiente comando para generar un par de claves:
gpg --full-generate-key

Seguimos las instrucciones en pantalla, elegimos el tipo de cifrado, el tamaño de la clave y configuramos nuestros datos.

Una vez generada la clave, podemos listar nuestras claves disponibles con:
gpg --list-keys

Esto nos permitirá confirmar que nuestra clave está lista para usarse."

(📄 Paso 2: Crear un Documento y Convertirlo a PDF)
"Ahora vamos a crear un documento de prueba. Usaremos un archivo de texto simple y lo convertiremos a PDF con LibreOffice.

Primero, creamos el archivo de texto:
echo "Este es un documento de prueba" > documento.txt

Luego, lo convertimos a PDF con el siguiente comando:
libreoffice --convert-to pdf documento.txt

Esto generará un archivo documento.pdf que utilizaremos para la firma."

(✍️ Paso 3: Firmar Digitalmente el Documento PDF)
"Con nuestro documento listo, ahora vamos a firmarlo digitalmente usando nuestra clave privada.

Ejecutamos el siguiente comando:
gpg --detach-sign --armor --output documento.pdf.sig documento.pdf

Esto creará un archivo .sig, que contiene la firma digital del documento."

(🔍 Paso 4: Verificación de la Firma Digital)
"Para comprobar si la firma es válida, podemos usar el siguiente comando:
gpg --verify documento.pdf.sig documento.pdf

Si la firma es correcta, el sistema nos dirá que la firma pertenece a nuestro usuario y que el documento está intacto."

(🚨 Paso 5: Simulación de Alteración del Documento)
"Ahora hagamos una prueba interesante: ¿qué pasa si el documento es modificado después de la firma?

Editamos el documento y agregamos una línea adicional:
echo "Texto alterado" >> documento.txt

Luego, lo convertimos nuevamente a PDF:
libreoffice --convert-to pdf documento.txt --outdir .

Si intentamos verificar la firma con el archivo modificado, ejecutamos nuevamente:
gpg --verify documento.pdf.sig documento.pdf

Y veremos que el sistema detectará que la firma no es válida, indicando que el documento ha sido alterado."

(📤 Paso 6: Exportar la Clave Pública para Verificación en Otro Equipo)
"Si queremos compartir nuestro documento firmado con alguien más, esa persona necesitará nuestra clave pública para verificar la firma.

Podemos exportarla con este comando:
gpg --export -a "Tu Nombre" > clave_publica.asc

Luego, el destinatario puede importar la clave en su sistema con:
gpg --import clave_publica.asc

Así podrán verificar que el documento realmente fue firmado por nosotros."

(🎯 Conclusión y Cierre)

"Con esto hemos aprendido cómo firmar digitalmente documentos en Kali Linux, cómo verificar la autenticidad de una firma, y cómo detectar si un documento ha sido alterado.

Recuerden que las firmas digitales son una herramienta clave en la seguridad informática, especialmente en entornos donde la autenticidad y la integridad de los documentos son críticas.

Si les gustó este laboratorio, no olviden darle like, compartirlo y suscribirse para más contenido sobre criptografía y seguridad digital.

¡Nos vemos en el próximo laboratorio!"

