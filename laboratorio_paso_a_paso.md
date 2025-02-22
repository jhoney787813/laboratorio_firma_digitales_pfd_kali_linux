🔬 Laboratorio: Firma Digital de un PDF en Kali Linux
Este laboratorio demostrará cómo firmar un documento PDF usando herramientas disponibles en Kali Linux, como gpg y openssl.

🛠 Requisitos previos
Antes de comenzar, asegúrate de que tienes instalado:
✅ Kali Linux (o cualquier otra distribución basada en Debian)
✅ GnuPG (GPG) para la gestión de claves
✅ LibreOffice (opcional) para la creación del PDF



📚 Preguntas y Respuestas

### 📌 ¿Qué garantiza el uso de firmar un documento?

Firmar digitalmente un documento garantiza:

✔ **Autenticidad:** Confirma la identidad del remitente.

✔ **Integridad:** Asegura que el documento no ha sido alterado después de la firma.

✔ **No repudio:** El remitente no puede negar haber firmado el documento.


### 📌 ¿Qué función realiza el certificado digital? ¿Cómo se relaciona con la firma digital?

🔹 Un certificado digital es un archivo que asocia una clave pública con la identidad de una persona o entidad.

🔹 La firma digital usa este certificado para garantizar la autenticidad y confiabilidad de la firma.

🔹 La firma digital es generada con la clave privada, mientras que el certificado digital contiene la clave pública para la verificación.


### 📌 ¿Qué proceso permite validar el uso de certificados y firmas digitales?

El proceso de validación de certificados y firmas digitales implica:

Verificar la firma digital con la clave pública del firmante.

Confirmar que el certificado digital del firmante es válido y no ha expirado.

Asegurar que el certificado proviene de una autoridad de certificación confiable (CA).

### 📌 ¿Qué permite validar los certificados digitales?

✔ La validación de certificados se realiza mediante la cadena de confianza, que incluye:

Verificación de la firma de la Autoridad de Certificación (CA).
Comprobación de la fecha de expiración del certificado.

Consulta en una Lista de Revocación de Certificados (CRL) o mediante el protocolo OCSP.

### 📌 ¿Qué algoritmos se usan para validar los certificados digitales?

🔹 Los algoritmos más comunes son:

✔ RSA (Rivest-Shamir-Adleman)

✔ ECDSA (Elliptic Curve Digital Signature Algorithm)

✔ SHA-256, SHA-384, SHA-512 (para la función hash)
