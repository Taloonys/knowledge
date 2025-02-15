> Доступность описывает способность программной системы поддерживать свою работоспособность и быть доступной для пользователей, даже при возникновении сбоев или нарушений в работе.

Для оценки доступности часто используются такие показатели, как **время безотказной работы системы (uptime)**, показывающее процент времени нормальной работы системы, и **время простоя (downtime)**, отражающее периоды, когда система недоступна.

В области системного проектирования для улучшения доступности используются множество методов и стратегий, включая:

- [Балансировка нагрузки](load-balancer.md)
- создание кластеров
- [Репликацию данных](replication.md)
- резервное копирование и восстановление
- непрерывный мониторинг

Эти стратегии реализуются с целью уменьшения вероятности возникновения единственных точек сбоя, быстрого обнаружения и устранения последствий отказов, а также обеспечения бесперебойной работы системы даже при возникновении сбоев или других проблем.

![Untitled](image-storage/Untitled%204.png)

# Service level objectives (SLO)
> Это соглашение между поставщиком и клиентом в рамках какого-то **конкретного показателя**. Или скорее обещание поставщика, что, например, какая-то транзакция всегда будет укладываться по времени в 5 секунд, условно.

# Service level agreements (SLA)
> Это соглашение между поставщиком и клиентом об уровне обслуживания, заключается юридически, если сервис не соответствует предъявленным требования по времени безотказной работы, то компания-поставщик сервиса должна как-то это компенсировать.

#  Service level indicator (SLI) 
> Индикатор уровня обслуживания сервиса, по факту это фактическое измерение доступности сервиса или индикатор для сравнения с обещанными показателями **SLA**.