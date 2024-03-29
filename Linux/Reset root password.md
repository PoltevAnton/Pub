# Сброс пароля root

## Ubuntu

1. **Загрузите систему и дождитесь появления меню GRUB.** Нажмите любую клавишу, когда увидите приглашение "Нажмите любую клавишу для входа в меню".
2. **Нажмите "e", чтобы редактировать команды загрузки.**
3. **Найдите строку загрузки ядра (например, `linux /boot/vmlinuz-3.2.0-24-generic root=UUID=bc6f8146-1523-46a6-8b6a-64b819ccf2b7 ro`).** Добавьте `quiet init=/bin/bash` в конец строки.
4. **Нажмите `Ctrl+x` или `F10`, чтобы продолжить загрузку.**
5. **Перемонтируйте файловую систему в режиме чтения-записи:**
   ```bash
   mount -o remount,rw /
   ```
6. **Измените пароль root:**
   ```bash
   passwd
   ```
7. **Перезагрузитесь:**
   ```bash
   reboot
   ```

## CentOS 7

1. **При загрузке остановите загрузчик на GRUB.** Выберите строку из меню загрузки и нажмите `e` (Edit).
2. **Добавьте `rd.break` в конец строки загрузки ядра (linux16).**
3. **Нажмите `Ctrl - x` для загрузки.**
4. **Найдите `/sysroot` в списке смонтированных файловых систем:**
   ```bash
   mount
   ```
5. **Перемонтируйте `/sysroot` в режиме чтения-записи:**
   ```bash
   mount -o remount,rw /sysroot
   ```
6. **Войдите в образ системы:**
   ```bash
   chroot /sysroot
   ```
7. **Измените пароль root:**
   ```bash
   passwd
   ```
8. **Создайте файл `.autorelabel` (если SELinux разрешен):**
   ```bash
   touch /.autorelabel
   ```
9. **Выйдите из `chroot` и перемонтируйте `/sysroot` в режиме только чтение:**
   ```bash
   exit
   mount -o remount,ro /sysroot
   ```
10. **Перезагрузитесь.**

### Второй вариант сброса пароля (только для CentOS 7)

1. **Добавьте `rd.break enforcing=0` в конец строки аргументов ядра.**
2. **Перезагрузитесь.**
3. **После входа в систему с новым паролем выполните:**
   ```bash
   restorecon /etc/shadow
   setenforce 1
   getenforce
   ```