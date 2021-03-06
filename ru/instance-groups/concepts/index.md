# Взаимосвязь ресурсов сервиса

Основная сущность, которой оперирует компонент [!KEYREF instance-groups-name] сервиса [!KEYREF compute-full-name], — _группа виртуальных машин_.

Каждая группа состоит из одной или нескольких однотипных виртуальных машин. Виртуальные машины группы могут находиться в разных зонах и регионах доступности. [Подробнее о географии Облака](../../overview/concepts/geo-scope.md).

При создании группы необходимо описать:

- [[!TITLE]](instance-group-instance-template.md) — указать нужное количество ядер процессора (vCPU) и памяти (RAM).
- [[!TITLE]](instance-group-policies.md) — указать тип создаваемой группы, зоны доступности и параметры развертывания.

По шаблону будут развертываться все виртуальные машины группы. Список доступных типов групп см. в разделе [[!TITLE]](instance-group-types.md).

Созданная в каталоге группа ВМ доступна по сети для всех виртуальных машин, подключенных к этой же облачной сети. [Подробнее о работе сети](../../vpc/).
