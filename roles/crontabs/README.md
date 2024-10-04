# os-crontabs

Definieer een crontab:

```
crontabs:
    daily_restart:
        user: sidadm
        name: "Daily restart BODS omgeving"
        minute: "00"
        hour: "22"
        job: /apps/SAP/deploytomcat.sh
```

maak alleen een cron.allow met een user aan
```
os:
    crontabs:
        allow_crontab_entry:
        user: sapadm 
```            