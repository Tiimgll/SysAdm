# FreeIPA без интегрированного DNS-сервера

Задача:
На сервере SRV-HQ сконфигурируйте основной доменный контроллер на базе FreeIPA

a) Создайте 30 пользователей user1-user30.
b) Пользователи user1-user10 должны входить в состав группы group1.
c) Пользователи user11-user20 должны входить в состав группы group2.
d) Пользователи user21-user30 должны входить в состав группы group3.
e) Разрешите аутентификацию с использованием доменных учетных данных на ВМ CLI-HQ.
f) Установите сертификат центра сертификации FreeIPA в качестве доверенного на клиентских ПК.

## SRV-HQ:
Развёртывание контроллера домена на базе FeeIPA:

Для ускорения установки можно установить демон энтропии haveged:

``` bash
apt-get update && apt-get install -y haveged
```

Включаем и запускаем службу haveged:

``` bash
systemctl enable --now haveged
```

Установим пакет FreeIPA:

``` bash
apt-get install -y freeipa-server
```

Запускаем интерактивную установку FreeIPA:

``` bash
ipa-server-install
```

1 - отвечаем no на вопрос, нужно ли сконфигурировать DNS-сервер BIND;

2, 3, 4 - нужно указать имя узла на котором будет установлен сервер FreeIPA, доменное имя и пространство Kerberos;

![screen1](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/1.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

1 - задаётся и подтверждается пароль для Director Manager;

2 - задаётся и подтверждается пароль для администратора FreeIPA;

![screen2](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/2.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

1 - указывается имя NetBIOS;

2 - Указать, если это необходимо, NTP-сервер или пул серверов:

3 - Далее необходимо проверить информацию о конфигурации и подтвердить ответив yes:

![screen3](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/3.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

Проверяем запущенные службы FreeIPA:

![screen4](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/4.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

### Далее выполняем создание необходимых групп и пользователей:
Пользователей и группы можно накликать и в веб-интерфейсе FreeIPA с CLI-HQ

получаем билет kerberos:

``` bash
kinit admin
```

вводим пароль доменного пользователя admin:

![screen5](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/5.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

Используя цикл for создадим 30 пользователей: user№ с паролем P@ssw0rd, а также сменим срок действия пароля до 2025 года, чтобы при входе из под пользователя на не пришлось менять пароль:

``` bash
for i in {1..30};do 
	echo "P@ssw0rd" | ipa user-add user$i --first=User --last=$i --password;
	ipa user-mod user$i --setattr=krbPasswordExpiration=20251225011529Z;
done
```

Создадим группы: group1, group2 и group3:

``` bash 
for i in {1..3}; do
	ipa group-add group$i;
done
```

Добавим пользователей в соответствующие группы:
group1:

``` bash
for i in {1..10}; do
	ipa group-add-member group1 --users=user$i;
done
```

Добавим пользователей в соответствующие группы:
group2:

``` bash
for i in {11..20}; do
	ipa group-add-member group2 --users=user$i;
done
```

Добавим пользователей в соответствующие группы:
group3:

``` bash
for i in {21..30}; do
	ipa group-add-member group3 --users=user$i;
done
```


## CLI-HQ:
Установим необходимые пакеты:

``` bash
apt-get update && apt-get install -y freeipa-client zip
```

Запустим скрипт настройки клиента в интерактивном режиме:

``` bash
ipa-client-install --mkhomedir
```

>[!NOTE]
>Скрипт установки должен автоматически найти необходимые настройки на FreeIPA сервере, вывести их и спросить подтверждение для найденных параметров;
>[!NOTE]
>Затем запрашивается имя пользователя, имеющего право вводить машины в домен, и его пароль (можно использовать администратора по умолчанию, который был создан при установке сервера);
>[!NOTE]
>Далее сценарий установки настраивает клиент.

![screen6](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/6.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

перезапускаем клиента:

``` bash
reboot
```

Также после ввода в домен - CLI-HQ автоматически доверяет интегрированному Корневому Центру Сертификации FreeIPA:

![screen7](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/7.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

![screen8](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/8.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)

![screen9](https://github.com/Tiimgll/Profis/blob/main/pic/FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0/9.FreeIPA%20%D0%B1%D0%B5%D0%B7%20%D0%B8%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE%20DNS-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0.png)


