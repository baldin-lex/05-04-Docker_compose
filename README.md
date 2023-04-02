# Домашнее задание к занятию 4. «Оркестрация группой Docker-контейнеров на примере Docker Compose» - Балдин Алексей

### Задача 1

Создайте собственный образ любой операционной системы (например ubuntu-20.04) с помощью Packer ([инструкция](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart)).

Чтобы получить зачёт, вам нужно предоставить скриншот страницы с созданным образом из личного кабинета YandexCloud.

### Решение:

```console
root@debian:~/terraform# yc compute image list
+----------------------+--------------------------------------+-------------------+----------------------+--------+
|          ID          |                 NAME                 |      FAMILY       |     PRODUCT IDS      | STATUS |
+----------------------+--------------------------------------+-------------------+----------------------+--------+
| fd8rrs2f4rettr4qgmrl | debian-11-nginx-2023-04-01t13-23-12z | debian-web-server | f2ev4ka42u0pf6k8hsp3 | READY  |
+----------------------+--------------------------------------+-------------------+----------------------+--------+
```

![DOCKER-COMPOSE](images/2.jpg)

---

### Задача 2

**2.1.** Создайте вашу первую виртуальную машину в YandexCloud с помощью web-интерфейса YandexCloud.        

**2.2.*** **(Необязательное задание)**      
Создайте вашу первую виртуальную машину в YandexCloud с помощью Terraform (вместо использования веб-интерфейса YandexCloud).
Используйте Terraform-код в директории ([src/terraform](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/terraform)).

Чтобы получить зачёт, вам нужно предоставить вывод команды terraform apply и страницы свойств, созданной ВМ из личного кабинета YandexCloud.

### Решение:

***Прилагаю:*** 

***1. Скриншот свойств ВМ, созданной вручную:***

![DOCKER-COMPOSE](images/3.jpg)

***2. Вывод ```terraform apply```:***

```console
root@debian:~/terraform# terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # yandex_compute_instance.node01 will be created
  + resource "yandex_compute_instance" "node01" {
      + allow_stopping_for_update = true
      + created_at                = (known after apply)
      + description               = "node01"
      + folder_id                 = "b1gniajb0ktdbe17rq35"
      + fqdn                      = (known after apply)
      + gpu_cluster_id            = (known after apply)
      + hostname                  = "node01"
      + id                        = (known after apply)
      + name                      = "node01"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v2"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8rrs2f4rettr4qgmrl"
              + name        = (known after apply)
              + size        = 30
              + snapshot_id = (known after apply)
              + type        = "network-ssd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = "e9b8tnu5mqpdski71t1l"
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 4
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

yandex_compute_instance.node01: Creating...
yandex_compute_instance.node01: Still creating... [10s elapsed]
yandex_compute_instance.node01: Still creating... [20s elapsed]
yandex_compute_instance.node01: Still creating... [30s elapsed]
yandex_compute_instance.node01: Still creating... [40s elapsed]
yandex_compute_instance.node01: Creation complete after 46s [id=fhmcubn1gasp0ideb502]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```
***3. Скриншот свойств ВМ, созданной terraform:***

![DOCKER-COMPOSE](images/4.jpg)

---

### Задача 3

С помощью Ansible и Docker Compose разверните на виртуальной машине из предыдущего задания систему мониторинга на основе Prometheus/Grafana.
Используйте Ansible-код в директории ([src/ansible](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/ansible)).

Чтобы получить зачёт, вам нужно предоставить вывод команды "docker ps" , все контейнеры, описанные в [docker-compose](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/docker-compose.yaml),  должны быть в статусе "Up".

### Решение:

***1. Взял код из исходников и развернул инфраструктуру:***

```console

root@debian:~/task_3/terraform# terraform apply

...
...
...
...

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

Outputs:

external_ip_address_node01_yandex_cloud = "158.160.37.99"
internal_ip_address_node01_yandex_cloud = "192.168.101.33"
```

***2. На моем Debian не ставится никак Ansible (E: Невозможно исправить ошибки: у вас зафиксированы сломанные пакеты.). Поэтому я решил на ВМ поставить всё при помощи Docker-Compose. В итоге получаю вывод ```docker ps```:***

```console
root@node01:~# docker ps
CONTAINER ID   IMAGE                              COMMAND                  CREATED          STATUS                          PORTS      NAMES
c99f18a751e8   prom/prometheus:v2.17.1            "/bin/prometheus --c…"   34 minutes ago   Restarting (1) 12 seconds ago              prometheus
4e179c64966e   prom/alertmanager:v0.20.0          "/bin/alertmanager -…"   34 minutes ago   Restarting (1) 13 seconds ago              alertmanager
83ab208f0ac1   stefanprodan/caddy                 "/sbin/tini -- caddy…"   34 minutes ago   Restarting (1) 14 seconds ago              caddy
34d6024feffc   prom/node-exporter:v0.18.1         "/bin/node_exporter …"   34 minutes ago   Up 25 minutes                   9100/tcp   nodeexporter
0539a2b6c2f8   gcr.io/cadvisor/cadvisor:v0.47.0   "/usr/bin/cadvisor -…"   34 minutes ago   Up 25 minutes (healthy)         8080/tcp   cadvisor
f4f52ef96527   grafana/grafana:7.4.2              "/run.sh"                34 minutes ago   Up 25 minutes                   3000/tcp   grafana
f8b39c2c007f   prom/pushgateway:v1.2.0            "/bin/pushgateway"       34 minutes ago   Up 25 minutes                   9091/tcp   pushgateway
```

