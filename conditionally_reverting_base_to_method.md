# Calling base implemtation of overridden method

Have you ever faced a situation where you want to alter the behaviour of method in a such a way that it's same as parent implementation?

```python

class Base:
    def ping():
        print("ping")
        
class SlowPing(Base):
    def ping():
        time.sleep(1)   # wait before ping
        print("ping")
        
sping = SlowPing()

for i in range(5):
    sping.ping()
```

Conside above code, in this SlowPing waits for a second before printing "ping" conside this "ping" as a method which is communicating with some server which accepts messages only after a delay or can not accept messages sent in quick succession. This would work fine for most of the cases but what if you want to test that server breaks when you send to many messages? or in other words, call `Base` method with `SlowPing` instance which has already overriddent behaviour?

## Use Super()

I initially thought of using `mock` framework for this but then i thought why not use some simple `lambda` to alter the behaviour of the instance and i was successful after few minutes of trial and error i was ready with following code

```python

sping = SlowPing()

sping.ping = lambda: super(type(sping), sping).ping

for i in range(5):
    sping.ping()

```

There is nothing fancy about above code but calling Base implementation with `super()` in Python made is super simple :) Honestly, i dont know how i would have got the exepected result without use of `super()`
