# Introdução ao Quarkus Mailer

* Quarkus guide: https://quarkus.io/guides/mailer

Este é um aplicativo mínimo que envia e-mails com o Gmail.
Ele segue as instruções do guia de primeiros passos, mas também contém alguns exemplos extras:

* [usando template](src/main/java/org/acme/extra/ExtraMailResource.java)
* [usando modelo seguro de tipo](src/main/java/org/acme/extra/TypeSafeMailResource.java)
* [usando anexos](src/main/java/org/acme/extra/ExtraMailResource.java)

## Pré-requisitos

1. Você precisa gerar uma senha de aplicativo para usar o Gmail a partir do aplicativo.
Siga estas [instruções](https://support.google.com/mail/answer/185833) para criar a senha.

2. Você precisa do Java 11+.
3. Você precisa do GraalVM e do `native-image` instalados e configurados. Siga as [instruções](https://quarkus.io/guides/building-native-image) para baixar, instalar e configurar o GraalVM.

## Construindo o aplicativo

Inicie a compilação do Maven nas fontes verificadas desta demonstração:

```shell script
> ./mvnw package
```

Gere o executável nativo usando:

```shell script
> ./mvnw package -Dnative
```

O aplicativo contém testes que usaram o mailer _mock_ para evitar o envio de e-mails reais durante os testes.
O modo dev também usa este mailer _mock_.

## Configurando o aplicativo

O aplicativo usa GMAIL.
Defina as seguintes propriedades do ambiente:

* `QUARKUS_MAILER_USERNAME` - your Google email address
* `QUARKUS_MAILER_FROM` - your Google email address (or alias)
* `QUARKUS_MAILER_PASSWORD` - the application password generated above

### Running the application in JVM mode

Run the application with:

```shell script
> java -jar ./target/quarkus-app/quarkus-run.jar
```

Thanks to the environment properties defined above, the application should authenticate with Gmail and send the email.
To send the email, use the `/mail` endpoint.

Before doing so:

1. Change the email address used in `src/main/java/org/acme/MailResource.java`, so you can receive the email
2. If you want to send an email (and not just simulate it), add `quarkus.mailer.mock=false` to the `src/main/resources/application.properties` file

Then, in a terminal use

```shell script
curl -v :8080/mail
```

### Running the application as a native executable

You can also create a native executable from this application without making any
source code changes. A native executable removes the dependency on the JVM:
everything needed to run the application on the target platform is included in
the executable, allowing the application to run with minimal resource overhead.

Compiling a native executable takes a bit longer, as GraalVM performs additional
steps to remove unnecessary code paths. Use the  `native` profile to compile a
native executable:

```shell script
> ./mvnw package -Dnative
```

After getting a cup of coffee, you'll be able to run this executable directly:

```shell script
> ./target/mailer-quickstart-1.0.0-SNAPSHOT-runner
```

Then send an email using the same _curl_ command. 


## Other demos

Before running the other demos, don't forget to edit the code to use your email address.

### Using attachments

* Code: [using attachments](src/main/java/org/acme/extra/ExtraMailResource.java)
* Command: `curl -v http://localhost:8080/extra/attachment`

### Using template

* Code: [using template](src/main/java/org/acme/extra/ExtraMailResource.java)
* Template: [template](src/main/resources/templates/ExtraMailResource/hello.html)  
* Command: `curl -v http://localhost:8080/extra/template`

### Using type-safe template

* Code: [using type safe template](src/main/java/org/acme/extra/TypeSafeMailResource.java)
* Template: [template](src/main/resources/templates/TypeSafeMailResource/hello.html)
* Command: `curl -v http://localhost:8080/type-safe`



