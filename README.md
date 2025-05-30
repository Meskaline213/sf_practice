# sf_practice
# sf_practice — Vagrant + VirtualBox Ubuntu 18.04

## Описание

Этот проект предназначен для автоматического развёртывания виртуальной машины с Ubuntu 18.04 с помощью Vagrant и VirtualBox.  
В процессе создания ВМ автоматически устанавливаются Python 3, pip, а также библиотеки Django и psycopg2 (или psycopg2-binary).  
Также в домашнюю директорию пользователя `vagrant` копируется файл `my_empty_file.txt` (может быть заменён на ваши скрипты).

## Требования

- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

## Быстрый старт

1. Клонируйте репозиторий или скопируйте содержимое в отдельную папку.
2. Убедитесь, что в папке находятся файлы:
    - `Vagrantfile`
    - `my_empty_file.txt` (или ваш скрипт)
3. Запустите виртуальную машину:
    ```bash
    vagrant up
    ```
4. Подключитесь к ВМ:
    ```bash
    vagrant ssh
    ```
5. Проверьте наличие файла и установленных библиотек:
    ```bash
    ls -l ~/my_empty_file.txt
    python3 --version
    pip3 show psycopg2
    pip3 show Django
    ```

## Проверка окружения (тесты)

После запуска и входа в виртуальную машину (`vagrant ssh`) выполните следующие команды для проверки:

```bash
# 1. Проверка наличия скопированного файла
ls -l ~/my_empty_file.txt

# 2. Проверка версий Python и pip
python3 --version
pip3 --version

# 3. Проверка установки библиотек через pip
pip3 show psycopg2   # или pip3 show psycopg2-binary, если использовали binary-версию
pip3 show Django

# 4. Проверка импорта библиотек в Python
python3 -c "import psycopg2; import django; print('psycopg2 и Django успешно импортированы!')"

# 5. Проверка версии Django
python3 -c 'import django; print(django.get_version())'
```

**Если все команды выполняются без ошибок и выводят ожидаемую информацию — окружение настроено корректно!**

## Примечания

- Для корректной работы файл `my_empty_file.txt` должен находиться в одной папке с `Vagrantfile`.
- При необходимости можно заменить `psycopg2` на `psycopg2-binary` в Vagrantfile для более быстрой установки.

## Пример Vagrantfile

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision "file", source: "my_empty_file.txt", destination: "/home/vagrant/my_empty_file.txt"
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y python3 python3-pip python3-dev libpq-dev
    pip3 install psycopg2 Django
  SHELL
end
```
