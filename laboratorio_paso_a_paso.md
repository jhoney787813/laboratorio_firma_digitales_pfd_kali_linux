# 🔬 Laboratorio: Firma Digital de un PDF en Kali Linux
Este laboratorio demostrará cómo firmar un documento PDF usando herramientas disponibles en Kali Linux, como gpg y openssl.

## **Universidad Politécnico Grancolombiano**  
**Especialización en Seguridad de la Información**  
**Materia:** Criptografía Asimétrica  
**Docente:** José Alfonso Valencia Rodríguez  
**Autor:** Jhon Edison Hincapié  
**Año:** 2025  

## Introducción

En la era digital, la autenticidad y la integridad de los documentos son aspectos fundamentales para garantizar la seguridad de la información. La firma digital es una técnica criptográfica que permite validar la autenticidad de un documento y asegurar que no haya sido alterado. En este documento, se presentará un laboratorio práctico para firmar documentos PDF en Kali Linux, además de una explicación detallada sobre los certificados digitales y su validación.




## 🛠 Requisitos previos

Antes de comenzar, asegúrate de que tienes instalado:

✅ Kali Linux (o cualquier otra distribución basada en Debian)

✅ GnuPG (GPG) para la gestión de claves

✅ LibreOffice (opcional) para la creación del PDF

✅ OpenSSL para verificación de firmas digitales.



## Procedimiento

### Paso 1: Generación de Claves GPG
Ejecutar el siguiente comando para generar un par de claves:
```bash
gpg --full-generate-key
```
Seleccionar el tipo de clave, el tamaño (recomendado 4096 bits) y configurar los datos de usuario.

### Paso 2: Creación del Documento PDF
Para generar un PDF desde un archivo de texto:
```bash
echo "Este es un documento de prueba" > documento.txt
libreoffice --convert-to pdf documento.txt
```

### Paso 3: Firma Digital del Documento PDF
Para firmar el documento con GPG:
```bash
gpg --detach-sign --armor --output documento.pdf.sig documento.pdf
```
Esto genera un archivo de firma digital `documento.pdf.sig`.

### Paso 4: Verificación de la Firma
Para comprobar la autenticidad del documento firmado:
```bash
gpg --verify documento.pdf.sig documento.pdf
```
Si la firma es válida, se confirmará que el documento no ha sido alterado.

### Paso 5: Simulación de Alteración del Documento
Para comprobar qué sucede si el documento es modificado después de la firma, edita el archivo de la siguiente manera:
```bash
echo "Texto alterado" >> documento.txt
libreoffice --convert-to pdf documento.txt --outdir .
```
Luego, intenta verificar la firma nuevamente:
```bash
gpg --verify documento.pdf.sig documento.pdf
```
Si el documento ha sido alterado, GPG mostrará un mensaje de error indicando que la firma no es válida.

### Paso 6: Exportar la Clave Pública (Opcional, para Verificación en Otro Equipo)
Si deseas verificar la firma en otro equipo, es necesario exportar la clave pública y compartirla:
```bash
gpg --export -a "Tu Nombre" > clave_publica.asc
```
En el otro equipo, se puede importar la clave pública con:
```bash
gpg --import clave_publica.asc
```
Esto permitirá verificar la firma en otro dispositivo de manera segura.


# **Aplicación del Certificado Digital en el Laboratorio**

En el laboratorio, usamos **GPG (GNU Privacy Guard)** para generar claves criptográficas y firmar documentos digitalmente. Sin embargo, si queremos llevar la seguridad un paso más allá y hacer que la firma digital sea verificable de manera confiable por terceros, necesitamos aplicar un **certificado digital** emitido por una **Autoridad Certificadora (CA)**.

## **📌 Pasos para Aplicar un Certificado Digital en el Laboratorio**

### **1️⃣ Generar un Certificado Digital con OpenSSL**
Si no tienes un certificado emitido por una CA oficial, puedes crear un **certificado autofirmado** para probar el proceso:

```bash
openssl req -x509 -newkey rsa:4096 -keyout mi_clave_privada.pem -out mi_certificado.pem -days 365
```

### **Explicación:**
✔️ `-x509`: Genera un certificado X.509 autofirmado.  
✔️ `-newkey rsa:4096`: Crea una nueva clave RSA de 4096 bits.  
✔️ `-keyout mi_clave_privada.pem`: Guarda la clave privada en un archivo.  
✔️ `-out mi_certificado.pem`: Guarda el certificado en formato PEM.  
✔️ `-days 365`: Define la validez del certificado por 1 año.  

