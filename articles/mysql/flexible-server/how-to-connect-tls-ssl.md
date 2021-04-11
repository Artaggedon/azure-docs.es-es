---
title: 'Conectividad cifrada mediante TLS/SSL en Azure Database for MySQL: servidor flexible'
description: 'Instrucciones e información sobre cómo conectarse mediante TLS/SSL en Azure Database for MySQL: servidor flexible.'
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: ce6150cf404f1ca68c93285a2f4a29a6373a55c0
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105110031"
---
# <a name="connect-to-azure-database-for-mysql---flexible-server-over-tls12ssl"></a>Conexión al servidor flexible de Azure Database for MySQL a través de TLS 1.2 o SSL

> [!IMPORTANT]
> Azure Database for MySQL: servidor flexible está actualmente en versión preliminar pública.

Azure Database for MySQL: servidor flexible admite la conexión de las aplicaciones cliente al servicio MySQL mediante la Seguridad de la capa de transporte (TLS), que anteriormente se denominaba Capa de sockets seguros (SSL). TLS es un protocolo estándar del sector que garantiza conexiones de red cifradas entre el servidor de bases de datos y las aplicaciones cliente, lo que le permite ajustarse a los requisitos de cumplimiento.

Azure Database for MySQL: servidor flexible solo admite conexiones cifradas mediante la Seguridad de la capa de transporte (TLS 1.2); se denegarán todas las conexiones entrantes con TLS 1.0 y TLS 1.1. Para todos los servidores flexibles, la aplicación de las conexiones TLS está habilitado y no se puede deshabilitar TLS/SSL para conectarse a un servidor flexible.

## <a name="download-the-public-ssl-certificate"></a>Descarga del certificado SSL público
Para usar con sus aplicaciones, descargue el [certificado SSL público](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem).

Guarde el archivo de certificado en la ubicación que prefiera. Por ejemplo, en este tutorial se usa `c:\ssl` o `\var\www\html\bin` en el entorno local o en el entorno cliente donde se hospeda la aplicación. Esto permitirá que las aplicaciones se conecten de forma segura a la base de datos a través de SSL. 

### <a name="connect-using-mysql-command-line-client-with-tlsssl"></a>Conexión mediante el cliente de línea de comandos mysql con TLS/SSL

Si ha creado el servidor flexible con *acceso privado (integración con VNet)* , deberá conectarse a su servidor desde un recurso en misma red virtual que el servidor. Puede crear una máquina virtual y agregarla a la red virtual creada con el servidor flexible.

Si ha creado el servidor flexible con *acceso público (direcciones IP permitidas)* , puede agregar la dirección IP local a la lista de reglas de firewall del servidor.

Puede elegir [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) o [MySQL Workbench](./connect-workbench.md) para conectarse al servidor desde su entorno local. 

En el ejemplo siguiente se muestra cómo conectarse al servidor mediante la interfaz de la línea de comandos mysql. Use la configuración de la cadena de conexión `--ssl-mode=REQUIRED` para aplicar la comprobación del certificado TLS/SSL. Pase la ruta de acceso al archivo del certificado local al parámetro `--ssl-ca`. Reemplace los valores por el nombre real del servidor y la contraseña. 

```bash
 mysql.exe -h mydemoserver.mysql.database.azure.com -u myadmin -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\DigiCertGlobalRootCA.crt.pem
```
> [!Note]
> Confirme que el valor pasado a `--ssl-ca` coincide con la ruta de acceso al archivo del certificado que guardó.

### <a name="verify-the-tlsssl-connection"></a>Comprobación de la conexión TLS/SSL

Ejecute el comando **status** de MySQL para comprobar que se ha conectado al servidor de MySQL mediante TLS/SSL:

```dos
mysql> status
```
Confirme que la conexión está cifrada revisando la salida, que debería mostrar:  **SSL: el cifrado en uso es AES256-SHA**. Este conjunto de cifrado muestra un ejemplo; en función del cliente, puede ver otro conjunto de cifrado.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Comprobación de que la aplicación o el marco de trabajo admiten conexiones TLS

Algunos marcos de trabajo de aplicaciones que usan MySQL en los servicios de bases de datos no habilitan TLS de manera predeterminada durante la instalación. El servidor MySQL establece conexiones TLS, pero, si la aplicación no está configurada para dicho protocolo, esta puede presentar un error al conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para aprender a habilitar las conexiones TLS.

