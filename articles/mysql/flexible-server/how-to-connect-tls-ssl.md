---
title: Зашифрованное подключение с использованием TLS/SSL в базе данных Azure для MySQL — гибкий сервер
description: Инструкции и сведения о подключении с использованием TLS/SSL в базе данных Azure для MySQL — гибкого сервера.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 24a8dd4d21cb6ab6edeb985db4e6e6a1349a758d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90940487"
---
# <a name="encrypted-connectivity-using-transport-layer-security-tls-12-in-azure-database-for-mysql---flexible-server"></a>Зашифрованное подключение с использованием протокола TLS 1,2 в базе данных Azure для MySQL — гибкий сервер

> [!IMPORTANT]
> Сейчас предоставляется общедоступная предварительная версия Гибкого сервера Базы данных Azure для MySQL.

База данных Azure для гибкого сервера MySQL поддерживает подключение клиентских приложений к службе MySQL с помощью протокола TLS, ранее известного как SSL (SSL). TLS — это стандартный промышленный протокол, обеспечивающий шифрование сетевых подключений между сервером базы данных и клиентскими приложениями, что позволяет соблюдать требования соответствия.

Сервер базы данных Azure для MySQL поддерживает только зашифрованные подключения с использованием протокола TLS 1,2, а все входящие подключения с TLS 1,0 и TLS 1,1 будут отклонены. Для всех гибких серверов принудительное применение TLS-подключений включено, и вы не можете отключить TLS/SSL для подключения к гибкому серверу.

## <a name="applications-that-require-certificate-verification-for-tlsssl-connectivity"></a>Приложения, для которых требуется проверка сертификата для подключения TLS/SSL
В некоторых случаях для безопасного подключения приложениям требуется локальный файл сертификата, созданный из файла сертификата доверенного центра сертификации (ЦС). В базе данных Azure для гибкого сервера MySQL используется *глобальный корневой ЦС DigiCert*. Скачайте этот сертификат, необходимый для взаимодействия по протоколу SSL от [глобального корневого центра сертификации DigiCert](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem) и сохранения файла сертификата в предпочтительном расположении. Например, в этом учебнике используется `c:\ssl`.

### <a name="connect-using-mysql-command-line-client-with-tlsssl"></a>Подключение с помощью клиента командной строки MySQL с TLS/SSL

Если вы создали гибкий сервер в режиме *Закрытый доступ (интеграция с виртуальной сетью)* , к этому серверу придется подключаться с другого ресурса в той же виртуальной сети. Например, можно создать виртуальную машину и добавить ее в виртуальную сеть, созданную для гибкого сервера.

Если вы создали гибкий сервер в режиме *Открытый доступ (разрешенные IP-адреса)* , вы можете добавить локальный IP-адрес в список правил брандмауэра на этом сервере.

Для подключения к серверу из локальной среды можно выбрать [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) или [MySQL Workbench](./connect-workbench.md)— >. 

В следующем примере показано, как подключиться к серверу с помощью интерфейса командной строки MySQL. Используйте `--ssl-mode=REQUIRED` параметр строки подключения для принудительной проверки сертификата TLS/SSL. Передайте путь к файлу локального сертификата `--ssl-ca` параметру. Замените в ней предложенные значения реальными значениями имени сервера и пароля. 

```bash
 mysql.exe -h mydemoserver.mysql.database.azure.com -u myadmin -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\DigiCertGlobalRootCA.crt.pem
```
> [!Note]
> Убедитесь, что значение, передаваемое в, `--ssl-ca` соответствует пути к файлу сохраненного сертификата.

### <a name="verify-the-tlsssl-connection"></a>Проверка подключения TLS/SSL

Выполните команду MySQL **Status** , чтобы убедиться, что вы подключились к серверу MySQL с помощью TLS/SSL:

```dos
mysql> status
```
Убедитесь, что подключение зашифровано, просмотрев выходные данные:  **SSL: используется ШИФР AES256-SHA**. Этот комплект шифров показывает пример и на основе клиента можно увидеть другой комплект шифров.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Убедитесь, что приложение или платформа поддерживает подключения TLS.

Некоторые платформы приложений, использующие MySQL для своих служб баз данных, по умолчанию не включают TLS во время установки. Сервер MySQL применяет TLS-подключения, но если приложение не настроено для TLS, приложение может не подключиться к серверу базы данных. Чтобы узнать, как включить TLS, ознакомьтесь с документацией по приложению.

## <a name="sample-code"></a>Образец кода
Строки подключения, предварительно определенные на странице "строки подключения", доступной для сервера в портал Azure включают необходимые параметры для общих языков для подключения к серверу базы данных с помощью TLS/SSL. Параметр TLS/SSL зависит от соединителя. Например, "useSSL = true", "sslmode = Required" или "ssl_verify_cert = true" и другие варианты.

Чтобы установить зашифрованное подключение к гибкому серверу через TLS/SSL из приложения, ознакомьтесь со следующими примерами кода:

### <a name="php"></a>PHP

```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/DigiCertGlobalRootCA.crt.pem", NULL, NULL);
mysqli_real_connect($conn, 'mydemoserver.mysql.database.azure.com', 'myadmin', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```

### <a name="php-using-pdo"></a>PHP (с использованием PDO)

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

### <a name="django-pymysql"></a>Django (Пимискл)

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

### <a name="java-mysql-connector-for-java"></a>Java (соединитель MySQL для Java)

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

### <a name="java-mariadb-connector-for-java"></a>Java (соединитель MariaDB для Java)

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

## <a name="next-steps"></a>Дальнейшие шаги
- [Подключение и запрос данных в базе данных Azure для гибкого сервера MySQL с помощью MySQL Workbench](./connect-workbench.md)
- [Подключение и запрос данных в базе данных Azure для гибкого сервера MySQL с помощью PHP](./connect-php.md)
- [Создание и управление базой данных Azure для гибкого сервера виртуальной сети MySQL с помощью Azure CLI](./how-to-manage-virtual-network-cli.md).
- Дополнительные сведения о работе [в сети в базе данных Azure для гибкого сервера MySQL](./concepts-networking.md)
- Узнайте больше о [правилах брандмауэра для гибких серверов базы данных Azure для MySQL](./concepts-networking.md#public-access-allowed-ip-addresses)
