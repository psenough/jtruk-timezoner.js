# jtruk-timezoner.js

A drop-in timezone handler for online schedules.

Demoparties are very welcome to use this on their websites (please [drop me a line](#contact--requests) if you do!).

## Known Deployments

- [Lovebyte 2024](https://lovebyte.party/#Timetable)
- [Outline 2024](https://outlinedemoparty.nl/timetable/)
- [Shadow Party 2024](https://shadow-party.org/timetable)
- [Evoke 2024](https://2024.evoke.eu/party/timetable/)
- [Deadline 2024](https://www.demoparty.berlin/events/)

## Overview

This was created to improve the experience of live events that are enjoyed from other timezones.

It's a small bit of JavaScript that adds a timezone selector to a website so that a timetable / schedule can be displayed in whichever timezone the visitor prefers.

Selling points are:

- **Progressive** - If the user has JavaScript disabled, the times will still be displayed with the default timezone.
- **Explicit** - It is clear to the user which timezone is being displayed. I've noticed some websites (e.g. Revision 2023/4) automatically remap to your system timezone, which is clever but can be confusing - especially when the hosts talk about the upcoming schedule, when someone is travelling / trying to plan arrival/departure from a different timezone, or when people in different timezones are discussing the event.
- **Configurable** - The initialisation accepts a configuration. This can be used to change how the local version of the time is displayed (and could be extended).
- **Contained** - Just one JavaScript include. No other dependencies.

## Usage

This requires three steps:

- Add an element that will be replaced by the timezone selector.
- Wrap the times to be updated in a class.
- Add attributes in the HTML to mark out dates.
- Include the script and initialise it.

You can additionally:

- Configure how the local time is displayed.
- Style with standard CSS.

### Timezone Selector

The content of this DOM element will be replaced by a dropdown selector when the script initialises.

```javascript
<span id="jtzr-ui">Times are presented in UTC+1</span>
```

I recommend the default text describes the default timezone for users who do not have JavaScript enabled.

### Time

Wrap any times that need to automatically update in a `jtzr-time` class:

```javascript
<span class="jtzr-time">10:00</span>
```

This is 24-hour format, with or without leading zero, and either with colon or dot as separator (i.e `08:00` and `8.00` are both acceptable).

Please [get in touch](#contact--requests) if you need it to support other formats.

You might like to add `white-space: nowrap` to these elements in your CSS, to prevent the time from wrapping onto a new line.

### Date

Add elements with a `data-jtzr-date` attribute to the document:

```javascript
<span data-jtzr-date="2024-05-11"></span>
```

This is required so that the script can add a day clarification, e.g. `(on Monday)`, to the end of timestamps that spill into the next or previous day.

It may be added to an earlier sibling element, or directly on a parent (e.g. the `table` containing the schedule).

It receives a sensible date format "YYYY-MM-DD"... sorry Americans! =)

The time stamps will pick up these markers from earlier in the HTML - there is an algorithm which should fit use cases - though may be fallible in exotic setups (please [let me know](#contact--requests)!).

### Include and Boot

Ensure you include your event's timezone in the initialisation:

```javascript
<script async defer src="jtruk-timezoner.js"></script>

<script defer>
    window.onload = () => {
        jtzrInit({
            eventUTC: 1
        });
    }
</script>
```

### Customisation

`jtzrInit` sets up a default time formatter function, which can be overridden with an optional parameter.

```javascript
jtzrInit({
    ...
    fnTimeFormatter: (el, params) => {
        ...
    }
})
```

The function specified by fnTimeFormatter receives a DOM element to be set, and a params object containing:

- `localUTCString` e.g. 'UTC+2'
- `localIDayOfWeek` (JavaScript Date day of week: 0 - 6 => Sunday - Saturday)
- `localHour` - 24-hour format
- `localMinute` - 0 - 59
- `eventUTCstring` e.g. 'UTC+1'
- `eventIDayOfWeek` - as per `localIDayOfWeek`

This custom function should construct a string and update the DOM element itself (e.g. `el.textContent = <new time string>`).

## Limitations

- If the clocks change during an event (hello Revision!), this won't be represented well.
- Complicated layouts may make it difficult for the code to figure out anchor dates (Happy to look at this if you have an example that needs fixing!)

## Contact / Requests

Feel welcome to Fork for your purposes, and make reasonable Pull Requests with improvements that are documented, backward-compatible, and useful to others.

If you deploy it on a website, please let me know, so that I can feel a tiny bit tickled: https://mastodon.social/@jtruk

If you have issues, or you find this code unsuitable when you think it should fit your use case, then please contact me. If your request makes it more useful to others then I'll be happy to improve this code!
