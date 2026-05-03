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

