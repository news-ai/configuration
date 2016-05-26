Create a file `restart_celery.sh` in `/home/api/`:

```
source /var/apps/news-discovery/env/bin/activate && cd /var/apps/news-discovery/news-discovery && echo yes | celery -A taskrunner purge && supervisorctl restart workers:celeryd1 workers:celeryd2
```

```
sudo crontab -e
```

```
0 */2 * * *  /home/api/restart_celery.sh
```
