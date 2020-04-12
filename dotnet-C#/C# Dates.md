# Dates in C

- Noda Time is a popular .Net Time library, with TimeZoneConverter also available for conversion

## Date Ambiguity

- Date ambiguity is one of the biggest problems with Dates, and happens when, for example, dates are in the form 10/06/2019 which could be 10th of June 2019, or 6th of October 2019.
- Date Ambiguity is resolved with ISO 8601 format
  - with the date `2019-06-10T18:00:00+00:00`
  - it goes like this `year, month, day, time delimiter (T), Hour, Minute, Second + Timezone offset or Zulu time (Z)

## `DateTime` class

- Does not contain Timezone information
- Avoid using `DateTime.Now` as it is the local representation of the current time **on your system**.
- `DateTime` may contain a property with value of enum `DateTimeKind` which can be one of `Unspecified = 0`, `Utc = 1` or `Local = 2`.
- `DateTime.UtcNow` gives us UTC Time
- We should use `DateTime` when we want to work with
  - Dates
  - Times
  - UTC
  - Date & time arithmetic
  - Missing time zone information
  - interop with external systems like SQL Server
- We can parse the dates using the `DateTime.Parse` method, by passing in the string and **CultureInfo**

## `TimeZoneInfo` class

- Allows us to get either Local, UTC timezone information, or we can also search by string Id using the `TimeZoneInfo.FindSystemTimeZoneById(string id)` method.
  - the `id` is different on Unix vs Windows systems
  - e.g. "E.Australia Standard Time" on Windows vs "Australia/Sydney" on Unix
- Once we have the time zone information, we can use the `TimeZoneInfo.ConvertTime(DateTime dateTime, TimeZoneInfo destinationTZ)` method to convert from one method to another.

## `DateTimeOffset` class

- this should be preffered over `DateTime` class.
- The offset is based on UTC

```csharp
  var time = DateTimeOffset.Now;
  foreach(var timeZone in TimeZoneInfo.GetSystemTimeZones())
  {
      if (timeZone.UtcOffset(time) == time.Offset) // time.Offset returns a TimeSpan
      {
          Console.WriteLine(timeZone);
      }
  }

```

- We can convert one DateTimeOffset from one offset to another using `DateTimeOffset.ToOffset` method

## Date Parsing

- We can use `DateTime.ParseExact()` to parse dates exactly, that means they must match the specified format (and culture?) exactly

  ```csharp
    var date = "9/10/2019 10:00:00 PM";
    var parsedDate = DateTime.ParseExact(
      date,
      "M/d/yyyy h:mm:ss tt",
      CultureInfo.InvariantCulture);
    // Invariant Culture means the date is not specific to any culture
  ```

- `DateTime` leverages the local system Timezone to parse dates

```csharp
  var date = "2019-07-01 10:00:00 PM +02:00";
  var parsedDate = DateTime.Parse(date);

  // The above parsing is dependent on system's timezone, change the timezone in the system and the parsing will be done differently.

  parsedDate.Kind; // Local if local, UTC if in UTC

  var parsedDate = DateTime.Parse(date,
    CultureInfo.InvariantCulture,
    DateTimeStyles.AdjustToUniversal  // this will convert to UTC
  );

  parsedDate.Kind; // Utc
```

## Formatting Dates and Times

- calling `.ToString()` on `DateTime` and `DateTimeOffset` allows use to specify a format on how we want the date to be formatted as a string.

```csharp
  var date = "9/10/2019 10:00:00 PM";
  var parsedDate = DateTime.ParsExact(
    date,
    "M/d/yyyy h:mm:ss tt",
    CultureInfo.InvariantCulture
  );

  var formattedDate = parsedDate.ToString("yyyy-MMM-dd")  // 2019-Sep-10

  // or

  parsedDate.ToString("s")  // 2019-09-10T22:00:00 Note: TZ info is missing

```

- More on string formatters here https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings?view=netframework-4.8

- Similar code for `DateTimeOffset`

```csharp
  var date = "9/10/2019 10:00:00 PM";
  var parsedDate = DateTimeOffset.ParsExact(
    date,
    "M/d/yyyy h:mm:ss tt",
    CultureInfo.InvariantCulture
  );

    // shift the offset +10 hours from UTC
  parsedDate = parsedDate.ToOffset(TimeSpan.FromHours(10));

  var formattedDate = parsedDate.ToString("s");
    // 2019-09-11T06:00:00 // tz info still missing

  var formattedDate = parsedDate.ToString("o");
    // this will conform with ISO 8601 standard
    // 2019-09-11T:06:00:00.0000000+10:00
