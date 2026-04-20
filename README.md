# nwaku
# Інструкція: Встановлення Logos Messaging Node на новий сервер

## Вимоги
- Ubuntu/Debian сервер
- Мінімум 4GB RAM, 100GB диску
- Відкриті порти: `30304/tcp`, `30304/udp`, `9005/udp
- Встановлення Docker і Git

## Клонування репозиторію

```/dev/null/setup.sh#L1-3
git clone https://github.com/logos-messaging/logos-delivery-compose.git
cd logos-delivery-compose
```

---

## Створення .env файлу

```/dev/null/setup.sh#L1-12
cp .env.example .env
```

Відкрий `.env` і зміни:

```/dev/null/.env#L1-12
# RPC для Linea Sepolia
RLN_RELAY_ETH_CLIENT_ADDRESS=
# Постійний ключ ноди (згенеруй один раз: openssl rand -hex 32)
NODEKEY=
STORAGE_SIZE=50GB
RLN_RELAY_CRED_PASSWORD=
RLN_RELAY_CRED_PATH=/keystore/keystore.json
```

> ⚠️ `NODEKEY` — зберігай в безпечному місці. Це постійна ідентичність ноди в мережі. Якщо загубиш — нода стане "новою" для мережі.

---

## Відновлення keystore

```/dev/null/setup.sh#L1-5
mkdir -p keystore
# Скопіюй свій keystore.json на сервер
/root/logos-delivery-compose/keystore/keystore.json
```

## Запуск ноди

```/dev/null/setup.sh#L1-4
# Встановити розмір сховища
./set_storage_retention.sh

docker compose up -d
```

---

## Перевірка

```/dev/null/check.sh#L1-8
# Здоров'я ноди (зачекай ~30 сек після запуску)
./chkhealth.sh

# Живі логи
docker compose logs logos-messaging-node -f --tail 50
```

## Оновлення ноди (у майбутньому)

```/dev/null/update.sh#L1-5
docker compose down
git pull origin master
docker compose up -d
```

