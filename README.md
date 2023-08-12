# Introdução ao Quarkus Mailer

* Guia do Quarkus: https://quarkus.io/guides/mailer

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

* `QUARKUS_MAILER_USERNAME` - seu endereço de e-mail do Google
* `QUARKUS_MAILER_FROM` - seu endereço de e-mail do Google (ou alias)
* `QUARKUS_MAILER_PASSWORD` - a senha do aplicativo gerada acima

### Executando o aplicativo no modo JVM

Execute o aplicativo com:

```shell script
> java -jar ./target/quarkus-app/quarkus-run.jar
```

Graças às propriedades do ambiente definidas acima, o aplicativo deve se autenticar no Gmail e enviar o e-mail.
Para enviar o e-mail, use o endpoint `/mail`.

Antes de fazer isso:

1. Altere o endereço de e-mail usado em `src/main/java/org/acme/MailResource.java`, para que você possa receber o e-mail
2. Se você deseja enviar um e-mail (e não apenas simular), adicione `quarkus.mailer.mock=false` ao arquivo `src/main/resources/application.properties`

Em seguida, em um terminal, use

```shell script
curl -v :8080/mail
```

### Executando o aplicativo como um executável nativo

Você também pode criar um executável nativo a partir deste aplicativo sem fazer nenhum
alterações no código-fonte. Um executável nativo remove a dependência da JVM:
tudo o que é necessário para executar o aplicativo na plataforma de destino está incluído no
o executável, permitindo que o aplicativo seja executado com sobrecarga mínima de recursos.

A compilação de um executável nativo demora um pouco mais, pois o GraalVM executa
etapas para remover caminhos de código desnecessários. Use o perfil `native` para compilar um
executável nativo:

```shell script
> ./mvnw package -Dnative
```

Depois de tomar uma xícara de café, você poderá executar este executável diretamente:

```shell script
> ./target/mailer-quickstart-1.0.0-SNAPSHOT-runner
```

Em seguida, envie um e-mail usando o mesmo comando _curl_.


## Outras demonstrações

Antes de executar as outras demos, não se esqueça de editar o código para usar seu endereço de e-mail.

### Usando anexos

* Código: [using attachments](src/main/java/org/acme/extra/ExtraMailResource.java)
* Commando: `curl -v http://localhost:8080/extra/attachment`

### Usando modelo

* Código: [using template](src/main/java/org/acme/extra/ExtraMailResource.java)
* Template: [template](src/main/resources/templates/ExtraMailResource/hello.html)  
* Commando: `curl -v http://localhost:8080/extra/template`

### Usando modelo de tipo seguro

* Code: [using type safe template](src/main/java/org/acme/extra/TypeSafeMailResource.java)
* Template: [template](src/main/resources/templates/TypeSafeMailResource/hello.html)
* Command: `curl -v http://localhost:8080/type-safe`



