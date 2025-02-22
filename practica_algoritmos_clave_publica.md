# Algoritmos que se pueden emplear para firmas digitales

Para validar certificados digitales, se emplean algoritmos de clave pública como RSA y ECDSA, complementando junto con funciones hash como SHA-256 para garantizar la integridad.

##  🔐 1. Validación con RSA

**RSA (Rivest-Shamir-Adleman)** es un algoritmo de cifrado asimétrico ampliamente utilizado para firmar y validar certificados digitales. Funciona con un par de claves:

Clave privada (para firmar)

Clave pública (para verificar)

### 📌 Ejemplo práctico: Crear y validar una firma con RSA en Kali Linux

#### Paso 1: Generar un par de claves RSA

Ejecuta el siguiente comando para generar una clave RSA de 2048 bits:
```bash
  openssl genpkey -algorithm RSA -out privada.pem -pkeyopt rsa_keygen_bits:2048
  openssl rsa -in privada.pem -pubout -out publica.pem
```
.  **privada.pem:** clave privada usada para firmar.

.  **publica.pem:** clave pública usada para verificar la firma.


#### Paso 2: Crear un mensaje de prueba
Generamos un archivo de texto que simularemos como un documento a firmar:

```bash
    echo "Este es un documento es la prueba aplicada" > documento.txt
```

#### Paso 3: Firmar el documento con RSA
  Usamos la clave privada para generar una firma digital del archivo:
  
```bash
    openssl dgst -sha256 -sign privada.pem -out firma.bin documento.txt
```

La salida del comando genera un archivo firma.bin, que es la firma del documento.

#### Paso 4: Verificar la firma
  Para comprobar si la firma es válida, usamos la clave pública:
  
```bash
    openssl dgst -sha256 -verify publica.pem -signature firma.bin documento.txt
```

Si la salida anterior es exitosa saldra:

```bash
    Verified OK
```
Si el documento fue modificado la **verificación fallara!**




## 🏎 2. Validación con ECDSA (Elliptic Curve Digital Signature Algorithm)

ECDSA es una variante de firma digital basada en criptografía de curvas elípticas. Es más eficiente que RSA porque ofrece la misma seguridad con claves más pequeñas.

### 📌 Ejemplo práctico: Crear y validar una firma con ECDSA

#### Paso 1: Generar un par de claves ECDSA

Generamos una clave privada con curva elíptica **secp256k1**:

```bash
  openssl ecparam -name secp256k1 -genkey -noout -out ecdsa_priv.pem
  openssl ec -in ecdsa_priv.pem -pubout -out ecdsa_pub.pem
```

. **ecdsa_priv.pem:** clave privada

. **ecdsa_pub.pem:** clave pública



#### Paso 2: Firmar el documento con ECDSA

```bash
  openssl dgst -sha256 -sign ecdsa_priv.pem -out firma_ecdsa.bin documento.txt
```

La salida del comando anterior es un archivo, con la firma digital **firma_ecdsa.bin**.

#### Paso 3: Verificar la firma

```bash
  openssl dgst -sha256 -verify ecdsa_pub.pem -signature firma_ecdsa.bin documento.txt
```

Si la salida anterior es exitosa saldra:

```bash
    Verified OK
```
Si el documento fue modificado la **verificación fallara!**

## 🔎 3. Validación con SHA-256 (Función Hash)

*SHA-256* es una función de hash criptográfico utilizada para garantizar la integridad del contenido de un certificado. No cifra ni firma, sino que genera un resumen único del contenido.

### 📌 Ejemplo práctico: Crear y validar un hash SHA-256

#### Paso 1: Generar el hash de un documento

```bash
  sha256sum documento.txt > hash_original.txt
```

#### Paso 2: Verificar si el documento fue alterado

Si alguien modifica documento.txt, generamos el nuevo hash y lo comparamos con el original:

```bash
  sha256sum documento.txt
  cat hash_original.txt
```
**Nota:** Si los hashes son diferentes, significa que el documento ha sido alterado.





## 🚀 Consideraciones Importantes

**RSA** y **ECDSA** se usan para firmar y verificar certificados digitales.
**SHA-256** permite garantizar la integridad del documento.
Estos algoritmos combinados permiten validar certificados de manera segura.


##### Espero estos cortos ejemplos ¡Te ayuden a comprender cómo funciona la validación de certificados digitales en la práctica! 🚀

---
[Jhon E -> GitHub Profile](https://github.com/jhoney787813/)