***Логи контейнеров prometheus, alertmanager, caddy примерно одинаковые. ВРоде как, жалуется на отсутствие конфигурационных файлов*** 

```
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:683 fs_type=EXT4_SUPER_MAGIC
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:684 msg="TSDB started"
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:788 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:535 msg="Stopping scrape discovery manager..."
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:549 msg="Stopping notify discovery manager..."
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:571 msg="Stopping scrape manager..."
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:531 msg="Scrape discovery manager stopped"
level=info ts=2023-04-02T15:34:43.272Z caller=manager.go:882 component="rule manager" msg="Stopping rule manager..."
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:545 msg="Notify discovery manager stopped"
level=info ts=2023-04-02T15:34:43.272Z caller=manager.go:892 component="rule manager" msg="Rule manager stopped"
level=info ts=2023-04-02T15:34:43.272Z caller=main.go:565 msg="Scrape manager stopped"
level=info ts=2023-04-02T15:34:43.275Z caller=notifier.go:598 component=notifier msg="Stopping notification manager..."
level=info ts=2023-04-02T15:34:43.275Z caller=main.go:738 msg="Notifier manager stopped"
level=error ts=2023-04-02T15:34:43.275Z caller=main.go:747 err="error loading config from \"/etc/prometheus/prometheus.yml\": couldn't load configuration (--config.file=\"/etc/prometheus/prometheus.yml\"): open /etc/prometheus/prometheus.yml: no such file or directory"
```

```
level=info ts=2023-04-02T15:34:41.650Z caller=cluster.go:632 component=cluster msg="gossip not settled but continuing anyway" polls=0 elapsed=24.64227ms
level=info ts=2023-04-02T15:35:42.061Z caller=main.go:231 msg="Starting Alertmanager" version="(version=0.20.0, branch=HEAD, revision=f74be0400a6243d10bb53812d6fa408ad71ff32d)"
level=info ts=2023-04-02T15:35:42.061Z caller=main.go:232 build_context="(go=go1.13.5, user=root@00c3106655f8, date=20191211-14:13:14)"
level=info ts=2023-04-02T15:35:42.061Z caller=cluster.go:161 component=cluster msg="setting advertise address explicitly" addr=172.21.0.5 port=9094
level=info ts=2023-04-02T15:35:42.062Z caller=cluster.go:623 component=cluster msg="Waiting for gossip to settle..." interval=2s
level=info ts=2023-04-02T15:35:42.085Z caller=coordinator.go:119 component=configuration msg="Loading configuration file" file=/etc/alertmanager/config.yml
level=error ts=2023-04-02T15:35:42.085Z caller=coordinator.go:124 component=configuration msg="Loading configuration file failed" file=/etc/alertmanager/config.yml err="open /etc/alertmanager/config.yml: no such file or directory"
level=info ts=2023-04-02T15:35:42.085Z caller=cluster.go:632 component=cluster msg="gossip not settled but continuing anyway" polls=0 elapsed=22.308692ms
```

```
2023/04/02 15:33:40 loading Caddyfile via flag: open /etc/caddy/Caddyfile: no such file or directory
2023/04/02 15:34:40 loading Caddyfile via flag: open /etc/caddy/Caddyfile: no such file or directory
2023/04/02 15:35:40 loading Caddyfile via flag: open /etc/caddy/Caddyfile: no such file or directory
2023/04/02 15:36:41 loading Caddyfile via flag: open /etc/caddy/Caddyfile: no such file or directory
```

***При этом конфигурационные файлы, на которые ругается docker, есть. Я использовал эти https://github.com/netology-code/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/ansible/stack*** 

***Подскажите, пожалуйста, что не так делаю.***

## Задача 4

1. Откройте веб-браузер, зайдите на страницу http://<внешний_ip_адрес_вашей_ВМ>:3000.
2. Используйте для авторизации логин и пароль из [.env-file](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/.env).
3. Изучите доступный интерфейс, найдите в интерфейсе автоматически созданные docker-compose-панели с графиками([dashboards](https://grafana.com/docs/grafana/latest/dashboards/use-dashboards/)).
4. Подождите 5-10 минут, чтобы система мониторинга успела накопить данные.

Чтобы получить зачёт, предоставьте: 

- скриншот работающего веб-интерфейса Grafana с текущими метриками, как на примере ниже.
<p align="center">
  <img width="1200" height="600" src="./assets/yc_02.png">
</p>
