# FAQ

This page collects common questions and answers in a simple, scannable format. Each question is shown as a heading, and the answer is placed in a collapsible section so readers can expand only the topics they need.

## Getting started

### What if after the startup robot doesn't stand up straight?

:::{dropdown} Answer
The issue is caused by edge case behavior. While we are working on a fix, please follow this workaround:

In the remote, use [Command Switch](GettingStarted/ControlConsole.md#user-interface) to get into the Power section, then:

- click `DISABLE BUS`
- set up the robot in the starting [position lying down](Physical/Overview.md#general-dimensions), like on the technical drawings
- click `ENABLE BUS`
- click `HEALTHCHECK`

Then go back to the Control Section and follow the normal starting routine.

If the error is still there, redo the procedure, but instead of laying the robot flat, rotate the problematic joint by 10 degrees in any direction.
:::
