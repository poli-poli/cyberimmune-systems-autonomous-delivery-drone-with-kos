# Киберимунный автономный квадрокоптер

Проект представляет собой набор инструментов и решений для создания киберимунного автономного квадрокоптера. Состоит из [ОрВД](orvd) и [наземной станции](planner), [прошивки для квадрокоптера](ardupilot), [симулятора](ardupilot), [модуля безопасности](kos), [документации](docs).

Содержание репозитория:

- [/ardupilot](ardupilot) - прошивка для квадрокоптера и симулятор
- [/docs](docs) - документация
- [/kos](kos) - модуль безопасности
- [/orvd](orvd) - Система организации воздушного движения
- [/planner](planner) - APM Planner 2 под Linux (наземная станция)
- [/tests](tests) - тесты проекта

## Квалификация

Читайте об условиях [квалификации в документе](docs/QUALIFICATION.md) - [Квалификация](docs/QUALIFICATION.md).

Отчёт о выполнении задания, вставлен код для вывода координат дрона из полётного контроллера.

```cpp
    int32_t lat, lon, alt;
    while (true) {
        if (!isDroneArmed()) {
            fprintf(stderr, "[%s] Drone is not armed, checking again in %ds\n",
                    ENTITY_NAME, RETRY_DELAY_SEC);
            break;
        }

        getCoords(lat, lon, alt);
        fprintf(stderr, "[%s] Drone is armed, this means it's flying. "
                        "Current coordinates: Latitude: %d, Longitude: %d, "
                        "Altitude: %d \n", ENTITY_NAME, lat, lon, alt);

        sleep(RETRY_DELAY_SEC);
    }
```

isDroneArmed() - функция проверки что дрон уже активирован и находится в состоянии полёта, так же проверяет случай когда полет будет отменён орвд.

```cpp
bool isDroneArmed() {
    char response[1024] = { 0 };
    sendSignedMessage("/api/fly_accept", response, "fly accept", RETRY_DELAY_SEC);
    return strstr(response, "$Arm: 0#") != NULL;
}
```

Скриншот с выполнением задачи:

Offline

![img](/docs/img/offline.png)

Online

![img](/docs/img/online.png)

## Документация

- [API](docs/API.md) - программный интерфейс модуля безопасности.
- [Архитектура](docs/ARCHITECTURE.md) - архитектурные диаграмы помогающие понять проект.
- [Разработка](docs/DEVELOPMENT.md) - документация для разработчика помогающая работать с проектом (установка, настройка, запуск).
- [Квалификация](docs/QUALIFICATION.md) - квалификационные задания для участников **Киберимунной автономности** на [Архипелаге 2024](https://xn--2035-43davo0a5a6bk9d.xn--p1ai/).
- [Инструменты](docs/TOOLS.md) - описание инструментов входящих в решение и полезные материалы по работе с ними.
