
| Method         | Return Type | Example                         | Purpose                  |
| -------------- | ----------- | ------------------------------- | ------------------------ |
| `now()`        | `LocalDate` | `LocalDate.now()`               | Current date             |
| `of(y,m,d)`    | `LocalDate` | `LocalDate.of(2026,6,6)`        | Create date              |
| `parse(str)`   | `LocalDate` | `LocalDate.parse("2026-06-06")` | Parse string             |
| `from()`       | `LocalDate` | `LocalDate.from(temporal)`      | Convert from Temporal    |
| `ofYearDay()`  | `LocalDate` | `LocalDate.ofYearDay(2026,100)` | Create using day of year |
| `ofEpochDay()` | `LocalDate` | `LocalDate.ofEpochDay(1000)`    | Create from epoch day    |




| Method                  | Example Output |
| ----------------------- | -------------- |
| `getYear()`             | `2026`         |
| `getMonth()`            | `JUNE`         |
| `getMonthValue()`       | `6`            |
| `getDayOfMonth()`       | `6`            |
| `getDayOfWeek()`        | `SATURDAY`     |
| `getDayOfYear()`        | `157`          |
| `get(ChronoField.YEAR)` | `2026`         |



| Method               | Example          |
| -------------------- | ---------------- |
| `plusDays(5)`        | Add 5 days       |
| `plusWeeks(2)`       | Add 2 weeks      |
| `plusMonths(3)`      | Add 3 months     |
| `plusYears(1)`       | Add 1 year       |
| `plus(amount, unit)` | Generic addition |



| Method                | Example             |
| --------------------- | ------------------- |
| `minusDays(5)`        | Remove 5 days       |
| `minusWeeks(2)`       | Remove 2 weeks      |
| `minusMonths(3)`      | Remove 3 months     |
| `minusYears(1)`       | Remove 1 year       |
| `minus(amount, unit)` | Generic subtraction |


| Method               | Example            |
| -------------------- | ------------------ |
| `withYear(2030)`     | Change year        |
| `withMonth(12)`      | Change month       |
| `withDayOfMonth(25)` | Change day         |
| `withDayOfYear(100)` | Change day-of-year |
| `with(adjuster)`     | Custom adjustment  |


| Method            | Return  |
| ----------------- | ------- |
| `isBefore(date)`  | boolean |
| `isAfter(date)`   | boolean |
| `isEqual(date)`   | boolean |
| `equals(obj)`     | boolean |
| `compareTo(date)` | int     |


| Method            | Example Output |
| ----------------- | -------------- |
| `isLeapYear()`    | `true`         |
| `lengthOfMonth()` | `30`           |
| `lengthOfYear()`  | `365`          |


| Method              | Return Type   |
| ------------------- | ------------- |
| `toString()`        | String        |
| `format(formatter)` | String        |
| `toEpochDay()`      | long          |
| `atStartOfDay()`    | LocalDateTime |
| `atTime(h,m)`       | LocalDateTime |
| `atTime(LocalTime)` | LocalDateTime |


