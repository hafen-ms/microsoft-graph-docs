---
title: "Agenda component in the Microsoft Graph Toolkit"
description: "The mgt-agenda web component is used to represent events in a user or group calendar."
localization_priority: Normal
author: nmetulev
---

# Agenda component in the Microsoft Graph Toolkit

The `mgt-agenda` web component represents events in a user or group calendar. By default, the calendar displays the current signed in user events for the current day. The component can also use any endpoint that returns events from Microsoft Graph.

## Example

[jsfiddle example](https://jsfiddle.net/metulev/ojt2c7vp/)

```html
<mgt-agenda group-by-day></mgt-agenda>
```

![mgt-agenda](./images/mgt-agenda.png)

## Properties

By default, the `mgt-agenda` component fetches events from the `/me/calendarview` endpoint and displays events for the current day. There are several properties you can use to change this behavior.

| Attribute | Property | Description |
| --- | --- | --- |
| date | date | A string that represents the start date of the events to fetch from Microsoft Graph. Value should be in a format that can be parsed by the [Date constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) - value has no effect if `event-query` attribute is set. |
| days | days | A number of days to fetch from Microsoft Graph - default is 3 - value has no effect if `event-query` attribute is set. |
| show-max | showMax | A number to indicate the maximum number of events to show. The default value is not set (no maximum). |
| group-id | groupId | A string ID for a group calendar to use instead of the current signed in user's calendar. |
| event-query | eventQuery | A string that represents an alternative query to be used when fetching events from Microsoft Graph. Optionally, add the delegated scope at the end of the string by delimiting it with `|` (`/groups/GROUP-ID-GUID/calendar/calendarView | group.read.all`). |
| events | events | An array of events to get or set the list of events rendered by the component - use this property to access the events loaded by the component. Set this value to load your own events - if value is set by developer, the `date`, `days`, or `event-query` attributes have no effect. |
| group-by-day | groupByDay | A Boolean value to group events by day - by default events are not grouped. |

The following example changes the behavior of the component to fetch data for a specific date and up to three days.

```html
<mgt-agenda
  group-by-day
  date="May 7, 2019"
  days="3"
  ></mgt-agenda>
```

The following example changes the behavior of the component to fetch data from a specific query.

```html
<mgt-agenda
  event-query="/me/events?orderby=start/dateTime"
  ></mgt-agenda>
```

## CSS custom properties

The `mgt-agenda` component defines these CSS custom properties

```css
mgt-agenda {
  --event-box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.092);
  --event-margin: 0px 10px 14px 10px;
  --event-padding: 8px 0px;
  --event-background: #ffffff;
  --event-border: solid 2px rgba(0, 0, 0, 0);

  --agenda-header-margin: 40px 10px 14px 10px;
  --agenda-header-font-size: 24px;
  --agenda-header-color: #333333;

  --event-time-font-size: 12px;
  --event-time-color: #000000;

  --event-subject-font-size: 19px;
  --event-subject-color: #333333;

  --event-location-font-size: 12px;
  --event-location-color: #000000;
}
```

To learn more, see [styling components](../style.md).

## Templates

The `mgt-agenda` component supports several [templates](../templates.md) that allow you to replace certain parts of the component. To specify a template, include a `<template>` element inside of a component and set the `data-type` value to one of the following:

| Data type | Data context | Description |
| --- | --- | --- |
| `default` | `events`: list of event objects | The default template replaces the entire component with your own. |
| `event` | `event`: event object | The template used to render each event. |
| `header` | `header`: string | The template used to render the header for each day. |
| `other` | `event`: event object | The template used to render additional content for each event. |
| `no-data` | No data context is passed | The template used when no events are available. |
| `loading` | No data context is passed | The template used when data is loading. |

The following examples illustrates how to use the `event` template:

```html
<mgt-agenda>
  <template data-type="event">
    <button class="eventButton">
      <div class="event-subject">{{ event.subject }}</div>
      <div data-for="attendee in event.attendees">
        <mgt-person
          person-query="{{ attendee.emailAddress.name }}"
          show-name
          show-email>
        </mgt-person>
      </div>
    </button>
  </template>
  <template data-type="no-data">
    There are no events found!
  </template>
</mgt-agenda>
```

To learn more, see [templates](../templates.md).

## Events

The following events are fired from the control.

| Event | Description |
| --- | --- |
| eventClick | The user clicks or taps on an event.|


## Graph scopes

This component uses the following Microsoft Graph APIs and permissions:

| resource | permission/scope |
| - | - |
| [/me/calendarview](/graph/api/calendar-list-calendarview?view=graph-rest-1.0) | `Calendars.Read` |

The component allows you to specify a different Microsoft Graph query to call (such as `/groups/{id}/calendar/calendarView`). In this case, append the scope at the end of the string, delimited by `|`

## Authentication

The login control leverages the global authentication provider described in the [authentication documentation](./../providers.md).

