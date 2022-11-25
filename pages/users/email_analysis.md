# Users' emails
Data analysis about Users email directions

# Overall

```email_data
select count(distinct email) as total_emails, avg(length(email)) as avg_length,(select count(distinct substring(email,charindex('.',email),10))from users) as distinct_domains
from users
```

<BigValue data={email_data} value=total_emails title='Total email directions'/>
<BigValue data={email_data} value=distinct_domains title='Total domains'/>

# Most common domains

```domain_data
select top 12 substring(email,charindex('.',email),10) as domain, count(*) as amount //10 set as max domain characters
from users
group by domain
order by amount desc
```

<BarChart data = {domain_data} x=domain y=amount sort=amount title = 'Top 12 most common domains among users'/>

<BigValue data={domain_data} value=domain title='domain'/>
<BigValue data={domain_data} value=max(domain) title='max(domain)'/>

# Email length

Let's keep going a bit deeper about email analysis, this time about treatment of character lengths. Excluding @ and . characters as non-countable.

```length_data
select length(email)-2 as email_length, count(*) as amount
from users
group by email_length
order by email_length asc
```

<LineChart data={length_data}  x=email_length y=amount/>