# 1. Инсталиране на необходимите пакети123

```
npm init -y
npm install express dotenv nodemailer node-cron pg

```

- `express` – за API (по избор)
- `dotenv` – за конфигурация на средата
- `nodemailer` – за изпращане на имейли
- `node-cron` – за автоматично изпълнение на задачи
- `pg` – за връзка с PostgreSQL

## 2. Конфигурация на `.env`

Създай `.env` файл с настройките:

```
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_password
DB_HOST=localhost
DB_USER=your_db_user
DB_PASS=your_db_pass
DB_NAME=birthdays

```

## 3. Свързване с базата данни (`database.js`)

```
const { Pool } = require('pg');
require('dotenv').config();

const pool = new Pool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME,
  port: 5432
});

module.exports = pool;

```

## 4. Създаване на таблица в PostgreSQL

```sql
CREATE TABLE birthdays (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    birthdate DATE
);

```

Добавяне на тестови данни:

```sql
INSERT INTO birthdays (name, email, birthdate)
VALUES ('Иван Иванов', 'ivan@example.com', '1990-03-06');

```

## 5. Изпращане на имейли (`sendEmail.js`)

```
const nodemailer = require('nodemailer');
require('dotenv').config();

const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: process.env.EMAIL_USER,
    pass: process.env.EMAIL_PASS
  }
});

async function sendBirthdayEmail(to, name) {
  const mailOptions = {
    from: process.env.EMAIL_USER,
    to,
    subject: 'Честит Рожден Ден!',
    text: `Честит Рожден Ден, ${name}! 🎉🎂 Пожелаваме ти много здраве и успехи!`
  };

  try {
    await transporter.sendMail(mailOptions);
    console.log(`Поздравен имейл изпратен на ${to}`);
  } catch (error) {
    console.error('Грешка при изпращане на имейл:', error);
  }
}

module.exports = sendBirthdayEmail;

```

## 6. Автоматична проверка и изпращане (`cronJob.js`)

```
const cron = require('node-cron');
const pool = require('./database');
const sendBirthdayEmail = require('./sendEmail');

cron.schedule('0 9 * * *', async () => {
  console.log('Проверяваме за рождени дни...');

  try {
    const today = new Date().toISOString().slice(0, 10);
    const { rows } = await pool.query('SELECT * FROM birthdays WHERE birthdate = $1', [today]);

    rows.forEach(user => {
      sendBirthdayEmail(user.email, user.name);
    });

    console.log('Проверка завършена.');
  } catch (error) {
    console.error('Грешка при изпълнение на cron job:', error);
  }
});

console.log('Cron job стартиран...');

```

## 7. Стартиране на сървъра (`server.js`)

```
const express = require('express');
require('./cronJob'); // Зареждаме автоматичната проверка за рождени дни

const app = express();
const PORT = 3000;

app.listen(PORT, () => console.log(`Сървърът работи на <http://localhost>:${PORT}`));

```

## 8. Стартиране на приложението

```
node server.js

```

## Допълнителни подобрения

- **Добавяне на уеб интерфейс** (React, Vue) за управление
- **Изпращане на SMS** чрез **Twilio API**
- **Интеграция с Telegram бот** за автоматични съобщения

---

**Our products:** 🔼 [3D Creator](https://www.koi-club.com/3dcreator) **|** ⚙️ [The Content Engine](https://www.notion.so/marketplace/templates/contentengine)

njF#wKybrv4h5yLTv