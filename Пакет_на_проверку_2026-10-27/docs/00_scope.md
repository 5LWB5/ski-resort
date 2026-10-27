# Travel Designer resort ranking

## Decisions recorded

- Build the analytical dataset from open sources from scratch. The sibling student package files are examples and are not source data for the analysis.
- The brief initially listed 32 resorts. Per the user, exclude КолаСпортЛэнд (reported no longer operating) and Гладенькая (reported temporarily closed), leaving 30 active resorts. Keep excluded rows in the roster for traceability. Recollect all attributes and metrics from fresh sources; do not reuse the example data files.
- The ranking is used by the trip builder to help select a ski resort for the chosen travel dates and preferences.
- Selected activity types are required criteria. A resort matching at least one selected activity is eligible; more matches rank it higher.
- Skill levels are beginner, intermediate, and expert. Data should be collected from resort websites and other open sources; the matching rule will be agreed later.
- Use the live weather forecast when one is available for the selected dates. Outside the forecast horizon, use the 2016–2025 monthly ERA5 historical estimate and label it separately; this is not a forecast for the future year.
- Start with a simpler first version; do not build a separate advanced freeride safety model in this phase.
- Planned weather refresh cadence: up to six times per day, subject to the chosen API's actual update schedule and terms. Resort characteristics are refreshed less often, after material changes.
- For the prototype, use general resort/locality geolocation rather than individual slope locations. Coordinate candidates must be checked against the resort name and region and remain labeled approximate.
- The current Open-Meteo integration is for a prototype/demo only. Before production use, confirm the commercial licence or replace the provider; retain required attribution.

## Open decisions

- How to map trail difficulty and resort information to beginner, intermediate, and expert levels.
- Exact fallback weather statistic and how the UI should distinguish an estimate from a forecast.
- Whether checked lift types follow the same eligibility/match-count rule as activities.

## Data handling

- Keep unmodified API responses in a raw layer with request URL, retrieval timestamp, and source.
- Store cleaned and standardized tables separately from raw responses.
- Never describe a settlement-centre weather point as the exact weather at a ski slope. Use approximate geolocation of the resort/locality for the prototype weather point; exact slope coordinates are out of scope. Store the location source and label coordinates approximate. For production calculations, confirm the weather provider terms and assess whether locality-level weather is sufficiently representative.




## Обязательные цены для конструктора поездки

По уточнению пользователя, сбор цен обязателен. Нужно заносить опубликованные тарифы:
- ски-пассы — все доступные типы и периоды действия, категории посетителей, зоны/сектора и условия;
- прокат — категории оборудования и тарифные периоды (например, час/день), комплектность и условия, если опубликованы;
- проживание — если объект расположен на территории курорта; фиксировать тип размещения/номера и условия тарифа;
- трансфер — если предоставляет курорт, записывать как услугу курорта. Если услугу оказывает сторонняя организация, указывать отдельно как внешний трансфер, с оператором и источником; не представлять его цену как тариф курорта.

Для каждой цены записывать валюту, единицу/период тарифа, сезон или даты действия, категорию гостя/услуги, URL и дату проверки. Не сравнивать цены без одинаковой единицы и сопоставимых условий. Это справочные опубликованные цены на дату проверки, а не гарантия стоимости будущей поездки. Использовать отдельную `resort_prices.csv`, связанную с курортом через `resort_id`; пустое значение означает, что цена не собрана.

## Последние решения пользователя по справочным данным

Все предоставленные пользователем схемы и описания трасс принимаются как актуальные и проверенные для прототипа. Для цен и других новых данных применяются источник и дата проверки, но отдельно перепроверять эти уже предоставленные схемы в рамках текущего прохода не требуется.

Координаты из присланных пользователем ссылок Яндекс.Карт считаются проверенными пользователем на уровне геолокации курорта. Они всё равно обозначают территорию курорта, а не конкретные склоны. Координатные кандидаты, которые подобрал аналитик через Open-Meteo/OSM и пользователь не присылал, остаются кандидатами.

Приоритет сбора — заполненные поля, необходимые для выбора курорта: геолокация, погода (отдельно через API), категории трасс и активности, а также обязательные цены. Километраж трасс — только дополнительная справка; прочие необязательные характеристики можно собирать позже.


## Использование цен прошлого сезона в прототипе

Если прайс текущего сезона не найден, использовать последний найденный опубликованный тариф как рабочую актуальную цену прототипа. Исходный сезон и условия источника сохранять в season/
otes; не переписывать исторический сезон на текущий. Сбор цен продолжается по каждому из 30 активных курортов.