Una vez generado, este **certificado digital** se puede usar para firmar documentos y validar la identidad del firmante.

---

### **2️⃣ Usar el Certificado para Firmar Digitalmente un Documento PDF**
Si en lugar de GPG queremos aplicar una firma con un certificado digital, podemos utilizar `openssl`:

```bash
openssl smime -sign -in documento.pdf -binary -outform DER -out documento_firmado.p7s -signer mi_certificado.pem -inkey mi_clave_privada.pem
```

### **Explicación:**
✔️ `-sign`: Indica que queremos firmar el documento.  
✔️ `-in documento.pdf`: Archivo de entrada a firmar.  
✔️ `-binary`: Mantiene el formato binario del archivo.  
✔️ `-outform DER`: Usa el formato DER para la firma.  
✔️ `-out documento_firmado.p7s`: Guarda la firma en un archivo `.p7s`.  
✔️ `-signer mi_certificado.pem`: Usa el certificado para firmar.  
✔️ `-inkey mi_clave_privada.pem`: Especifica la clave privada para firmar.  

Este método genera un archivo **documento_firmado.p7s**, que contiene la firma digital basada en el certificado.

---

### **3️⃣ Validar la Firma Digital con el Certificado**
Una vez firmado el documento, cualquier persona con acceso a nuestro **certificado público** puede verificar la firma:

```bash
openssl smime -verify -in documento_firmado.p7s -inform DER -content documento.pdf -CAfile mi_certificado.pem
```

### **Explicación:**
✔️ `-verify`: Indica que queremos verificar la firma.  
✔️ `-in documento_firmado.p7s`: Especifica el archivo con la firma.  
✔️ `-inform DER`: Define el formato DER para la firma.  
✔️ `-content documento.pdf`: Archivo original para comparar.  
✔️ `-CAfile mi_certificado.pem`: Certificado usado para validar la firma.  

Si la firma es válida, el sistema mostrará un mensaje confirmando la autenticidad del documento.

---

### **4️⃣ Distribuir la Clave Pública para Validación**
Para que otros usuarios puedan verificar nuestras firmas digitales, debemos compartir nuestro **certificado público**, sin revelar la clave privada:

```bash
openssl x509 -in mi_certificado.pem -out mi_certificado_publico.pem
```

Los destinatarios pueden importar este certificado en sus sistemas y verificar la autenticidad de nuestros documentos firmados.


### **🔍 Conclusión de Certificado Digital con OpenSSL**

✅ **Diferencia con GPG:** Mientras que GPG usa un modelo de **confianza basada en claves públicas y privadas**, los certificados digitales permiten que una **tercera entidad confiable (CA)** valide nuestra identidad.  
✅ **Aplicación en el laboratorio:** Se usa para firmar documentos con mayor seguridad y permitir la verificación externa sin necesidad de importar claves manualmente.  
✅ **Escalabilidad:** Este enfoque es utilizado en entornos corporativos, firmas electrónicas de contratos y comunicaciones seguras en internet (HTTPS, correos cifrados, etc.).  

Con esto, llevamos nuestro laboratorio a un nivel más avanzado, aplicando criptografía asimétrica con certificados digitales. 🚀



## 📚 Preguntas y Respuestas

### 📌 ¿Qué garantiza el uso de firmar un documento?

La firma digital garantiza autenticidad, integridad y no repudio del documento.

✔ **Autenticidad:** Confirma la identidad del remitente.

✔ **Integridad:** Asegura que el documento no ha sido alterado después de la firma.

✔ **No repudio:** El remitente no puede negar haber firmado el documento.


### 📌 ¿Qué función realiza el certificado digital? ¿Cómo se relaciona con la firma digital?

Un certificado digital asocia una clave pública con la identidad del usuario y es fundamental para validar la autenticidad de la firma digital.

🔹 Un certificado digital es un archivo que asocia una clave pública con la identidad de una persona o entidad.

🔹 La firma digital usa este certificado para garantizar la autenticidad y confiabilidad de la firma.

🔹 La firma digital es generada con la clave privada, mientras que el certificado digital contiene la clave pública para la verificación.


