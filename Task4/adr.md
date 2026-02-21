### Название задачи
Передача ставок в кол-центр

### Автор
Никита Щербо

### Дата
03-06-2025

### Функциональные требования

| **№** | **Действующие лица или системы**               | **Use Case**               | **Описание**                                                                        |
|:-----:|------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------|
|   1   | Менеджер кол-центра, Сервис ставок             | Просмотр актуальных ставок | Сотрудник видит действующие ставки через внутренний интерфейс                       |
|   2   | Менеджер партнёрского кол-центра, Файл XML/CSV | Получение ставок по файлу  | Партнёрский кол-центр загружает ставки в виде файла                                 |
|   3   | Сотрудник бек-офиса, Сервис ставок             | Обновление ставок          | Менеджер вручную обновляет данные ставок через интерфейс или импортирует Excel      |
|   4   | Сервис заявок                                  | Использование ставок       | Использует ставки из общей базы для отображения и расчёта условий при подаче заявки |

### Нефункциональные требования

| **№** | **Требование**                                                                |
|:-----:|-------------------------------------------------------------------------------|
|   1   | Разделение доступа: партнёрский кол-центр не должен видеть внутренние системы |
|   2   | Формат выгрузки — файл CSV/XML, регулярная генерация                          |
|   3   | Минимальное изменение текущего процесса (всё ещё Excel-основа в MVP)          |
|   4   | Возможность расширения до API в будущем                                       |
|   5   | Лёгкая интеграция со сторонними системами                                     |

### Решение

В рамках MVP создаётся отдельный Сервис ставок, который агрегирует данные ставок, предоставляет их по внутреннему API и формирует регулярные выгрузки для партнёрского кол-центра.

#### Диаграмма контекста

[Исходник](./context.puml)  

![Изображение](./context.png)

#### Диаграмма компонентов

[Исходник](./container.puml)  

![Изображение](./container.png)

**Аргументация выбора:**
- Централизация данных ставок упрощает поддержку и контроль
- Партнёры получают только то, что им нужно, без доступа к внутренним системам
- Снижается нагрузка на бек-офис за счёт единообразного интерфейса ставок
- Возможность развивать автоматизацию без смены модели

### Альтернативы

- **Прямая передача Excel-файла партнёрам** — отклонено из-за проблем с безопасностью и масштабируемостью.
- **Интеграция через API** — невозможна из-за ограничений на стороне партнёрского кол-центра (нельзя использовать SFTP или REST).

### Недостатки, ограничения, риски

- Обновление ставок по-прежнему происходит вручную -> риск несвоевременности данных
- Выгрузка файла требует мониторинга (по расписанию)
- Необходимо разграничивать доступ для разных типов кол-центров
- Требуется организационная договорённость о формате передачи ставок


# Список крупных задач

|        **ID**         | **Название задачи**                                   | **Квартал** | **Описание**                                                        |
|:---------------------:|-------------------------------------------------------|-------------|---------------------------------------------------------------------|
|     Сервис ставок     |
|          T1           | Проектирование API                                    | Q3          | Проектирование модели ставок и интерфейсов                          |
|          T2           | Реализация UI                                         | Q3          | Интерфейс для обновления и просмотра ставок                         |
|          T3           | Экспорт в CSV/XML                                     | Q3          | Формирование файлов для передачи партнёру                           |
|          T4           | Cron-выгрузки                                         | Q3          | Автоматизация экспорта на уровне расписания                         |
|          T5           | Права доступа                                         | Q4          | Разделение доступа между внешними и внутренними пользователями      |
|          T6           | Стабилизация и тестирование сервиса ставок            | Q4          | Завершение работ, стабилизация, проверка корректности и мониторинг  |
|       Кол-центр       |                                                       
|          T7           | Интеграция с API ставок                               | Q3          | Подключение к сервису ставок                                        |
|          T8           | Обновление UI                                         | Q3          | Отображение ставок в интерфейсе операторов                          |
|          T9           | Стабилизация, обратная связь по кол-центру            | Q4          | Проверка, сбор обратной связи от операторов                         |
| Партнерский кол-центр |                                                       |
|          T10          | Разработка и согласование формата                     | Q3          | Утверждение структуры и формата выгрузки (CSV/XML)                  |
|          T11          | Автоматизация импорта                                 | Q4          | Обработка выгрузок и загрузка на стороне партнёра                   |
|          T12          | Стабилизация, тестирование по партнерскому кол-центру | Q4          | Проверка на корректность, обратная связь, отладка процессов         |
|     Интернет-банк     |                                                       
|          T13          | Доработки для поддержки логики ставок                 | Q3          | Обновление отображения персонализированных ставок при необходимости |
|         Сайт          | 
|          T14          | Отображение доступных публичных ставок                | Q3          | Добавление блока ставок на сайт                                     |




# Roadmap

Сделан с использованием практик диаграммы Ганта - ведь некоторые задачи зависят друг от друга

