---
title:   Date and Time
isChild: true
anchor:  date_and_time
---

## Date and Time {#date_and_time_title}

PHP has a class named DateTimeImmutable to help you when reading, writing, comparing or calculating with date and time. There
are many date and time related functions in PHP besides DateTimeImmutable, but it provides nice object-oriented interface to
most common uses. DateTimeImmutable can handle time zones, but that is outside the scope of this short introduction.

To start working with DateTimeImmutable, convert a raw date and time string to an object with the `createFromFormat()` factory method
or do `new DateTimeImmutable` to get the current date and time. Use the `format()` method to convert a DateTimeImmutable to a string for
output. These options are described in more detail in the [Official docs](https://www.php.net/manual/datetime.format).

{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = DateTimeImmutable::createFromFormat('d. m. Y', $raw);

echo 'Start date: ' . $start->format('Y-m-d') . PHP_EOL;
{% endhighlight %}

Calculations with DateTimeImmutable are possible with the DateInterval class. DateTimeImmutable has methods such as `add()` and `sub()` that
take a DateInterval as an argument and return a new DateTimeImmutable instance. Use date intervals instead of calculating by seconds to ensure
that daylight savings and leap years etc are taken into account.

To calculate the difference between two dates use the `diff()` method which returns a new DateInterval. DateIntervals use the ISO8601 interval
specification, simply described as `P<date>T<time>` such as `P2DT5H` which is 2 days and 5 hours. The Time part is optional.

{% highlight php %}
<?php
// Add one month and 6 days to $start, returning a new DateTimeImmutable instance
$end = $start->add(new DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Difference: ' . $diff->format('%m month, %d days (total: %a days)') . PHP_EOL;
// Difference: 1 month, 6 days (total: 37 days)
{% endhighlight %}

You can use standard comparisons on DateTimeImmutable objects:

{% highlight php %}
<?php
if ($start < $end) {
    echo "Start is before the end!" . PHP_EOL;
}
{% endhighlight %}

A final feature of the DateTime library is the DatePeriod class. This is used to build up a list of DateTimeImmutable objects which can then
be iterated over. An example of this could be to create a range of dates once per day, or occurring every month in a year. This works by applying
the DateInterval on each date to generate the next date, so queries such as 'first thursday' or 'next month' will work just as well as the ISO8601
intervals. The DatePeriod::EXCLUDE_START_DATE will skip the first date.

{% highlight php %}
<?php
// output all thursdays between $start and $end
$periodInterval = DateInterval::createFromDateString('first thursday');
$periodIterator = new DatePeriod($start, $periodInterval, $end, DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // output each date in the period
    echo $date->format('Y-m-d') . PHP_EOL;
}
{% endhighlight %}

A popular PHP API extension is [Carbon](https://carbon.nesbot.com/). It inherits everything in the DateTime class, so involves minimal code alterations, but extra features include Localization support, further ways to add, subtract and format a DateTime object, plus a means to test your code by simulating a date and time of your choosing.

* [Read about DateTime][datetime]
* [Read about date formatting][dateformat]
* [Read about date interval formatting][dateinterval]

[datetime]: https://www.php.net/book.datetime
[dateformat]: https://www.php.net/manual/datetime.format
[dateinterval]: https://www.php.net/manual/dateintervalformat
