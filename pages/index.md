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
  cast(coalesce(nullif(trim(zona_metropolitana), ''), 'Sin zona') as varchar) as zona_metropolitana,
  sum(primer_ingreso) filter (where ciclo = 2020) as y2020,
  sum(primer_ingreso) filter (where ciclo = 2021) as y2021,
  sum(primer_ingreso) filter (where ciclo = 2022) as y2022,
  sum(primer_ingreso) filter (where ciclo = 2023) as y2023,
  sum(primer_ingreso) filter (where ciclo = 2024) as y2024
from datos_zm
where control is not null
  and lower(control) != 'null'
  and ciclo between 2020 and 2024
group by 1
order by 1
```
<DataTable data={matxzonas} search={true}>
  <Column id="zona_metropolitana" title="Zona Metropolitana" />
  <Column id="y2020" title="2020" />
  <Column id="y2021" title="2021" />
  <Column id="y2022" title="2022" />
  <Column id="y2023" title="2023" />
  <Column id="y2024" title="2024" />
</DataTable>