```

## UTC Time aka Zulu Military Time

- Always Store time in DB as UTC.
- Always use `DateTimeOffset.UtcNow` to get the current time. This gives the current time in UTC, rather than system's local time.
- To Convert to Local time, we can use `DateTimeOffset.UtcNow.ToLocalTime()`.
- If we do get local time, we can convert it to UTC using `DateTime.Now.ToUniversalTime()`

---

## Calculating Time Differences

- `TimeSpan` represents a time interval. e.g 1 day 23 hours 0 minutes.
- `TimeSpan` automaticall calculates the correct amount of days, hours, seconds, milliseconds and ticks based on the data it is being created with.

```csharp
  // we can also supply `ticks` which is 100 nanoseconds
  var timeSpan = new TimeSpan(60, 100, 200);  // hours minutes and seconds

  timeSpan.Hours; // 13 (this is the number of hours left after rounding the time to nearest day)
  // 60 hours + 100 minutes + 200 seconds = 2 days 13 hours 43 minutes 20 seconds


  // to get total hours we use
  timeSpan.TotalHours; // 61.72222
  timeSpan.TotalMilliseconds // similarly
```

- using TimeSpan.

  ```csharp
    var start = DateTimeOffset.UtcNow;
    var end = start.AddSeconds(45);

    TimeSpan difference = start - end;  //
    difference.Seconds; // seconds

    TimeSpan difference2 = end - start
    difference2.Seconds; // -45

    difference2.TotalSeconds; // 45

  ```

- NOTE: had we defined `end` like this `DateTimeOffset.UtcNow.AddSeconds(45)`, then the time needed to calculated `end` would also have mattered, hence the actual time may be increased slightly e.g. `45.000034`

#### Adding two Dates

- Addition, multiplication or division of two dates does not make sense.
- If we do want to do the above operations, we must use the built in operations.

```csharp

  var start = DateTimeOffset.UtcNow;
  var end = start.AddSeconds(45);

  TimeSpan difference = end - start;  // 0.75

  difference = difference.Subtract()
  // similarly
  // difference.Add() & difference.Divide() & difference.Multiply(2)  -> 1.5


  // we can also use methods directly on dates

  var end = start.AddDays() // or AddHours, AddMinutes etc
```

### Displaying Week Number

```csharp

  Calendar calendar = CultureInfo.InvariantCulture.Calendar;

  var start = new DateTimeOffset(2007, 12, 31, 0, 0, 0, TimeSpan.Zero);

  calendar.GetWeekOfYear(
    start.DateTime, // to convert to DateTime
    CalendarWeekRule.FirstFourDayWeek,
    DayOfWeek.Monday
    );  // This gives us week 53

  // but as per ISO 8601, weeks should always have 7 days,
  // so the above date should actually be a part of the week for next year.

  var isoWeek = ISOWeek.GetWeekOfYear(start.DateTime); // 1
  // so the above gives us week number as per ISO8601

```

### Extending a Date (by a certain time)

```csharp

  var contractDate = new DateTimeOffset(2019, 7, 1, 0, 0, 0, TimeSpan.Zero);
  // 2019-07-01 12:00:00 AM +00:00

  // let say we want to extend the above contract by a month, or a year.

  // Lets add 6 months

  contractDate = contractDate.AddMonhts(6);
  // 2020-01-01 12:00:00 AM +00:00

  // The problem above is that it is expiring on 1st of Jan, instead of by
  // the end of the year

  // we could subtract a tick, and that would put the time back to the previous year

  contractDate = contractDate.AddMonths(6).AddTicks(-1);
  // 2019-12-31 11:59:59 PM +00:00



  // now lets say, for the same contract date, we want to extend it by a month

  contractDate = contractDate.AddMonths(1);
  // 2020-03-28 11:59:59 PM +00:00

  // the problem now is the the whole month of March is not covered.
  // the date is stopping at 28 March, when it should go all the way till
  // March 31.



```

```csharp


  // lets define a method to deal with the above
  // the idea is to find the exact last day of a month every time we
  // extend a contract.

  public static class XYZ
  {
      public static DateTimeOffset ExtendContract(
        DateTimeOffset current,
        int months)
      {
          var newContractDate = current.AddMonths(months).AddTicks(-1);

          // DateTime.DaysInMonth() is only available in DateTime class
          return newDateTimeOffset(
            newContractDate.Year,
            newContractDate.Month,
            DateTime.DaysInMonth(newContractDate.Year, newContractDate.Month),
            23,
            59,
            59,
            current.Offset
          );
      }
  }

  // now when we extend the contract defined in the previous snippet

  contractDate = ExtendContract(contractDate, 1);
  // 2020-03-31 11:59:59 PM +00:00

```

---
