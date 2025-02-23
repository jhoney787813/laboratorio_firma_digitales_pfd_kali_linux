# 📜 Guion para Teleprónter – Aplicación del Certificado Digital en el Laboratorio

## 🏁 Comienzo – Saludo y Presentación

“¡Hola a todos! Bienvenidos a este laboratorio práctico donde aprenderemos a aplicar un certificado digital en Kali Linux para firmar y validar documentos electrónicos.

Hoy exploraremos no solo la implementación técnica, sino también su importancia en la seguridad digital, garantizando la autenticidad, integridad y no repudio de la información.

Así que si te interesa la criptografía aplicada y la protección de documentos digitales, quédate hasta el final porque vamos a hacer una demostración completa, paso a paso.”

## 📅 Introducción a los Certificados Digitales

"Antes de entrar en la práctica, respondamos una pregunta clave: ¿qué es un certificado digital y por qué es importante?

Un certificado digital es un documento electrónico emitido por una Autoridad Certificadora (CA) que asocia una clave pública con la identidad de una persona, organización o entidad.

Los certificados digitales desempeñan un papel clave en la firma digital, ya que permiten:

✅ Garantizar la autenticidad del firmante.
✅ Verificar la integridad del documento.
✅ Evitar el repudio del remitente.

Ahora, veamos cómo implementarlo en un entorno de laboratorio con Kali Linux.”

## ⚙️ Paso 1: Instalación de Herramientas en Kali Linux

"Para aplicar un certificado digital, necesitamos algunas herramientas esenciales. Si aún no las tienes instaladas, usa los siguientes comandos:

### Actualización del sistema:
```bash
sudo apt update && sudo apt upgrade -y
```

### Instalación de GPG y OpenSSL:
```bash
sudo apt install gnupg openssl -y
```

### Instalación de LibreOffice para la generación de documentos PDF:
```bash
sudo apt install libreoffice -y
```

Con esto, estamos listos para generar y aplicar nuestro certificado digital."

## 📄 Paso 2: Generación de un Certificado Digital Autofirmado

"Para generar un certificado digital autofirmado, utilizaremos OpenSSL. Ejecutemos el siguiente comando:

```bash
openssl req -x509 -newkey rsa:4096 -keyout mi_clave_privada.pem -out mi_certificado.pem -days 365
```

Este comando genera:
- Un archivo `mi_clave_privada.pem` con la clave privada.
- Un archivo `mi_certificado.pem` que es el certificado digital.

Durante la generación, se nos pedirá ingresar algunos datos, como el nombre de la organización, el país y el correo electrónico."

## 📝 Paso 3: Creación de un Documento y Conversión a PDF

"Ahora, creamos un documento de prueba y lo convertimos a PDF:

```bash
echo "Este es un documento firmado digitalmente" > documento.txt
libreoffice --convert-to pdf documento.txt
```

Esto generará un archivo `documento.pdf`, que usaremos para la firma digital."

## ✍️ Paso 4: Firma Digital del Documento con el Certificado Digital

"Para firmar el documento con nuestro certificado digital, usamos:

```bash
openssl smime -sign -in documento.pdf -out documento_firmado.pdf -signer mi_certificado.pem -inkey mi_clave_privada.pem -outform DER
```

Este comando genera `documento_firmado.pdf`, que incluye la firma digital basada en nuestro certificado."

## 🔍 Paso 5: Validación de la Firma Digital

"Para verificar que la firma es válida, usamos:

```bash
openssl smime -verify -in documento_firmado.pdf -CAfile mi_certificado.pem
```

Si la firma es válida, el sistema indicará que la autenticidad del documento está garantizada."

## 🌟 Conclusión y Reflexión Final

"Con esto, hemos aprendido a generar y aplicar un certificado digital en Kali Linux, firmar documentos y validar su autenticidad. Este proceso es fundamental para la seguridad de la información en entornos digitales.

La pregunta clave es: ¿Están las empresas garantizando la autenticidad e integridad de sus documentos? Implementar criptografía asimétrica y certificados digitales no solo es una mejor práctica, sino una necesidad en los sistemas modernos.

Si este laboratorio te fue útil, no olvides compartirlo y suscribirte para más contenido sobre ciberseguridad y criptografía. ¡Nos vemos en el próximo laboratorio!"