| Constant        | Value            |
| --------------- | ---------------- |
| `LocalDate.MIN` | -999999999-01-01 |
| `LocalDate.MAX` | +999999999-12-31 |


    LocalDate now = LocalDate.now();
    System.out.println("Date Now : " + now);
    // Date Now : 2026-06-06
    
    LocalDate nowInTimeZone = LocalDate.now(ZoneId.of("UTC"));
    System.out.println("Date Now in Time Zone : " + nowInTimeZone);
    // Date Now in Time Zone : 2026-06-06
    
    
    int year = now.getYear();
    Month month = now.getMonth();
    int monthValue = now.getMonthValue();
    int day = now.getDayOfMonth();
    int day1 = now.getDayOfYear();
    DayOfWeek day2 = now.getDayOfWeek();
    
    System.out.println(year);
    // 2026
    
    System.out.println(month);
    // JUNE
    
    System.out.println(monthValue);
    // 6
    
    System.out.println(day);
    // 6
    
    System.out.println(day1);
    // 157
    
    System.out.println(day2);
    // SATURDAY
    
    
    System.out.println("currentDate : " + now);
    // currentDate : 2026-06-06
    
    
    LocalDate nowPlus3Year3Month3Day3Week =
    now.plusYears(3).plusMonths(3).plusDays(3).plusWeeks(3);
    
    System.out.println("nowPlus3Year3Month3Day3Week : "
    + nowPlus3Year3Month3Day3Week);
    // nowPlus3Year3Month3Day3Week : 2029-09-30
    
    
    LocalDate minus3Year3Month3Day3Week =
    nowPlus3Year3Month3Day3Week.minusYears(3)
    .minusMonths(3)
    .minusDays(3)
    .minusWeeks(3);
    
    System.out.println("Minus3Year3Month3Day3Week : "
    + minus3Year3Month3Day3Week);
    // Minus3Year3Month3Day3Week : 2026-06-06
    
    
    LocalDate nowMinus5Days = now.minus(5, ChronoUnit.DAYS);
    
    System.out.println("nowMinus5Days : " + nowMinus5Days);
    // nowMinus5Days : 2026-06-01
    
    
    LocalDate nowPlus5Days = now.plus(5, ChronoUnit.DAYS);
    
    System.out.println("nowPlus5Days : " + nowPlus5Days);
    // nowPlus5Days : 2026-06-11
    
    
    LocalDate date1 = LocalDate.of(2020, 6, 3);
    LocalDate date2 = LocalDate.of(2020, 4, 7);
    
    System.out.println(date1.isBefore(date2));
    // false
    
    System.out.println(date1.isAfter(date2));
    // true
    
    System.out.println(date1.isEqual(date2));
    // false
    
    System.out.println(date1.isLeapYear());
    // true
    
    
    LocalDate year2007 = now.withYear(2007);
    LocalDate month10 = now.withMonth(10);
    
    System.out.println(year2007);
    // 2007-06-06
    
    System.out.println(month10);
    // 2026-10-06
    
    
    System.out.println(now.lengthOfMonth());
    // 30
    
    System.out.println(now.lengthOfYear());
    // 365
    
    
    String nowInString = now.toString();
    LocalDate nowInLocalDate = LocalDate.parse(nowInString);
    
    System.out.println(nowInString);
    // 2026-06-06
    
    System.out.println(nowInLocalDate);
    // 2026-06-06
    
    
    System.out.println("LocalDate.now() : " + LocalDate.now());
    // LocalDate.now() : 2026-06-06
    
    System.out.println("LocalDate.MIN : " + LocalDate.MIN);
    // LocalDate.MIN : -999999999-01-01
    
    System.out.println("LocalDate.MAX : " + LocalDate.MAX);
    // LocalDate.MAX : +999999999-12-31
    
    System.out.println("LocalDate.from(...) : "
    + LocalDate.from(LocalDate.now()));
    // LocalDate.from(...) : 2026-06-06
    
    System.out.println("LocalDate.ofYearDay(2026, 100) : "
    + LocalDate.ofYearDay(2026, 100));
    // LocalDate.ofYearDay(2026, 100) : 2026-04-10
    
    // Epoch date = 1970-01-01
    System.out.println("LocalDate.ofEpochDay(1000) : "
    + LocalDate.ofEpochDay(1000));
    // LocalDate.ofEpochDay(1000) : 1972-09-27
    
    
    DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern("dd-MM-yyyy");
    
    String formatted = now.format(formatter);
    
    LocalDate parsedDate =
    LocalDate.parse(formatted, formatter);
    
    System.out.println(formatted);
    // 06-06-2026
    
    System.out.println(parsedDate);
    // 2026-06-06

------------------------------------------------------------------------------------------------------------------------------------


    LocalDateTime = LocalDate + LocalTime


| Category          | LocalDate | LocalDateTime |
| ----------------- | --------- | ------------- |
| `now()`           | ✅         | ✅             |
| `of()`            | ✅         | ✅             |
| `parse()`         | ✅         | ✅             |
| `getYear()`       | ✅         | ✅             |
| `getMonth()`      | ✅         | ✅             |
| `getMonthValue()` | ✅         | ✅             |
| `getDayOfMonth()` | ✅         | ✅             |
| `getDayOfWeek()`  | ✅         | ✅             |
| `getDayOfYear()`  | ✅         | ✅             |
| `plusDays()`      | ✅         | ✅             |
| `plusMonths()`    | ✅         | ✅             |
| `plusYears()`     | ✅         | ✅             |
| `minusDays()`     | ✅         | ✅             |
| `minusMonths()`   | ✅         | ✅             |
| `minusYears()`    | ✅         | ✅             |
| `withYear()`      | ✅         | ✅             |
| `withMonth()`     | ✅         | ✅             |
| `isBefore()`      | ✅         | ✅             |
| `isAfter()`       | ✅         | ✅             |
| `isEqual()`       | ✅         | ✅             |
| `format()`        | ✅         | ✅             |



Additional Time Methods

    LocalDateTime now = LocalDateTime.now();
Get Time Components
    
    now.getHour();      // 14
    now.getMinute();    // 30
    now.getSecond();    // 45
    now.getNano();      // nanoseconds

Add Time

    now.plusHours(5);
    
    now.plusMinutes(30);
    
    now.plusSeconds(10);
    
    now.plusNanos(1000);

Subtract Time
    
    now.minusHours(2);
    
    now.minusMinutes(15);
    
    now.minusSeconds(20);

Modify Time
    
    now.withHour(10);
    
    now.withMinute(45);
    
    now.withSecond(0);
    
    now.withNano(0);

Conversions
    
    LocalDateTime → LocalDate
    LocalDate date = now.toLocalDate();
    LocalDateTime → LocalTime
    LocalTime time = now.toLocalTime();
    Combine Date + Time
    LocalDate date = LocalDate.now();
    LocalTime time = LocalTime.now();
    
    LocalDateTime dt =
    LocalDateTime.of(date, time);

