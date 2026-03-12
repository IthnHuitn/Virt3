# Домашнее задание к занятию "`Оркестрация группой Docker контейнеров на примере Docker Compose`" - `Ефимов Вячеслав`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

[Docker-Hub](https://hub.docker.com/repository/docker/ithhuitn/custom-nginx/general)


---

### Задание 2

![Virt3-2-1](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-2-1.png)
![Virt3-2-2](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-2-2.png)


---

### Задание 3

![Virt3-3-1](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-1.png)

#### Почему контейнер остановился:
Контейнер остановился, потому что команда docker attach подключилась к основному процессу контейнера (PID 1), которым является nginx. Когда мы нажали Ctrl-C, сигнал был передан этому процессу, и nginx завершил свою работу. В Docker контейнер живет, пока живет его главный процесс (PID 1). Как только процесс nginx завершился, контейнер остановился.

![Virt3-3-2](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-2.png)
![Virt3-3-3](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-3.png)
![Virt3-3-5](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-5.png)

#### Объяснение проблемы:
Проблема в том, что контейнер был запущен с пробросом порта 127.0.0.1:8080:80, что означает: "все запросы, приходящие на порт 8080 хоста, перенаправлять на порт 80 контейнера". Однако внутри контейнера мы изменили конфигурацию nginx так, что он теперь слушает порт 81, а не 80. В результате:

- Запросы приходят на порт 80 контейнера (через проброс с хоста)

- Но nginx внутри контейнера ничего не слушает на порту 80

- Соединение не может быть установлено (connection reset)

Командой
```bash
docker inspect --format="{{.Id}}" custom-nginx-t2
```     
нахожу полный ID контейнера

![Virt3-3-8](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-8.png)

редактирую файлы конфигурации контейнера : hostconfig.json
 - Было
```json
"PortBindings":{"80/tcp":[{"HostIp":"127.0.0.1","HostPort":"8080"}]} 
```
 - Стало
```json
"PortBindings":{"81/tcp":[{"HostIp":"127.0.0.1","HostPort":"8080"}]}
```

![Virt3-3-6](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-6.png)

- config.v2.json
  - Было
```json
"Config": {
    ...
    "ExposedPorts": {
        "80/tcp": {}
    },
    ...
}
```
  - Стало
```json
"Config": {
    ...
    "ExposedPorts": {
        "81/tcp": {}
    },
    ...
}
```

![Virt3-3-7](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-7.png)

После внесения изменений запускаю Docker и проверяю результат.


![Virt3-3-8-1](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-8-1.png)


Удаляю контейнер одной командой:

![Virt3-3-9](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-3-9.png)

### Задание 4

![Virt3-4-1](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-4-1.png)
![Virt3-4-2](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-4-2.png)


### Задание 5

![Virt3-5-1](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-1.png)
![Virt3-5-2](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-2.png)
![Virt3-5-3](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-3.png)
![Virt3-5-4](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-4.png)
![Virt3-5-5](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-5.png)
![Virt3-5-6](https://github.com/IthnHuitn/Virt3/blob/master/screens/Virt3-5-6.png)


