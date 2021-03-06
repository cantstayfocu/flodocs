# Основні відомості про dcrctl 

Останнє оновлення для v1.0.0.

Цей посібник допоможе вам вивчити основні команди додатку `dcrctl` використовуючи мінімальний конфігураційний файл[minimal configuration file](/advanced/manual-cli-install.md#minimum-configuration). 

**Передумови:**

- Використовуйте останню версію [dcrinstall](/getting-started/user-guides/cli-installation.md) для установки `dcrctl`. кщо використано інший метод установки, будуть потрібні додаткові кроки. 
- Review how the launch commands for the Command Prompt (Windows) and Bash (macOS/Linux) shells differ [here](/getting-started/cli-differences.md).
- Встановіть dcrd [Setup dcrd](/getting-started/user-guides/dcrd-setup.md) і запустіть його у фоновому режимі.
- [Setup dcrwallet](/getting-started/user-guides/dcrwallet-setup.md) і запустіть його у фоновому режимі.

---

`dcrctl` це клієнт, який керує `dcrd` та `dcrwallet` за допомогою виклику віддалених процедур (RPC). Ви можете використовувати `dcrctl` для багатьох речей, таких як перевірка балансу, придбання тікетів, створення транзакцій та перегляд інформації про мережу.

** НАГАДУВАННЯ: ** Цей посібник використовує приклади використання команд безвідносно операційних систем. Перегляньте передумови, щоб визначити, чи слід Вам використовувати `./dcrctl` або `dcrctl.exe` замість `dcrctl`.

---

> Налаштування імені користувача та паролю RPC

ля надсилання команд до `dcrd` або `dcrwallet` будуть потрібні ім`я користувача / паролі RPC у конфігураційних файлах.

Якщо Ви використовували [`dcrinstall`](/getting-started/user-guides/cli-installation.md), Ваші конфігураційні файли вже містять імена користувача / паролі RPC для `dcrd`, `dcrwallet`, та `dcrctl`.

Якщо Ви не використовували `dcrinstall`, Вам потрібно буде ввімкнути мінімальні параметри у Ваших конфігураційних файлах. Для цього дотримуйтесь інструкцій [цього посібника] [this guide](/advanced/manual-cli-install.md#minimum-configuration).

---

## Команди dcrctl

YВам потрібно буде запускати команди dcrctl у окремому вікні командної оболонки (Command Prompt / Bash).

Щоб задавати команди `dcrwallet`, Вам буде потрібно використовувати `dcrctl --wallet <command>`.

Щоб задавати команди `dcrd`, Вам буде потрібно використовувати `dcrctl <command>`.

Щоб переглянути повний список команд, які `dcrctl` може надсилати `dcrd` та `dcrwallet`, введіть у командому рядку таке:

```no-highlight
dcrctl -l
```

З`явиться дуже довгий список команд, розділених за додатками. Команди верхньої секції призначені для `dcrd` а команди в нижній секції - для `dcrwallet`.  Ви можете дізнатись більше про кожну окрему команду, увівши для `dcrwallet` таке:

```no-highlight
dcrctl help --wallet <command>
```

І для команд `dcrd` таке:

```no-highlight
dcrctl help <command>
```

---

## Розблокування гаманця

Деякі функції `dcrwallet` вимагають розблокування гаманця.

Команда для розблокування Вашого гаманця виглядає так: 

```no-highlight
dcrctl --wallet walletpassphrase <private passphrase set during wallet creation> 0
```

Тут ми уточнюємо для `dcrctl` необхідність надіслати команду на `dcrwallet` використовуючи прапорець `--wallet` Фактична команда - це `walletpassphrase` який сприймає два параметри, Ваш особистий пароль (passphrase) та часовий ліміт. Зазначення `0` як часового ліміту разблковує `dcrwallet` на необмеженний час. Зауважте, однак, що це розблоковує гаманець лише для поточного сеансу. Якщо Ви закриєте вікно, в якому працює гаманець, або зупините / перезапустите `dcrwallet`, то Вам доведеться ще раз розблокувати його при наступному запуск. 

Якщо команда була успішною, Ви не отримаєте підтвердження від `dcrctl`, але якщо подивитеся на свою сесію `dcrwallet` вона покаже:

```no-highlight
[INF] RPCS: The wallet has been unlocked without a time limit.
```

ПРИМІТКА. Оскільки розблокування потрібне для багатьох функцій `dcrwallet`, `dcrwallet` можна запустити з використанням прапорця `--promptpass` або параметром `promptpass=true` у `dcrwallet.conf` (обговорювалося тут [here](/advanced/storing-login-details.md#dcrwalletconf)).

---

## Перевірка балансу

Щоб надіслати команду `getbalance` на `dcrwallet` використовуючи `dcrctl`, введіть у командну оболонку таке:

```no-highlight
dcrctl --wallet getbalance
```

З`являються всі баланси всіх Ваших облікових записів.

---

## Отримання нової адреси для отримання платежів

Щоб надіслати команду `getnewaddress` на `dcrwallet` використовуючи `dcrctl`, введіть у командну оболонку таке:

```no-highlight
dcrctl --wallet getnewaddress
```

Це дозволить створити та показати нову платіжну адресу. Щоб мінімізувати повторне використання адреси, використовуйте цю команду для отримання нової адреси для кожної транзакції, яку Ви бажаєте отримати

---

## Відправлення DCR

Щоб надіслати DCR на адресу, задайте команду `sendtoaddress` на `dcrwallet` використовуючи `dcrctl`. Введіть у командну оболонку таке, заповнивши адресу одержувача та суму,що надсилається:

```no-highlight
dcrctl --wallet sendtoaddress <address> <amount>
```

У випадку успіху `dcrctl` покаже хеш транзакції, який можна буде використовувати для перегляду транзакції в офіційному [Decred Block Explorer](/getting-started/using-the-block-explorer.md).

---

## Перевірити статистику мережі

Існує багато різних команд для перевірки різних статистичних даних мережі. Тут ми розглянемо `getinfo` для `dcrd` і `getstakeinfo` для `dcrwallet`.

Щоб отримати інформацію про Ваш локальний `dcrd` вузол, надішліть команду `getinfo` на `dcrd` використовуючи `dcrctl`. Введіть у командну оболонку таке:

```no-highlight
dcrctl getinfo
```

Щоб отримати інформацію про ставки у мережі Proof-of-Stake, надішліть команду `getstakeinfo` на `dcrwallet` за допомогою `dcrctl`. Введіть у командну оболонку таке:

```no-highlight
dcrctl --wallet getstakeinfo
```

---

## Додаткові команди

Додаткові команди також можна знайти на сторінці "Опції програми [Program Options](/advanced/program-options.md).

---