[Roadmap](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=RoadMap_bank_Standart.drawio&dark=auto#R%3Cmxfile%20pages%3D%222%22%3E%3Cdiagram%20id%3D%221zcTAVt1k4KSup7FvAfL%22%20name%3D%22Roadmap%22%3E7VzZcts2FP0azbQPzIALuDxqdbqkjcdqmz51GImSOKVIhaIjO19f7LokIEd2IsZ1YE9o4AKESJyDuwHKwB9v767qdLd5Uy2zYuCh5d3Anww8D%2FshIn%2Bo5J5L%2FBgnXLKu8yWXuUfBTf4pE0Jx4%2Fo2X2b7Vsemqoom37WFi6oss0XTkqV1XR3a3VZV0f7UXbrONMHNIi106V%2F5stmI1%2FAROja8zvL1Rnw0aRIvuE1lb9F1v0mX1QGI%2FOnAH9dV1fDS9m6cFXT65MT4k2g2ufLSd8PkKnTjf36pyrXDB5s95hb1DnVWNk8e%2BnBVV6PXrndzHzm%2Fr0f739DqZ8fjQ39Mi1sxYeJdm3s5g%2BS1d7RYM3hGq7woxlVR1azVX61W3mJB5Pumrv7NZEtZlRntXJWN4IQXkPqm2Rak7JLiYZM32c0uXdDGA2EfkaVFvi5JtchW9KM%2BZnWTEyiHQtxUtNOe3JOXa1LHpFZXt%2BUyo6%2BISO3MiRITSsfP7gBPxMRdZdU2a%2Bp70kW0Oh6WhBELwUl8ITgcaeWGkvYbQCk3FsJUcHmtxj%2FCRQoCsUegF1j0noie6wbhq%2FhcABPV96tDiDUI5%2BRlkEP%2BDcikJLMBme%2BYlUdTdsXsOmRyj5VjrQ%2BXc8lE9mH3emj49qdHsyQLDSwhLcsoeY9Qmypu%2FL%2BmSpBoVDGs9AgbeOJfap1H%2BjonszB0JR8U%2BpAPLqMQKxypArtNBZG6ZGjPtAk%2BgG6XKpj%2BCkIAOf8xUShkPwYKXQ7i0D9TG3g9K4PYiHMyBHCNHIZmCFTBROLLaGDRNFpmI5QXM8yJBuS1T%2BrkF5%2FUvfsmo2jssjonD5HVQvT2WP88eneZdMFpfS%2BK6AFkz0eQ1cVju0CHLwii7OnaTDtFmT6NvBcYNHdsYEIUvAq%2BnAqJc%2F0p%2B3C4%2Bns%2FL5G3K67nHyPhlL8wH02D5VxEHwNf1K%2BHbUTP1dETXhk3ptzyKvM6aVtkbny5t6Z8sES4dqOXo9YvQQdNrRvpcDG1bmTDi4yWe1nL%2BBmsZd%2B8luP2aut41HBFew6ruFpw9cx96F6WpxHhfpfni0yH9LI8%2FWewPPVMCAhykTCdfJHa1WYErN%2FVFmp4XQc2xgExzgWI4Ibxc4xx9PzU3ANJzCECbi0vj4BDHAFnOgQJrBm5%2FY8Xk6u8hFrQc5UoMbDhUrlKIxf0HNbch1yQHtMxOJrJOAgEU55MVHpofPMneZB3b361TDitF5IYd5mAdSYEIe6RCXoSbB4IJozrqnRkKpoAztWBDyjgA9Ugo2eL%2F2Pwd781%2FvIBIAHw4MTeFtyYQHL5B0AzuCAd4gO9QTqH6ZbOfPl%2Bv2OTHBYNB6Js8SX8cEu36kcLzgQStKF6%2FT79gYxAXhB1%2FvzIhkKUIs4q3ebFPb9lW5XVnvNCtXMnhLai3R2XE4Qbh%2FLG2VZL0VhWgkjHZyGlNf%2BLu6cwMJlqKmUHDFRNTj1mk08kE1qmnh%2Bmk40JYp%2Fr66q%2BkndPGsY7DsMRVi1U4WOGM2YjTuhlOJElGi7jCX0KukclZDNVmqpWpEpea6gRrzLDgIVpkI%2FC4VaPwlUErTIlQQpATcheUlXQVqgsVLtQGOxuqTJoI1EaVMbUBquzKlAdVMgwp3KuPqiIKRAqaqsQ2kSVCHsOoUaoDDOJVCVUwp8FAMBVyjkQqyaF7lGfYKpRVE%2BhVeRA97CFWRfZwjWMamVaRrYJTaMafXAf0ThKTrUO8BVRyziIFU20EF%2FUcs1Ye3B%2BnGDaxfajPj1DV0%2Bmz0PoGuob1S4IE84NGaSzgLSMHhz%2FUWcmrIGxBsYaGGtgrIFRBgZ7XQNjSj2ovGU%2FBsbTDAxD3Iu4lemo8ZObPzAUfcDMiDNW7Dzd005aWT6dTmUlhvi130yWa94wHHrxSTpNgdcy0XyLUXt3v3Uq0yY6H2KHSmZJdsThN2eHvts4T766Nzv%2Bqn7sWLrHiqcdJQdPsaCuqjsazJl8bFIeW%2FfYusfWPbbusTVYD7nHscGd6dk7PnHAgn1Vr7tXi4CGR5oJmwJjMWyn7OO2vZhKb1qZPL31oS%2BrsM5wG4B3G2s2C7U7q%2FMi6nPlZ2nWypL4bJ88NIR4xhxieDES66dO5m7rC1NQc8HoS6fM2anE4%2B3tTWrBO%2BvuWHfHujvW3fl%2BLYW%2B3RRGBkuBDacPLmcpDIfSaILQBujWYlmLZS2WtVjftcXC3RP1IdItVs8Buvnr3yRAD4DZagXfRjNBrp3vEXZCanTeybqJPJpJrurGGejmtg0lkUcnbrR7Y18Sh7vdONw3nu3sNQ43HO514TnvRE%2FaPJxBitvUHLWD8BngSgCu8Kt6oWk0mPzxwUqwZ4o%2FT7vYe9U5VRyYDpVj1e0LqUeqx%2F9Ni7UBd8%2Bf%2Fgc%3D%3C%2Fdiagram%3E%3Cdiagram%20id%3D%226faDem5PxRRIMGQd80wj%22%20name%3D%22Roadmap_change%22%3E7Zpfb9o6FMA%2FTR6Z%2FCcJ5BEobHuY1Km9mnTfUmIgWohRSAvdp5%2FtOOYkNi3cy7YwhRbLPj62g%2F1zzjlOPDrdHD4W8Xb9hScs8whaFWni0TuPECy%2BQrCNV6whkBoP6Y9aiLT0OU3YrqFYcp6V6bYpXPA8Z4uyIYuLgu%2Bbakue2ZfxsIgzZkm%2FpUm5rqSUInSs%2BMTS1VqPJKqiqmYT19padbeOE74HIjrzSOgReog9OvGkrPlPpwXn5btqtfLmMGWZnNp6Yqtx5%2F%2B9A%2FPjC5aXV%2Brza7afIPQDDXj5hILwe%2FLPt38HWM%2FZS5w964nXc1a%2B1ishpm8rs4Va1ckyzbIpz3ihaulyuSSLhZDvyoJ%2FZ3VNznMmlXleapSIL8rrcpOJPBbZ%2FTot2cM2XsjKvSBUyOIsXeWimLGlHOqFFWUqkBhrccml0k60SfOVKAeiVPDnPGHyNyI1IerXiHbscHIq8TkMtGfzI%2BMbVhavoqx7HpCghvFVSwKqBfsjsjisOVwDXHGkhbHeFivT%2F0XLLHT1Sl8ZjFEPxhXBGJ4PxqjjYAx7MK4IRuQEA98gGLTn4opcYOyHH0bnohEZ3Y7C4VtwPIq5QwPxHd9%2Flj2JlRxhT0z9JFB5pPJEpSNQq4bVQpHeqSpSNxQpBc2rbkOgP6qb%2B6DJXKUzlQ7BiATIiW4YVam6mDG%2BGHEWOhAXNckwekKoybmwwX8b5%2F7I4rw2goByGjkgD7p9%2BwtswokmXAEzt9gOAIcVqENAuCiaKpFGKp04gJRbQLd6mD%2FeD97cSbWkK9guBGKsuAFwDaYGXOTb4AaBA9yw2%2BCGtt123ObCTHKRpC8iu5JZXfH2DfuP3aqryxVTBq%2B4DX2TJxemgOL2lgjknwYfyKuPa6uE6uPYKr8X5JC2QT7haRAHyt13NOzoRN4U%2B6V3Lr0z%2BHCue8djD%2Fuw4uvp6GNXMrnAW1ak4jdI66NE98fy%2B0AcWH1CKss7nUVvwHI%2BFKqsLxu7bGUT3lMU%2FukAhjjsozt8Gfof%2FE7jhbHt3Al%2F7%2BjcEWWDJsACzrTlIiayMUYKWWrj2tJB6zZRTabA0s1AQ2QZ00p%2FBkaEkVNLeaovWI87bo4r0wGwx0HTfgOb3fuRl%2B0TPApb%2ByR0bJPId%2BySjsc%2FmNh7xPesAAiyTUHQYxAz7l9wyd6B%2FbQCLOERmDOG%2F%2B2LdjT6vxX8CWrjPyQ2%2FuaQ4IaiKGwffz6GV8H%2FnFMAHWxVri4MmqqR5rVwBqxMYAFvtpWRt04a5kAZNrlzdNhvjQstQ9DeGv7Q3hrhLVoG%2B%2FAXHB684aHA096reSh95Oedeh5JHGx1PfLD9rnrJXCF8UYuef602%2BqrueA%2BZ7xsCCfV%2BZ7bX8Gt8wULJ7ddf7%2FCPnZ9pMBfGBvDbMK%2F5gn%2BkRsK6JlCL6B%2FDvALHmBZD%2FCxTSTphAOrdN95kU33B14QpLOf%3C%2Fdiagram%3E%3C%2Fmxfile%3E)
