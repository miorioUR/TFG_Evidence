# Users
Data analysis about Users distribution among gender and age

# Gender distribution

```male_users_data
select 
  count(*) as n_male_users,
  avg(age) as average_male_age,
  (select top 1 first_name from users where gender = 'Male' group by first_name order by count(*) desc) as most_common_male_name
from users
where gender = 'Male'
```

```female_users_data
select 
  count(*) as n_female_users,
  avg(age) as average_female_age,
  (select top 1 first_name from users where gender = 'Female' group by first_name order by count(*) desc) as most_common_female_name 
from users
where gender = 'Female'
```
<BigValue data={male_users_data} value=n_male_users title='Number of male users'/>
<BigValue data={female_users_data} value=n_female_users title='Number of female users'/>

```pie_query
select 'Male' as pie, <Value data={male_users_data} column=n_male_users/> as count
union all
select 'Female' as pie, <Value data={female_users_data} column=n_female_users/> as count
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

# Age on Female users

Analysing the age distribution and most common name among <Value data={female_users_data} column=n_female_users/> female users

<BigValue data={female_users_data} value=average_female_age title='Average age among female users'/>
<BigValue data={female_users_data} value=most_common_female_name title='Most common female name'/>

```female_users
select
  concat(first_name,' ',last_name) as nombre,
  email as correo_electronico,
  age as edad,
  concat((floor(edad/10))*10,'-',((floor(edad/10))*10)+9) as grupo_edad
from users
where gender = 'Female'

order by 1 asc
```

```female_users_age
select
  concat((floor(age/10))*10,'-',((floor(age/10))*10)+9) as grupo_edad, count(*) as value 
from users
where gender = 'Female'

group by grupo_edad
order by grupo_edad asc
```

<BarChart data = {female_users_age} x=grupo_edad y=value sort=grupo_edad title = 'Female distribution by age group' />

# Focusing on Male users

Analysing the age distribution and most common name among <Value data={male_users_data} column=n_male_users/> male users

<BigValue data={male_users_data} value=average_male_age title='Average age among male users'/>
<BigValue data={male_users_data} value=most_common_male_name title='Most common male name'/>

```male_users
select
  concat(first_name,' ',last_name) as nombre,
  email as correo_electronico,
  age as edad,
  concat((floor(edad/10))*10,'-',((floor(edad/10))*10)+9) as grupo_edad
from users
where gender = 'Male'

order by 1 asc
```

```male_users_age
select
  concat((floor(age/10))*10,'-',((floor(age/10))*10)+9) as grupo_edad, count(*) as value 
from users
where gender = 'Male'

group by grupo_edad
order by grupo_edad asc
```

<BarChart data = {male_users_age} x=grupo_edad y=value sort=grupo_edad title = 'Male distribution by age group' />

# Age group distribution by gender (unified)

```users_age
select
  concat((floor(age/10))*10,'-',((floor(age/10))*10)+9) as grupo_edad,
  gender,
  count(*) as valor
from users

group by grupo_edad,gender
order by grupo_edad asc
```

<BarChart 
    data={users_age} 
    x=grupo_edad 
    y=valor 
    series=gender
    sort=grupo_edad
/>

# Age group distribution by gender (comparative)

```users_age_distribution
select age, gender, count(*) as amount
from users

group by age,gender
```

<LineChart 
    data={users_age_distribution} 
    x=age 
    y=amount 
    series=gender 
    yAxisTitle="Age" 
    xAxisTitle="Number of users"
/>

<BarChart 
    data={users_age} 
    x=grupo_edad 
    y=valor 
    series=gender
    sort=grupo_edad
    type=grouped
/>
