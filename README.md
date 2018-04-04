#### Utworzenie kluczy SSL

> keytool -genkey -alias broker -keyalg RSA keystore broker.ks

> keytool -export -alias broker -keystore broker.ks -file broker_cert 

> keytool -genkey -alias client -keyalg RSA -keystore client.ks

> keytool -import -alias broker -keystore client.ts -file broker_cert

Zapamiętać hasła do keystore

> keytool -list -keystore broker.ks

### Utworzenie serviceaccount w openshift

> echo {“kind”: “ServiceAccount”, “apiVersion”: “v1”, “metadata”: {“name”: “broker-service-account”}} | oc create -f –

### Utworzenie secretu

> oc secrets new broker-secret broker.ks

### Dodanie secretu do serviceaccount 

> oc secrets add sa/broker-service-account secret/broker-secret

### Dodanie roli view do serviceaccount

> oc policy add-role-to-user view system:serviceaccount:ekw-dev:broker-service-account

### Utworzenie brokera na openshift

- dodać do projektu aplikację z template JBoss A-MQ 6.3 (ssl)
- w parametrach ustawić nazwę i hasło użytkownika activemq
- w parametrach podać nazwę utworzonego secretu broker-secret
- jako keystore i truststore podać broker.ks
- w parametrach ustawić hała do keystore i truststore

### Utworzenie routy ssl

Dla serwisu broker-amq-ssl utworzyć routę: secure, terminacja passthrough

### konfiguracja klienta

Klient ma się podłączać po ssl na adres routy i port 443