Formatting
    
    DateTimeFormatter formatter =
    DateTimeFormatter.ofPattern(
    "dd-MM-yyyy HH:mm:ss");
    
    String str = now.format(formatter);

Example:
    
    06-06-2026 14:30:45
    Useful Creation Methods
    LocalDateTime.now();
    
    LocalDateTime.of(2026, 6, 6, 10, 30);
    
    LocalDateTime.parse(
    "2026-06-06T10:30:00"
    );
    
    LocalDateTime.of(
    LocalDate.now(),
    LocalTime.now()
    );


-------------------------------------------------------------------------------------------------------------------------------------------


| Class            | Example                                   |
| ---------------- | ----------------------------------------- |
| `LocalDate`      | `2026-06-06`                              |
| `LocalTime`      | `10:30:45.123`                            |
| `LocalDateTime`  | `2026-06-06T10:30:45.123`                 |
| `OffsetDateTime` | `2026-06-06T10:30:45+05:30`               |
| `ZonedDateTime`  | `2026-06-06T10:30:45+05:30[Asia/Kolkata]` |
| `Instant`        | `2026-06-06T05:00:00Z`                    |


1. Period

        Used for date differences.
        
        LocalDate start = LocalDate.of(2020, 1, 1);
        LocalDate end = LocalDate.of(2026, 6, 6);
        
        Period period = Period.between(start, end);
        
        System.out.println(period.getYears());
        System.out.println(period.getMonths());
        System.out.println(period.getDays());

Output:

    6
    5
    5

Interview question:

Difference between Period and Duration?

2. Duration

        Used for time differences.
        
        LocalDateTime start =
        LocalDateTime.of(2026,6,6,10,0);
        
        LocalDateTime end =
        LocalDateTime.of(2026,6,6,12,30);
        
        Duration duration =
        Duration.between(start,end);
        
        System.out.println(duration.toMinutes());

Output:

    150

3. ChronoUnit

Very common.

    long days =
    ChronoUnit.DAYS.between(startDate,endDate);
    
    long months =
    ChronoUnit.MONTHS.between(startDate,endDate);

Example:

    long days =
    ChronoUnit.DAYS.between(
    LocalDate.of(2026,1,1),
    LocalDate.of(2026,6,6)
    );

4. Instant

        Extremely important in distributed systems.
        
        Instant now = Instant.now();
        
        System.out.println(now);

Output:

    2026-06-06T05:30:00Z

Questions:

    What is UTC?
    Why store Instant in DB?
    Why use Instant instead of LocalDateTime?


What is UTC?

    UTC (Coordinated Universal Time) is the world's standard time reference.
    
    It does not change with time zones.
    It does not observe daylight saving time (DST).
    Every time zone is defined as an offset from UTC.

Examples:

    Time Zone	Offset from UTC
    India (IST)	UTC+05:30
    London (winter)	UTC+00:00
    New York (winter)	UTC−05:00
    
    If UTC time is:
    
    2026-06-06T12:00:00Z
    
    Then in India:
    
    2026-06-06T17:30:00+05:30
    
    The Z means UTC (zero offset).
    
    Why store Instant in DB?
    
    Instant represents an exact moment on the timeline.
    
    Instant now = Instant.now();
    
    Example:
    
    2026-06-06T08:15:30.123Z

Benefits:

1. Unambiguous
   2026-06-06 10:00

Using LocalDateTime, we don't know:

    10:00 in India?
    10:00 in New York?
    10:00 in London?

With Instant:

2026-06-06T04:30:00Z

    There is only one interpretation.

2. Works globally

        Suppose:
        
        User A creates an order in India.
        User B views it in New York.
        
        Store:
        
        2026-06-06T04:30:00Z
        
        Then display:
        
        instant.atZone(ZoneId.of("Asia/Kolkata"))
        
        or
        
        instant.atZone(ZoneId.of("America/New_York"))
        
        Everyone sees the same event in their local time.

   3. Easier sorting and comparison
      order1.getCreatedAt().isBefore(order2.getCreatedAt())

           No timezone confusion.
        
           Why use Instant instead of LocalDateTime?
           LocalDateTime
           LocalDateTime.of(2026, 6, 6, 10, 0)
        
           Represents:
        
           2026-06-06 10:00
        
           But does not contain:
        
           timezone
           offset
        
           So it is ambiguous.
        
           Instant
           Instant.parse("2026-06-06T04:30:00Z")
        
           Represents a unique moment worldwide.
        
           Example
        
           Suppose a meeting starts at:
        
           10:00 AM IST
        
           Store as LocalDateTime:
        
           2026-06-06T10:00
        
           A user in New York reads it:
        
           10:00 AM New York
        
           Wrong!
        
           Store as Instant:
        
           2026-06-06T04:30:00Z
        
           Then:
        
           India:
        
           10:00 AM IST
        
           New York:
        
           12:30 AM EDT
        
           Both represent the same real-world moment.