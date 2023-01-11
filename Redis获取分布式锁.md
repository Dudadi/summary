### redis获取分布式锁
为了保证读和写操作的原子化，将并行的操作序列化。通常针对单机操作，可以通过原子化操作`compareAndSet`或者加锁来实现。而针对分布式集群，需要通过分布式锁来保证并行操作序列化。
redis可以用于设置分布式锁，设置分布式锁本质需要原子化操作。
```bash
setnx key value
expire key time
```
`setnx`的意思是`set if not exit`，将判断是否存在和`set`两个操作设置为一个原子化操作。
因此流程是一个端成功设置值，然后获取到锁，然后超期或者自动释放锁。在超期和自动释放锁之前，其他端`setnx`会返回`0`，获取锁失败。
	
由于存在`setnx`成功`expire`失败的场景，可能会导致获取锁的端一旦死机，其他所有端无法再获取锁了。
因此需要将`setnx`和`expire`设置成一个原子化操作。`redis`扩展了`set`命令。通过如下指令可以实现
```bash
  set key value ex time nx
```
当然可以通过`multi`指令来实现`transaction`操作。
