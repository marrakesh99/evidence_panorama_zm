---
title: Panorama de zonas metropolitanas
---


```sql zonas
select distinct zona_metropolitana
from 'datos_zm'
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
from 'datos_zm'
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
  zona_metropolitana as "Zona Metropolitana",

  sum(primer_ingreso) filter (where ciclo = 2020) as "2020",
  sum(primer_ingreso) filter (where ciclo = 2021) as "2021",
  sum(primer_ingreso) filter (where ciclo = 2022) as "2022",
  sum(primer_ingreso) filter (where ciclo = 2023) as "2023",
  sum(primer_ingreso) filter (where ciclo = 2024) as "2024"

from datos_zm
where lower(control) != 'NULL'
  and ciclo between 2020 and 2024
group by zona_metropolitana
order by zona_metropolitana
```
<DataTable data={matxzonas} search=True 
columns={['zona_metropolitana', '2020', '2021', '2022', '2023', '2024']}/>
