# CommandLimit Plugin

Плагин для сервера Unturned, который ограничивает использование команды `/i` (выдача предметов).

## Возможности

- **Ограничение количества предметов** - устанавливает максимальное количество предметов за одну команду
- **Блэклист предметов** - запрещает выдачу определенных предметов
- **Замена предметов** - автоматически заменяет одни предметы на другие
- **Отдельные лимиты для админов** - разные ограничения для админов и игроков
- **Система разрешений** - возможность обхода ограничений через права доступа

## Установка

1. Поместите `CommandLimit.dll` в папку `Plugins` вашего сервера
2. Перезапустите сервер
3. Настройте конфигурацию в файле `CommandLimit.configuration.xml`

## Конфигурация

### Основные настройки

```xml
<Configuration>
  <!-- Лимит предметов для обычных игроков -->
  <MaxItemAmount>100</MaxItemAmount>
  
  <!-- Включить ограничение для игроков -->
  <EnableItemSpawnLimit>true</EnableItemSpawnLimit>
  
  <!-- Разрешение для обхода всех ограничений -->
  <BypassPermission>commandlimit.bypass</BypassPermission>
</Configuration>
```

### Настройки для админов

```xml
<!-- Включить ограничение для админов -->
<EnableAdminItemSpawnLimit>true</EnableAdminItemSpawnLimit>

<!-- Лимит предметов для админов -->
<AdminMaxItemAmount>500</AdminMaxItemAmount>
```

### Блэклист предметов

```xml
<!-- Включить блэклист -->
<EnableBlacklist>true</EnableBlacklist>

<!-- Список запрещенных ID предметов -->
<BlacklistedItems>
  <ushort>519</ushort>   <!-- Пример: запретить ID 519 -->
  <ushort>1337</ushort>  <!-- Пример: запретить ID 1337 -->
</BlacklistedItems>
```

### Замена предметов

```xml
<!-- Список замен предметов -->
<ItemReplacements>
  <ItemReplacement>
    <OriginalItemId>16</OriginalItemId>      <!-- Заменить предмет ID 16 -->
    <ReplacementItemId>17</ReplacementItemId> <!-- На предмет ID 17 -->
  </ItemReplacement>
  <ItemReplacement>
    <OriginalItemId>4</OriginalItemId>
    <ReplacementItemId>5</ReplacementItemId>
  </ItemReplacement>
</ItemReplacements>
```

### Дополнительные настройки

```xml
<!-- Подробное логирование -->
<EnableVerboseLogging>false</EnableVerboseLogging>
```

## Разрешения

- `commandlimit.bypass` - полный обход всех ограничений плагина

Выдать разрешение: `/p [игрок] commandlimit.bypass`

## Сообщения игрокам

Плагин отправляет следующие сообщения:

- **"You cannot give more than X items at once!"** - превышен лимит количества
- **"This item is blacklisted and cannot be spawned!"** - предмет в блэклисте
- **"Invalid item specified!"** - неверный ID/название предмета

Сообщения можно изменить в файле переводов плагина.

## Принцип работы

1. **Перехват команды** - плагин перехватывает выполнение команды `/i`
2. **Проверка разрешений** - если у игрока есть `commandlimit.bypass`, ограничения пропускаются
3. **Валидация предмета** - проверяется существование предмета и его наличие в блэклисте
4. **Замена предмета** - если настроена замена, предмет автоматически заменяется
5. **Проверка количества** - проверяется не превышает ли количество установленный лимит
6. **Выполнение команды** - если все проверки пройдены, команда выполняется

## Примеры использования

### Базовая настройка для PvP сервера
```xml
<MaxItemAmount>50</MaxItemAmount>
<EnableItemSpawnLimit>true</EnableItemSpawnLimit>
<EnableAdminItemSpawnLimit>true</EnableAdminItemSpawnLimit>
<AdminMaxItemAmount>200</AdminMaxItemAmount>
<EnableBlacklist>true</EnableBlacklist>
<BlacklistedItems>
  <ushort>519</ushort> <!-- Запретить взрывчатку -->
</BlacklistedItems>
```

### Замена опасных предметов на безопасные
```xml
<ItemReplacements>
  <ItemReplacement>
    <OriginalItemId>519</OriginalItemId>  <!-- Взрывчатка -->
    <ReplacementItemId>112</ReplacementItemId>  <!-- Заменить на MRE -->
  </ItemReplacement>
</ItemReplacements>
```
