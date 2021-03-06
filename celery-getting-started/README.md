#Celery Beat
###Celery beat without django guide

**Install Celery Beat**

```sudo pip install celery```

**Install RAbbitMQ broker**

```sudo apt-get install rabbitmq-server```

**Create a celeryconfig.py**

Use RabbitMQ for the broker
```
BROKER_URL = 'amqp://localhost//'
CELERY_RESULT_BACKEND = 'db+postgresql://localhost/<db for messages. Create this on your own>'
```

**Create some tasks (save to tasks.py)**

```
from celery import Celery

celery = Celery('tasks')
celery.config_from_project('celeryconfig')

@celery.task
def add(x, y):
    return x + y
```

**Update your celeryconfig.py**

```
from datetime import timedelta

CELERYBEAT_SCHEDULE = {
    'every-second': {
        'task': 'tasks.add',
        'schedule':timedelta(seconds=1),
        'args':(51,52),
    },
}
```
*'every-second' is the name of the schedule task*

*'task' is the task found in tasks.py*

*'schedule' is to schedule how often the task is supposed to run*

*'args' are the positional arguments you pass to the task*

**Start Celery through the terminal**

```
celery -A tasks worker --loglevel=info --beat
```
*-A is the app instance to use*
