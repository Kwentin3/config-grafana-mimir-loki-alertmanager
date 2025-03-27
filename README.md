# 🛠️ Monitoring Stack Configs Backup
**Автоматизированное резервное копирование конфигураций Grafana, Mimir, Loki и Alertmanager**

---

## 🔍 Краткое описание
Этот репозиторий содержит:
- Актуальные конфигурации сервисов мониторинга:
  - `mimir/config.yaml`  
  - `loki/config.yaml`  
  - `alertmanager/config.yaml`  
- Docker Compose файлы (`compose/production/`)  
- Скрипт автоматического бэкапа [`sync_and_backup.sh`](../sync_and_backup.sh)

---

## ⚙️ Как работает синхронизация
1. **Источник данных**:  
   Конфиги берутся с сервера по путям:
   ```
   /opt/monitoring-configs/{mimir,loki,alertmanager}/config.yaml
   /var/lib/docker/volumes/portainer_data/_data/compose/66/docker-compose.yml
   ```

2. **Скрипт бэкапа**:
   - Копирует файлы → коммитит изменения → пушит в `main`  
   - **Не удаляет файлы**, существующие только в GitHub (без `git add --all`)  
   - Логирует действия в `/var/log/monitoring_backup.log`

3. **Расписание**:  
   Автозапуск через Cron каждый день в **02:30**:
   ```bash
   30 2 * * * /opt/monitoring-configs/sync_and_backup.sh
   ```

---

## 🚀 Быстрый старт
### Восстановить конфиги на сервере:
```bash
cd /opt/monitoring-configs/config-grafana-mimir-loki-alertmanager
sudo cp -r mimir/config.yaml /opt/monitoring-configs/mimir/
sudo cp -r loki/config.yaml /opt/monitoring-configs/loki/
```

### Запустить синхронизацию вручную:
```bash
sudo /opt/monitoring-configs/sync_and_backup.sh
```

---

## 🔧 Troubleshooting
| Проблема | Решение |
|----------|---------|
| `git push` rejected | Выполните `git pull --rebase origin main` |
| Нет прав на запись | `sudo chmod 644 /var/log/monitoring_backup.log` |
| Скрипт падает с ошибкой | Проверьте `tail -f /var/log/monitoring_backup_errors.log` |

---

## 📜 Лицензия
MIT © [Ваше имя/команда]  
**Обновлено**: 2025-03-28
```

---

### Ключевые преимущества:
1. **Минимум текста** — только факты и команды.
2. **Готовые решения** для частых проблем в таблице.
3. **Логическая структура** — от общего к частному.
4. **Поддержка копирования** — все команды можно сразу использовать.

Файл можно дополнять деталями по мере необходимости (например, секцией "Контакты").
