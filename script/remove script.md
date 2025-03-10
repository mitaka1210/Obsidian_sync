
---
title: "himom.sh - Node.js Cleaner & Runner"  
tags: ["bash", "script", "nodejs", "cleanup"]  
created: {{date}}  
updated: {{date}}  
---

# 📝 himom.sh - Node.js Cleaner & Runner  

## 🔍 Описание  
Този Bash скрипт **изчиства кеша и ненужните файлове** на проекти с **React, Angular, Vue и Next.js**. След това преинсталира **npm пакетите** и **стартира съответния framework**.

## ⚡ Основни функционалности  
- Изтрива **`node_modules`**, **`.angular`**, **`.next`** и `package-lock.json`  
- Чисти npm кеша  
- Преинсталира зависимостите  
- Позволява избор на framework и го стартира  

## 🖥 Код  
```bash
#!/bin/bash
echo "Да започваме купона"
whereami=$(pwd)
echo "where am i $whereami"
cd $whereami
find . -name 'node_modules' -type d -prune -exec rm -rf '{}' +
rm -rf ".angular"
rm -rf ".next"
rm package-lock.json
sleep 2
echo "DELETE ALL"
sleep 2
npm cache clean --force
echo "FINISH CLEAN CACHE"
sleep 2
npm i --force
echo "FINISH INSTALL NODE"
sleep 5
echo "FINISH ALL"
sleep 5
echo "framework name"
sleep 2
read -p "Enter first string: " VAR1
if [[ "$VAR1" == "react" ]]; then
    echo "Strings = react."
    npm start
elif [[ "$VAR1" == "angular" ]]; then
    echo "Strings = angular."
    npm start
elif [[ "$VAR1" == "vue" ]]; then
    echo "Strings = vue."
    npm run serve
elif [[ "$VAR1" == "nextjs" ]]; then
    echo "Strings = nextjs build START"
    npm run build
    sleep 10
    echo "Strings = nextjs build READY!!!!"
    npm run start
else
    echo "String = WRONG"
fi
