# Vagrant_FAQ

---

## Что нужно

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/downloads)

## Как установить

***Если используется антивирус Kaspersky отключить его на время установки и при каждом обращение к vagrantcloud.com или добавить сайт vagrantcloud.com в доверенные в настройках антивируса***

1. 

Создайте директорию вашей будущей виртуальной машины и создать в ней файл под названием - *Vagrantfile*. Vagrantfile это файл без какого либо расширения, удалить расширение если присвоется.

2. 

Откройте его любым текстовым редактором. Разместите в нём код:

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
end
```

Vagrant предоставляет всем пользователям репозиторий готовых настроек — boxes. Найти их можно [здесь](https://app.vagrantup.com/boxes/search).

3. 

Выйдем в директорию, где у нас будет располагаться новая виртуальная машина, запустим консоль и выполним команду на клонирование GitHub-репозитория:

`git clone https://github.com/laravel/homestead.git Homestead` - Пример с виртуалкой Homestead 

4. 

Теперь перейдём в полученную директорию

Выполним там:

```
// для Mac / Linux...
bash init.sh

// для Windows
init.bat
```

В результате выполнения у нас создастся файл Homestead.yaml. Это файл конфигурации нашей будущей виртуальной машины. Он уже не на языке Ruby, а в формате YML, для удобства.

Откройте его любым текстовым редактором и внесите изменения

```
ip: "192.168.10.10" — это внутренний сетевой адрес, который будет присвоен вашей машине. На вашем личном компьютере можно оставить таким же, по умолчанию.
memory: 2048 — нашей виртуалке будет выделено два гигабайта RAM. Этого вполне достаточно для учебных проектов.
name: homestead — имя виртуальной машины. Для каждой новой виртуалки создавайте своё имя.
cpus: 2 — для виртуальной машины будет выделено два ядра CPU . Если у вас слабый процессор (например, Intel i3), можно уменьшить значение до 1.
provider: virtualbox — это наша среда виртуализации. Оставляем, как есть.
folders — это группа настроек общих директорий (проброс):
map — директория на хостовой машине. Укажите здесь адрес директории, в которой у вас будет храниться код проекта.
to — директория на виртуальной машине. Пока лучше оставить по умолчанию.

Когда вы меняете код в map, он будет автоматически проброшен в to.
Вы можете создать сколько угодно проброшенных директорий.
Sites — это настройка веб-адресов, которые будут обрабатываться нашей виртуальной машиной:
Создадим здесь map: mysite.local
Точка входа будет в директории /public того расположения, которое мы указали в folders (мы ещё раз вернёмся к такому расположению, когда будем изучать MVC)
В файле hosts нужно прописать DNS-соответствие. На Windows он расположен в :\Windows\System32\drivers\etc\hosts. На Linux/Mac — /etc/hosts.
Добавьте в конец файла hosts запись: 192.168.10.10  mysite.local
Databases — базы данных, которые будут созданы при установке. Зададим здесь БД skill-php.
Ports — пробрасываемые порты. Это важно для корректного подключения к машине, так как несколько виртуальных машин могут иметь открытыми одни и те же номера портов.
80 -> 80 — это порт для прямого обращения к созданному нами сайту
443 -> 443 — то же самое, но для HTTPS соединений
Обратите внимание, что по умолчанию часть портов уже проброшена:
2222 -> 22 – для SSH,
33060 -> 3306 – для MySQL
```

Весь файл:

```
---
ip: "192.168.10.10"
memory: 2048
name: homestead
cpus: 2
provider: virtualbox

folders:
    - map: F:\Homestead\code
      to: /home/vagrant/code

sites:
    - map: mysite.dev
      to: /home/vagrant/code/public

databases:
    - homestead

features:
    - mariadb: false
    - ohmyzsh: false
    - webdriver: false

ports:
    - send: 80
      to: 80
    - send: 443
      to: 443
```

5.

Теперь, когда мы всё настроили, можно запускать сборку виртуальной машины. Выполняем из директории Homestead следующие команды:


`git checkout release`

`vagrant up --provision`

---

