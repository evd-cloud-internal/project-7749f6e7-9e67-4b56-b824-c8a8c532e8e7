---
name: Home
assetId: d4b4f7f1-f3ea-4916-b609-995fe9fed3cd
type: page
---

# Informe 9BOX Calibrado 2025

## Distribución Global (Nivel Organizacional)

```sql dist_global
SELECT
    "9BOX CALIBRADO 2025" as caja,
    count(*) as cantidad
FROM archivo_master_desempeno9boxcompromiso_base_informe
WHERE "9BOX CALIBRADO 2025" NOT IN ('', 'No aplica', 'Pendiente', 'No coincidente')
GROUP BY "9BOX CALIBRADO 2025"
ORDER BY cantidad DESC
```

{% bar_chart
    data="dist_global"
    x="caja"
    y="cantidad"
    stacked="100%"
    series="caja"
    title="Distribución % por Caja 9BOX - Nivel Organizacional"
    y_fmt="pct"
    x_sort="desc"
    data_labels={
        position="middle"
        fmt="pct1"
    }
/%}

## Comparativo por Vicepresidencia (Todas)

```sql dist_todas_vp
SELECT
    VICEPRESIDENCIA as vp,
    "9BOX CALIBRADO 2025" as caja,
    count(*) as cantidad
FROM archivo_master_desempeno9boxcompromiso_base_informe
WHERE "9BOX CALIBRADO 2025" NOT IN ('', 'No aplica', 'Pendiente', 'No coincidente')
  AND VICEPRESIDENCIA != ''
GROUP BY VICEPRESIDENCIA, "9BOX CALIBRADO 2025"
ORDER BY VICEPRESIDENCIA, cantidad DESC
```

{% bar_chart
    data="dist_todas_vp"
    x="vp"
    y="cantidad"
    series="caja"
    stacked="100%"
    title="Distribución % por Caja 9BOX - Todas las Vicepresidencias"
    y_fmt="pct"
    x_axis_options={
        label_rotate=-45
        max_label_length=30
    }
    series_order=["Top", "Experto", "Clave", "Sólido", "Potencial", "Prometedor", "Emergente", "Inconsistente"]
    height=500
/%}

## Detalle por Vicepresidencia (Filtro)

{% dropdown
    id="vp_filter"
    data="archivo_master_desempeno9boxcompromiso_base_informe"
    value_column="VICEPRESIDENCIA"
    title="Seleccione una Vicepresidencia"
    where="VICEPRESIDENCIA != ''"
    order="VICEPRESIDENCIA"
/%}

```sql dist_vp
SELECT
    VICEPRESIDENCIA as vp,
    "9BOX CALIBRADO 2025" as caja,
    count(*) as cantidad
FROM archivo_master_desempeno9boxcompromiso_base_informe
WHERE "9BOX CALIBRADO 2025" NOT IN ('', 'No aplica', 'Pendiente', 'No coincidente')
  AND VICEPRESIDENCIA != ''
  AND VICEPRESIDENCIA = {{vp_filter}}
GROUP BY VICEPRESIDENCIA, "9BOX CALIBRADO 2025"
ORDER BY cantidad DESC
```

{% bar_chart
    data="dist_vp"
    x="caja"
    y="cantidad"
    stacked="100%"
    series="caja"
    title="Distribución % por Caja 9BOX - {{vp_filter.literal}}"
    y_fmt="pct"
    x_sort="desc"
    data_labels={
        position="middle"
        fmt="pct1"
    }
/%}
