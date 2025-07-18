После подключения пк к гитхаб через [[SSH]]

Настраиваем Git

```bash
  git config --global user.email "you@example.com"
  git config --global user.name "Ваше Имя"
```

---

# Пушим хронология

Заходим в папку которую хотим запушить и делаем

```bash
git init
```

Создаем репозиторий на GitHub, потом переходим снова в папку которую пушим

```bash
git remote add origin git@github.com:mikemokoshi/linuxwiki.git
git branch -M main
git add -A
git commit -m "Комменатрий"
git push -u origin main # Первый раз пушим так, а потом просто git push
```

---

[[Arch Linux Database]]
