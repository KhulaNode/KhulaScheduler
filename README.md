# KhulaScheduler

`KhulaScheduler` is a React scheduler and calendar library for day, week, and
month views. It is intended for teams that want to own scheduling UI inside
their own app instead of depending on a separate scheduling platform.

This fork is maintained by KhulaNode for direct product integration.

## What it provides

- day, week, and month scheduler views
- add, update, and delete event flows
- customizable buttons, tabs, and event modals
- provider-based state management
- exported event types and validation schema

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

- This package ships React components and Tailwind utility class names. Your
  app must provide the Tailwind/shadcn-compatible styling environment.
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
