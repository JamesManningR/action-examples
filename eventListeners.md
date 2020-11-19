# Event Types

To follow along with this documentation, you can take a look a the example repo [action-examples](https://github.com/JamesManningR/action-examples/actions)

This is also mainly a summary of the github docs page: [Events that trigger workflows - GitHub | Docs](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#repository_dispatch)

----

## Scheduled

Scheduled tasks will run based on the timing function, they can be created like this

```yml
on:
    schedule:
        - cron '[cron schedule expression]'
```

cron schedule expressions work like this

| Minute | Hour | Day of Month  | Month of Year | Day of Week |
|-|-|-|-|-|
| (0-59) | (0-11) | (1-31) | (1-12 or JAN-DEC) | (0 - 6 or SUN-SAT) |

Some other values can exist such as:

- `*` \- Any Value (wildcard) 
- `,` \- List seperator (eg. `1,4,7,12`)
- `-` \- Range (eg `1-4`)
- `/` \- Steps (eg `10/5` - starting at 10, for every 5)

----

### Examples

| Example | Minute | Hour | Day (month) | Month | Day (week) |
| - | - | - | - | - | - |
31st March at 4:05 | 5 | 4 | 31 | 3 | \*
Every Day 22:00 | \* | 22 | \* | \* | \* |
At 22:00 on Saturday | \* | 22 | \* | \* | SAT |
Also at 22:00 on Saturday | \* | 22 | \* | \* | 6 |
| | | | | | |

You can learn more and play about with  this at [crontab.guru](https://crontab.guru/)

----

## Manual Events

### workflow_dispatch

an action with a `workflow_dispatch` trigger runs when triggered on github itself, or with the github rest api.

```yml
on: workflow_dispatch
```

#### Via GitHub

To trigger on gihub simply go to YOUR REPO > actions > Workflows > NAME OF WORKFLOW > Run workflow

#### Via Rest Api

Send an HTTP `POST` Request to `api.github.com/repos/{owner}/{repo}/actions/workflows/{workflow_id}/dispatches`

With the Parameters

|     Name    |  Type  |   In   |                           Description                                              |
|:-----------:|:------:|:------:|:----------------------------------------------------------------------------------:|
| accept      | string | header | Setting to application/vnd.github.v3+json is recommended.                          |
| owner       | string | path   |                                                                                    |
| repo        | string | path   |                                                                                    |
| workflow_id |        | path   | The ID of the workflow or workflow file name                                       |
| ref         | string | body   | Required. The git reference for the workflow. The reference can be a branch or tag |
| inputs      | object | body   | Input keys and values configured in the workflow file.                             |

So on this repo, we'd set up an api call that looks like this

<!-- TODO: Add some example here -->

### repository_dispatch

Repository dispatch works slightly differently, repository_dispatch works more like a custom webhook event, so the event will be triggered by external means, but this will have to be manually triggered by the GitHub Api.

```yml
on:
    repository_dispatch:
        types: custom-event-name
```