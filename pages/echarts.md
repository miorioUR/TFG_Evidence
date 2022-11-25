# Welcome to Evidence! 👋
Build a polished business intelligence system using only SQL & Markdown.

# Write Markdown
Evidence creates pages from markdown files. 


```pie_query
select 'Apple' as pie, 60 as count
union all
select 'Blueberry' as pie, 70 as count
union all
select 'Cherry' as pie, 40 as count
union all
select 'Pecan' as pie, 35 as count
```

```pie_data
select pie as name, count as value
from ${pie_query}
```

<ECharts config={
    {
        tooltip: {
            formatter: '{b}: {c} ({d}%)'
        },
        series: [
        {
          type: 'pie',
          data: pie_data,
        }
      ]
      }
    }
/>