### 📌 ¿Qué proceso permite validar el uso de certificados y firmas digitales?

La validación se realiza mediante la verificación de la firma digital y la autenticidad del certificado digital utilizando la cadena de confianza.

*El proceso de validación de certificados y firmas digitales implica:*

Verificar la firma digital con la clave pública del firmante.

Confirmar que el certificado digital del firmante es válido y no ha expirado.

Asegurar que el certificado proviene de una autoridad de certificación confiable (CA).

### 📌 ¿Qué permite validar los certificados digitales?

Se validan verificando la firma de la autoridad certificadora, la fecha de expiración y listas de revocación.

🔹 La validación de certificados se realiza mediante la cadena de confianza, que incluye:

✔ Verificación de la firma de la Autoridad de Certificación (CA).

✔ Comprobación de la fecha de expiración del certificado.

✔ Consulta en una Lista de Revocación de Certificados (CRL) o mediante el protocolo OCSP.

### 📌 ¿Qué algoritmos se usan para validar los certificados digitales?

🔹 Los algoritmos más comunes son:


✔ **RSA (Rivest-Shamir-Adleman):** RSA es un algoritmo de cifrado asimétrico ampliamente utilizado para firmas digitales y seguridad en comunicaciones. Fue desarrollado en 1977 por Ron Rivest, Adi Shamir y Leonard Adleman, y se basa en la dificultad de factorizar números primos grandes, lo que lo hace altamente seguro.

##### 🎯 ¿Dónde se usa RSA?

📜 Firmas Digitales y Certificados Electrónicos
🔑 Autenticación en Sistemas y Dispositivos
🔐 Cifrado de Datos y Comunicaciones Seguras

RSA es un pilar en la seguridad informática moderna. Su uso en firmas digitales, cifrado de datos, autenticación y blockchain garantiza privacidad y confianza en sistemas críticos. Sin embargo, es más lento que otros métodos modernos como ECC (Elliptic Curve Cryptography), por lo que en algunos casos se prefiere ECDSA. 🚀




✔ **ECDSA (Elliptic Curve Digital Signature Algorithm):** ECDSA es una variante de firma digital basada en criptografía de curvas elípticas. Es más eficiente que RSA porque ofrece la misma seguridad con claves más pequeñas.

##### 🎯 ¿Dónde se usa ECDSA?

🔑 Bitcoin y criptomonedas: Para firmar transacciones.

🔐 TLS/SSL (HTTPS): Para conexiones seguras en navegadores web.

📜 SSH: Para autenticación en servidores.

📱 Dispositivos IoT: Gracias a su eficiencia en consumo de recursos.

 **ECDSA** es preferido en sistemas modernos por su seguridad con claves más cortas y su eficiencia en comparación con RSA. 🚀


✔ **SHA-256, SHA-384, SHA-512 (para la función hash):** permiten garantizar la integridad del documento.

##### 🎯 ¿Dónde se usan los SHA-?

🔐 Almacenamiento Seguro de Contraseñas

🔑 Verificación de Integridad de Archivos

📜  Protocolos de Seguridad (TLS, VPN, SSH)

Las funciones **SHA-256, SHA-384 y SHA-512** son fundamentales en seguridad informática. Se usan en firmas digitales, almacenamiento de contraseñas, blockchain, verificación de archivos y protocolos de seguridad. Son herramientas esenciales para garantizar la integridad y autenticidad de los datos en la era digital. 🚀


[🔎 Ver-> Explicación, Practica para cada algoritmo](https://github.com/jhoney787813/laboratorio_firma_digitales_pfd_kali_linux/blob/main/practica_algoritmos_clave_publica.md)




## Conclusión
El uso de firmas digitales y certificados garantiza la seguridad de los documentos electrónicos, permitiendo verificar su autenticidad y proteger la información contra modificaciones no autorizadas.


## Referencias

1. **National Institute of Standards and Technology.** (2023). Digital Signature Standard (DSS). Recuperado de [NIST](https://www.nist.gov)
2. **Zimmermann, P.** (1995). *The Official PGP User's Guide*. MIT Press.
3. **OpenSSL Foundation.** (2024). OpenSSL Cryptography and SSL/TLS Toolkit. Recuperado de [OpenSSL](https://www.openssl.org)

---
[Jhon E -> GitHub Profile](https://github.com/jhoney787813/)
