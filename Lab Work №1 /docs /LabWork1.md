# Лабораторная работа 1

Тема: Разработка информационной системы прохождения интерактивных презентаций и ее интерфейса.

# 1. Перечень заинтересованных лиц:
  - учителя школ (для проведения уроков, автоматизации проверки тестов и домашнего задания).
  - преподаватели ВУЗов (для проведения аудиторных занятий, автоматизации проверки тестов и домашнего задания).
  - репетиторы (для проведения занятий, автоматизации проверки тестов и домашнего задания).
  - преподавателей организаций дополнительного образования (для проведения занятий, автоматизации проверки тестов и домашнего задания).
  - спикеры на конференциях (для добавления интерактивности в свое выступление; как способ взаимодействия с публикой; проведения конкурсов/розыгрышей/викторин).
# 2. Функциональные требования:
  - Система должна позволять пользователю регистрироваться в системе.
  - Система должна позволять пользователю создавать интерактивные презентации.
  - Система должна позволять пользователю выбирать тип нового слайда: 
    - 5 типов слайдов-вопросов
      - множественный выбор
      - открытый вопрос
      - да/нет
      - правильный порядок
      - соответствие
    - 3 слайда-контента
      - текстовый слайд
      - рейтинг
      - QR-код 
  - Система должна позволять пользователю запускать презентацию в одном из режимов:
     - Онлайн с управляемым переключением слайдов
     - Онлайн с переключением слайдов по времени
     - Отложенный запуск
  - Система должна позволять пользователю встраивать презентацию на свой сайт или в Moodle.
  - Система должна позволять пользователю настраивать содержимое слайдов, а также их дизайн (устанавливать в качестве заднего фона изображение с открытого фотостока Unsplash).
  - Система должна позволять пользователю просматривать отчеты о результатах прохождения презентаций.
  - Система должна позволять пользователю скачивать отчеты в форматах Excel и PDF.
  - Система должна позволять пользователю настраивать такие параметры презентации как наличие музыки и реакций при прохождении презентации, модерация участников, перемешивание слайдов.
  - Система должна позволять пользователю отслеживать участников, подключенных к презентации, и при необходимости запрещать им прохождение.
# 3. Диаграмма вариантов использования для функциональных требований:
![Диаграмма вариантов использования](https://github.com/AnaSKBK/PAPS/blob/LabWork1/LR_1_diag.jpg)

# 4. Предположения:
  - У компании небольшой бюджет для реализации системы.
  - У компании сжатые сроки для реализации системы.
  - Платформа должна быть удобна при работе с мобильного устройства.
  - Подключение к презентации должно происходить с помощью ссылки или QR-кода.
# 5. Нефункциональные требования:
  - Система должна быть масштабируемой при увеличении количества пользователей.
  - Система должна корректно работать как минимум в 90% случаев.
