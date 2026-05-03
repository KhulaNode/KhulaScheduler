# KhulaScheduler

`KhulaScheduler` is a React scheduler/calendar library for day, week, and month
views. It is intended for product teams that want to own scheduling UI inside
their own app rather than wiring in a separate scheduling platform.

This fork is maintained by KhulaNode for direct product integration.

## What it provides

- day, week, and month scheduler views
- add, update, and delete event flows
- customizable buttons, tabs, and event modals
- provider-based state management
- exported event types and utility hooks

## Install

```bash
npm install @khulanode/khula-scheduler
```

## Basic usage

```tsx
"use client";

import {
  KhulaScheduler,
  SchedulerProvider,
  type Event,
} from "@khulanode/khula-scheduler";

const initialEvents: Event[] = [
  {
    id: "evt-1",
    title: "Intro session",
    startDate: new Date(),
    endDate: new Date(Date.now() + 60 * 60 * 1000),
    variant: "primary",
  },
];

export function SchedulerExample() {
  return (
    <SchedulerProvider initialState={initialEvents} weekStartsOn="monday">
      <KhulaScheduler />
    </SchedulerProvider>
  );
}
```

## Notes for host apps

- This package ships React components and Tailwind utility class names. Your app
  must provide the Tailwind/shadcn-compatible styling environment.
- If your Tailwind setup does not scan package code inside `node_modules`, add
  `@khulanode/khula-scheduler` to your scan/content configuration.
- This package provides scheduler UI and local event-state flow. Your product
  remains responsible for auth, persistence, booking rules, and access gating.

## Credits

`KhulaScheduler` is based on the upstream `mina-scheduler` project by
[Mina Massoud](https://mina-massoud.com/).

- Upstream repository: https://github.com/Mina-Massoud/mina-scheduler

This fork keeps upstream credit intact while adapting the package for
KhulaNode-owned installation and product integration.
  );
};
```

### Return Values

The `useScheduler` hook returns an object containing the following properties:

- **`events`**: 
  - Type: `SchedulerState`
  - Description: Contains the current state of the scheduler, including an array of events.

- **`dispatch`**: 
  - Type: `Dispatch<Action>`
  - Description: A function to modify the scheduler state. Use it to send actions to the reducer.

  #### Dispatch Call Shape

  The shape of the dispatch call is as follows:

  ```typescript
  dispatch({
    type: "ADD_EVENT", // or "REMOVE_EVENT" or "UPDATE_EVENT"
    payload: { /* Event object */ } // required for "ADD_EVENT" and "UPDATE_EVENT"
  });
  ```

  **Action Types**:
  - **`ADD_EVENT`**: Adds a new event to the scheduler. Use this action when you want to create a new event.
  - **`REMOVE_EVENT`**: Removes an event based on its `id`. Use this action to delete an existing event.
  - **`UPDATE_EVENT`**: Updates an existing event. Use this action when you need to modify the details of an event.

- **`getters`**: 
  - Type: `Getters`
  - Description: An object containing utility functions to retrieve information from the scheduler state.

  #### Getter Functions
  - **`getDaysInMonth(month: number, year: number)`**: Returns an array of objects representing each day in the specified month, each containing a list of events for that day.
  - **`getEventsForDay(day: number, currentDate: Date)`**: Retrieves all events for a specific day.
  - **`getDaysInWeek(week: number, year: number)`**: Returns an array of Date objects for the specified week.
  - **`getWeekNumber(date: Date)`**: Returns the week number for the given date.
  - **`getDayName(day: number)`**: Returns the name of the day for a given index (0 for Sunday, 6 for Saturday).

- **`handlers`**: 
  - Type: `Handlers`
  - Description: An object containing functions to handle specific actions related to events.

  #### Handler Functions
  - **`handleEventStyling(event: Event, dayEvents: Event[])`**: Returns styling properties for rendering an event based on its position among other events on the same day.
  - **`handleAddEvent(event: Event)`**: Adds a new event to the scheduler. You can call this function to handle event creation logic.
  - **`handleUpdateEvent(event: Event, id: string)`**: Updates an existing event by its `id`. Use this function to modify event details.
  - **`handleDeleteEvent(id: string)`**: Deletes an event by its `id`. Call this function to remove events from the scheduler.

### Example

Here’s a simple example of how to use the `useScheduler` hook in a component:

```tsx
import React from "react";
import { useScheduler } from "@/path/to/SchedulerContext";

