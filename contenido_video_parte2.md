# 📜 Guion para Teleprónter – Aplicación del Certificado Digital en el Laboratorio

## 🏁 Comienzo – Saludo y Presentación

## 📚 Resumen del Laboratorio Anterior
¡Hola a todos! Bienvenidos!

En la primera parte de este laboratorio, exploramos el proceso de firma digital de documentos en Kali Linux. Aprendimos a generar claves GPG, firmar archivos PDF y verificar la autenticidad e integridad de los documentos mediante firmas digitales. También simulamos modificaciones en los documentos para entender cómo la firma digital detecta cambios no autorizados.

Hoy exploraremos no solo la implementación técnica, sino también su importancia en la seguridad digital.

Ahora, daremos un paso adelante y veremos cómo aplicar un certificado digital, que permite validar y autenticar la identidad del firmante a través de una infraestructura de clave pública (PKI). ¡Comencemos!

---

## 🎥 Introducción: Importancia del Certificado Digital

"Para garantizar la autenticidad y validez de una firma digital, necesitamos un certificado digital.

Pero, ¿qué es exactamente un certificado digital?

Es un documento electrónico emitido por una Autoridad Certificadora (CA) que vincula una clave pública con la identidad de su propietario. Con este certificado, podemos comprobar que la firma digital pertenece realmente a quien dice ser, fortaleciendo la seguridad y confianza en la comunicación digital."

---

## 🛠️ Instalación del Certificado Digital

"Para aplicar un certificado digital en Kali Linux, seguimos estos pasos:

### 1. Descargar el Certificado Digital
   - Si el certificado proviene de una Autoridad Certificadora, lo descargamos en formato `.crt` o `.pem`.
   - Si es un certificado personal generado en GPG, lo exportamos con:
     ```bash
     gpg --export -a "Tu Nombre" > certificado_publico.asc
     ```

### 2. Importar el Certificado Digital
   - Para importar un certificado GPG, usamos:
     ```bash
     gpg --import certificado_publico.asc
     ```
   - Para agregar un certificado X.509 a nuestro sistema:
     ```bash
     sudo cp certificado.crt /usr/local/share/ca-certificates/
     sudo update-ca-certificates
     ```

### 3. Verificar la Instalación del Certificado
   - Para comprobar que el certificado está instalado correctamente, usamos:
     ```bash
     gpg --list-keys
     ```
   - En sistemas con certificados X.509, verificamos con:
     ```bash
     openssl x509 -in certificado.crt -text -noout
     ```

---

## 🔒 Aplicación del Certificado Digital en la Firma Digital

"Una vez que tenemos el certificado digital instalado, podemos usarlo para firmar documentos de forma autenticada.

### 1. Firmar un Documento PDF con el Certificado Digital
   ```bash
   gpg --detach-sign --armor --output documento.pdf.sig documento.pdf
   ```

### 2. Validar la Firma con el Certificado
   ```bash
   gpg --verify documento.pdf.sig documento.pdf
   ```
   - Si el certificado es válido, el sistema mostrará la información del firmante y confirmará la integridad del documento.

### 3. Compartir el Documento y el Certificado
   - Para que otros usuarios puedan validar la firma, exportamos el certificado y lo compartimos junto con el documento firmado.
   ```bash
   gpg --export -a "Tu Nombre" > certificado_publico.asc
   ```

---

## 🔧 Prueba de Validación en Otro Equipo

"Para comprobar la autenticidad en otro equipo:

1. Importamos el certificado del firmante:
   ```bash
   gpg --import certificado_publico.asc
   ```
2. Verificamos la firma digital del documento recibido:
   ```bash
   gpg --verify documento.pdf.sig documento.pdf
   ```
   - Si el certificado es reconocido, la firma será válida.
   - Si no es reconocido, se nos advertirá sobre un certificado desconocido."

---

## 📊 Conclusión

"En este laboratorio hemos aprendido a instalar y aplicar un certificado digital para firmar y verificar documentos. La combinación de firmas digitales y certificados garantiza la seguridad, autenticidad e integridad de la información en entornos digitales modernos.

En un mundo donde la confianza digital es clave, el uso de estas tecnologías nos permite proteger documentos, autenticar usuarios y prevenir manipulaciones no autorizadas.

👌 Si te gustó este laboratorio, no olvides compartirlo y suscribirte para más contenido sobre seguridad y criptografía.

👉️ ¡Nos vemos en el próximo laboratorio!"


