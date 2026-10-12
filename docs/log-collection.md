# Настройка сбора логов на Ubuntu 26.04

При первых тестах брутфорса события атаки доходили до Wazuh нестабильно: правило
и декодер работали (проверено wazuh-logtest), атака доходила до сервера
(видно в journalctl), но в алертах фиксировалась лишь малая часть попыток.
Разбор показал две причины, связанные с особенностями свежей Ubuntu.

## Причина 1. Новый OpenSSH

В Ubuntu 26.04 демон SSH разделён на процессы, и в логах фигурирует имя
sshd-session (вместо sshd). Декодер Wazuh это имя распознаёт корректно
(^sshd совпадает с sshd-session), поэтому на декодирование это не влияет.

Дополнительно OpenSSH применяет механизм srclimit_penalise: после нескольких
неудачных попыток источник штрафуется (соединения обрываются на preauth), из-за
чего часть попыток вообще не логируется как Failed password. Для
воспроизводимости тестов в лаборатории штраф отключён:

```bash
echo 'PerSourcePenalties no' | sudo tee /etc/ssh/sshd_config.d/99-lab.conf
sudo systemctl restart ssh
```

## Причина 2. Rate-limit journald

systemd-journald при всплеске однотипных сообщений может их отбрасывать. Лимит снят:

```bash
sudo mkdir -p /etc/systemd/journald.conf.d
printf '[Journal]\nRateLimitIntervalSec=0\nRateLimitBurst=0\n' \
  | sudo tee /etc/systemd/journald.conf.d/no-ratelimit.conf
sudo systemctl restart systemd-journald
```

## Надёжный сбор через текстовый лог

По умолчанию в Ubuntu 26.04 работает только journald, файла /var/log/auth.log
нет. Для стабильного сбора событий SSH установлен rsyslog и Wazuh переключён на
чтение текстового лога:

```bash
sudo apt install -y rsyslog
sudo systemctl enable --now rsyslog
```

В /var/ossec/etc/ossec.conf добавлен блок:
```xml
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/auth.log</location>
</localfile>
```

После изменений:
```bash
sudo systemctl restart wazuh-manager
```

## Итог

Эти доработки обеспечили стабильную доставку событий от источника до правил
Wazuh и воспроизводимость сценариев атак. Сам по себе разбор (декодер vs
доставка, особенности OpenSSH/journald) — пример практической работы с
конвейером обработки событий SIEM.
