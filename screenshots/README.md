# Скриншоты

Скриншоты, используемые в документации проекта.

## Развёртывание (docs/deployment.md)
- vbox-soclab-general.png — параметры VM SOC_LAB
- vbox-soclab-network.png — Host-Only адаптер сервера
- vbox-kali-general.png — параметры VM Kali
- vbox-kali-network.png — Host-Only адаптер Kali
- ubuntu-console-version.png — консоль сервера, Ubuntu 26.04.1 LTS

## Сценарий 1. Брутфорс SSH (scenarios/01-ssh-bruteforce.md)
- 01-connectivity-ping.png — проверка связи (ping)
- 01-connectivity-nc.png — проверка порта 22
- 01-wordlist-creation.png — создание списка паролей
- 01-hydra-attack.png — запуск атаки hydra
- 01-authlog-failed.png — неудачные входы в auth.log
- 01-events-5760-table.png — события 5760 в дашборде
- 01-events-5760-histogram.png — всплеск событий на гистограмме
- 01-events-detection-2502.png — агрегирующее правило 2502 (уровень 10)
- 01-overview-sshd.png — сводка по неудачным аутентификациям
- 01-dashboard-before-attack.png — панель до атаки (0 неудачных аутентификаций)
- 01-dashboard-after-attack.png — панель после атаки (48 неудачных аутентификаций)
- 01-grep-5712-zero.png — правило 5712 не срабатывает по существующему юзеру
- mitre-t1110-reference.png — техника T1110 в MITRE ATT&CK

## Чего пока не хватает (по желанию доснять)
- развёрнутое отдельное событие 5760 с полями srcip / dstuser / rule.mitre.id
- раздел MITRE ATT&CK внутри самого дашборда Wazuh (а не сайт attack.mitre.org)
- вкладка Agents со статусом агента 000 Active
