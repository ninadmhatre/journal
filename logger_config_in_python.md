# Logger config in Python

We use logger config files to configure all the loggers in the project and recently i faced an issue at work where all the loggers in our projects were disabled and i had to add hack to forcefully enable. I googled but i could not find any solution so i left it with hack. 

After few days, i decided to revisit this issue and tracked all the places where logger is active and found 1 of the imports after which all loggers were getting disabled. Why? because this file was loading it's own logger config file and by default it disables all existing loggers.

```python
import logging.config

logger.config.fileConfig('logger.ini', disable_existing_loggers=False)  # added extra parameter
```

Ok, now the next question was why this module was having it's own logger config? Looking little more revealed that this was used as standalone script as well. I wanted to load the logger config when it's executed as standalone script and use main logger config so i made a simple fix which is used when we write any python app 

```python

if __name__ == '__main__':
    logging.config.fileConfig('logger.ini', disable_existing_loggers=False)

logger = logging.getLogger('my_app')
```

While using logger config in a project there should be just **one** logger config and all modules should use default/project wise config and if any custom logging is needed for any module then add that config in the logger config. 

Nice lesson learnt!
