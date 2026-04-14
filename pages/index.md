---
title: Panorama de zonas metropolitanas
---


```sql zonas
select distinct zona_metropolitana
from datos_zm
order by zona_metropolitana
```
<Dropdown data={zonas} name=zona_metropolitana value=zona_metropolitana>
    <DropdownOption value="%" valueLabel="NACIONAL"/>
</Dropdown>


```sql ingresos
select
    ciclo,
    control,
    sum(primer_ingreso) as total
from datos_zm
where zona_metropolitana like '${inputs.zona_metropolitana.value}'
and control != 'NULL'
group by ciclo, control
order by ciclo
```

<LineChart
    data={ingresos}
    x=ciclo
    y=total
    series=control
/>

```sql matxzonas
select
  zona_metropolitana as zona_metropolitana,

  sum(primer_ingreso) filter (where ciclo = 2020) as "2020",
  sum(primer_ingreso) filter (where ciclo = 2021) as "2021",
  sum(primer_ingreso) filter (where ciclo = 2022) as "2022",
  sum(primer_ingreso) filter (where ciclo = 2023) as "2023",
  sum(primer_ingreso) filter (where ciclo = 2024) as "2024"

from datos_zm
where lower(control) != 'null'
  and ciclo between 2020 and 2024
group by zona_metropolitana
order by zona_metropolitana
```
<DataTable data={matxzonas} search={true}>
  <Column id="zona_metropolitana" title="Zona Metropolitana" />
  <Column id="2020" />
  <Column id="2021" />
  <Column id="2022" />
  <Column id="2023" />
  <Column id="2024" />
</DataTable>