## <a name="sample-code"></a>Código de ejemplo
Las cadenas de conexión que están predefinidas en la página "Cadenas de conexión", disponible para el servidor en Azure Portal, incluyen los parámetros necesarios de lenguajes comunes para conectarse a su servidor de bases de datos mediante TLS/SSL. El parámetro TLS/SSL varía en función del conector. Por ejemplo, puede ser "useSSL=true", "sslmode=required" o "ssl_verify_cert=true", entre otras variables.

Para establecer una conexión cifrada a su servidor flexible a través de TLS/SSL desde la aplicación, consulte los siguientes ejemplos de código:

### <a name="wordpress"></a>WordPress
Descargue el [certificado SSL público](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem) y agregue las siguientes líneas en wp-config.php después de la línea ```// ** MySQL settings - You can get this info from your web host ** //```.

```php
//** Connect with SSL** //
define('MYSQL_CLIENT_FLAGS', MYSQLI_CLIENT_SSL);
//** SSL CERT **//
define('MYSQL_SSL_CERT','/FULLPATH/on-client/to/DigiCertGlobalRootCA.crt.pem');
```

### <a name="php"></a>PHP

```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/DigiCertGlobalRootCA.crt.pem", NULL, NULL);
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```

### <a name="php-using-pdo"></a>PHP (mediante PDO)

```phppdo
$options = array(
    PDO::MYSQL_ATTR_SSL_CA => '/var/www/html/DigiCertGlobalRootCA.crt.pem'
);
$db = new PDO('mysql:host=mydemoserver.mysql.database.azure.com;port=3306;dbname=databasename', 'myadmin', 'yourpassword', $options);
```

### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)

```python
try:
    conn = mysql.connector.connect(user='myadmin',
                                   password='yourpassword',
                                   database='quickstartdb',
                                   host='mydemoserver.mysql.database.azure.com',
                                   ssl_ca='/var/www/html/DigiCertGlobalRootCA.crt.pem')
except mysql.connector.Error as err:
    print(err)
```

### <a name="python-pymysql"></a>Python (PyMySQL)

```python
conn = pymysql.connect(user='myadmin',
                       password='yourpassword',
                       database='quickstartdb',
                       host='mydemoserver.mysql.database.azure.com',
                       ssl={'ca': '/var/www/html/DigiCertGlobalRootCA.crt.pem'})
```

### <a name="django-pymysql"></a>Django (PyMySQL)

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'quickstartdb',
        'USER': 'myadmin',
        'PASSWORD': 'yourpassword',
        'HOST': 'mydemoserver.mysql.database.azure.com',
        'PORT': '3306',
        'OPTIONS': {
            'ssl': {'ca': '/var/www/html/DigiCertGlobalRootCA.crt.pem'}
        }
    }
}
```

### <a name="ruby"></a>Ruby

```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mysql.database.azure.com',
        :username => 'myadmin',
        :password => 'yourpassword',
        :database => 'quickstartdb',
        :sslca => '/var/www/html/DigiCertGlobalRootCA.crt.pem'
    )
```

### <a name="golang"></a>Golang

```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/DigiCertGlobalRootCA.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin' , 'yourpassword', 'mydemoserver.mysql.database.azure.com', 'quickstartdb')
db, _ := sql.Open("mysql", connectionString)
```

### <a name="java-mysql-connector-for-java"></a>Java (conector de MySQL para Java)

```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " +
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " +
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="java-mariadb-connector-for-java"></a>Java (conector de MariaDB para Java)

```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " +
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " +
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)

```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    SslCa = "DigiCertGlobalRootCA.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

## <a name="next-steps"></a>Pasos siguientes
- [Use MySQL Workbench para conectarse y consultar datos en Azure Database for MySQL: servidor flexible](./connect-workbench.md).
- [Use PHP para conectarse y consultar datos en Azure Database for MySQL: servidor flexible](./connect-php.md).
- [Cree y administre redes virtuales de Azure Database for MySQL: servidor flexible mediante la CLI de Azure](./how-to-manage-virtual-network-cli.md).
- Obtenga más información sobre las [redes de Azure Database for MySQL: servidor flexible](./concepts-networking.md).
- Obtenga más información sobre las [reglas de firewall de Azure Database for MySQL: servidor flexible](./concepts-networking.md#public-access-allowed-ip-addresses).
