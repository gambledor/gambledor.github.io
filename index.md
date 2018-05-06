**(In Progress)**
# PHP Generators
Generators and coroutines have been around since PHP 5.5.
Even so, a lot of PHP developers do not know how to use them well
and do not recognise situations where they are helpful. In this post
I will share some use cases and insights.

## Generators
Generators are functions that provide a simpler way to loop through
data without the need to build an array in memory. It looks just
like a normal function, except that instead of returning a value,
it *yield*s as many values as it needs to in a sequential order,
emitted one by one.  In a few words they allow you to implement
iterators more easily without the need of implementing a class
that implements the Iterator interface.

When a generator function has been called, it returns a **Generator**
object which can be iterated over. As you can see in [doc](http://php.net/manual/en/class.generator.php)
the Generator class implements *Iterator*. Since it is an iterator
object it can be used in a *foreach* statement to iterate over a
collection of data in lazy mode (without the need to build the whole
array of data in memory); though not exceeding memory limit or
requiring any amount of processing time to generate it.
Let's make an example:

```php
function getRange(int $max = 10)
{
    for ($i = 0; $i < $max; $i++) {
        $value = $i * mt_rand();
        yield $i => $value ;
    }
}

foreach (getRange(PHP_INT_MAX) as $range => $value) {
    echo "Dataset {$range} has {$value} value" . PHP_EOL;
}
```
The advantage of this approach is to allow you to work with large
detests without loading them into memory all in a once.

## Coroutines
Coroutines add to the above generators the ability to send values to
them. This turn out a one-way communication from generators to caller
into a two-way communication by channel between them.