const EventList = () => {
  const { events, dispatch, handlers } = useScheduler();

  const removeEvent = (id) => {
    handlers.handleDeleteEvent(id);
  };

  return (
    <div>
      {events.events.map((event) => (
        <div key={event.id}>
          <h3>{event.title}</h3>
          <button onClick={() => removeEvent(event.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
};
```

### Event Schema and Form Data

The library uses **Zod** for form validation, and **React Hook Form** for handling form data. Here's the event schema and how it's used in the form.

#### Event Schema (Zod)

The `eventSchema` defines the structure and validation rules for event forms using **Zod**.

```ts
export const eventSchema = z.object({
  title: z.string().nonempty("Event name is required"),
  description: z.string().optional(),
  startDate: z.date(),
  endDate: z.date(),
  variant: z.enum(["primary", "danger", "success", "warning", "default"]),
  color: z.string().nonempty("Color selection is required"),
});
```

#### EventFormData

The form data is handled through the `EventFormData` interface, which corresponds to the schema's structure.

```ts
export type EventFormData = z.infer<typeof eventSchema>;
```

### SelectDate Component

The `SelectDate` component helps with selecting a date range and times for an event.

#### Props:
- **data** `(optional)`: `{ startDate: Date; endDate: Date; time: Time }` – The initial data for start and end dates and times.
- **setValue**: `UseFormSetValue<EventFormData>` – Function from React Hook Form to set form values.

#### Example of Usage:

```tsx
import { UseFormSetValue } from "react-hook-form";
import SelectDate from "@/components/schedule/_components/add-event-components/select-date";

<SelectDate data={data} setValue={setValue} />
```

### Types and Interfaces

#### Event

Represents an individual event on the calendar.

```ts
export interface Event {
  id: string;
  title: string;
  description?: string;
  startDate: Date;
  endDate: Date;
  variant?: Variant;
}
```

#### Variant

Defines the style variant of an event, which can be one of the following:
- `"success"`
- `"primary"`
- `"default"`
- `"warning"`
- `"danger"`

#### Views

Defines the available views for mobile and desktop.

```ts
export type Views = {
  mobileViews?: string[];
  views?: string[];
};
```

#### startOfWeek

Specifies the starting day of the week, either `"sunday"` or `"monday"`.

```ts
export type startOfWeek = "sunday" | "monday";
```

#### CustomEventModal

Represents customization options for the event modal, including the form and title.

```ts
export interface CustomEventModal {
  CustomAddEventModal?: {
    title?: string;
    CustomForm?: React.FC<{ register: any; errors: any }>;
  };
}
```

#### CustomComponents

Defines customizable components such as buttons, tabs, and event modals.

```ts
export interface CustomComponents {
  customButtons?: {
    CustomAddEventButton?: React.ReactNode;
    CustomPrevButton?: React.ReactNode;
    CustomNextButton?: React.ReactNode;
  };

  customTabs?: {
    CustomDayTab?: React.ReactNode;
    CustomWeekTab?: React.ReactNode;
    CustomMonthTab?: React.ReactNode;
  };
  CustomEventComponent?: React.FC<Event>; // Using custom event type
  CustomEventModal?: CustomEventModal;
}
```

#### ClassNames

Groups class names for buttons, tabs, and views.

```ts
export interface ClassNames {
  event?: string;
  buttons?: ButtonClassNames;
  tabs?: TabsClassNames;
  views?: ViewClassNames;
}
```

#### ButtonClassNames

Specifies class names for previous, next, and add event buttons.

```ts
export interface ButtonClassNames {
 

 prev?: string;
  next?: string;
  addEvent?: string;
}
```

#### TabsClassNames

Specifies class names for various parts of the tab interface.

```ts
export interface TabsClassNames {
  cursor?: string;
  panel?: string;
  tab?: string;
  tabContent?: string;
  tabList?: string;
  wrapper?: string;
}
```

#### ViewClassNames

Specifies class names for day, week, and month views.

```ts
export interface ViewClassNames {
  dayView?: string;
  weekView?: string;
  monthView?: string;
}
```

## License

This library is licensed under the MIT License.

---

Thank you, feel free to follow me on linkedIn : https://www.linkedin.com/in/mina-melad/
contact with me and discover my portfolio : https://mina-massoud.onrender.com/
