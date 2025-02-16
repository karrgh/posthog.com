---
title: Group analytics (Alpha)
sidebar: Docs
showTitle: true
---

<blockquote class="warning-note">
<strong>Group Analytics</strong> is currently a alpha feature and not available for everyone.
<br />
Exact details of the feature can change over the alpha period.
</blockquote>

**Group Analytics** allows you to calculate metrics on a group level. A "group" is an entity events
belong to - such as an Company, Team or Playlist of songs. If you're a B2B company, groups help
you analyze how various groups interact with your product rather than individual users.

## Prerequisites

- The feature is only available in alpha right now. <a href="/slack" target="_blank">Reach out</a> if you're interested.
- You are able to instrument up to 5 different group types per project
- The feature is currently available on PostHog Cloud

## Known limitations

- You can't (yet) analyze groups in insights or use them in feature flags. This will be added over the coming week(s).
- Updating group properties overwrites _all_ previous properties for that group

## How to set up group analytics

To make use of group analytics, you need to update your event capture code. See the sections below for how to set it up depending on how you are sending data to PostHog.

The following examples use `company_id` as a group type and `id:5` as the group key. Replace these with your particular values.

### [posthog-js](https://posthog.com/docs/integrate/client/js)

Update to version 1.16.0 or above to make use of the new functionality.

In posthog-js it's possible to declare that a user is currently "active" in a particular group. This means all events (both normal and autocaptured) are considered to be for that group.

To make sure all events get attached to groups, we recommend calling `posthog.group` in `loaded` callback.

```js
posthog.init('[your api key]', {
    api_host: 'https://posthog.[your-domain].com',
    loaded: function(posthog) {
        posthog.identify('[user unique id]');

        posthog.group('company_id', 'id:5');
        posthog.group('playlist', 'id:77', {
            length: 77,
            some: 'properties'
        });
    }
});
```

Subsequent calls to `posthog.group()` with the same group type but a different group key make the new group be active.

#### Handling logging out

When the user logs out it's important to call `posthog.reset()` to avoid new events being registered under registered groups and the logged in user.

### [posthog-python](https://posthog.com/docs/integrate/server/python)

Update to version 1.4.3 or above to make use of the new functionality.

```python
# Capturing an event with groups
posthog.capture('[distinct id]', 'some event', groups={'company_id': 'id:5'})

# Updating group properties
posthog.group_identify('company_id', 'id:5', {
    'company_name': 'Awesome Inc',
    'employees': 11
})
```

### [posthog-php](https://posthog.com/docs/integrate/server/php)

Update to version 2.1.0 or above to make use of the new functionality.

```c
# Capturing an event with groups
PostHog::capture(array(
    'distinctId' => '[distinct id]',
    'event' => 'some event',
    '$groups' => array("company_id" => "id:5")
));

# Updating a groups properties
PostHog::groupIdentify(array(
    'groupType' => 'company_id',
    'groupKey' => 'id:5',
    'properties' => array("company_name" => "Awesome Inc", "employees" => 11)
));
```

### [posthog-go](https://posthog.com/docs/integrate/server/go)

```go
// Capturing an event with groups
client.Enqueue(posthog.Capture{
    DistinctId: "[distinct id]",
    Event:      "some event",
    Groups: posthog.NewGroups().
        Set("company_id", "id:5").
})

// Updating a groups properties
client.Enqueue(posthog.GroupIdentify{
    Type: "company_id",
    Key:  "id:5",
    Properties: posthog.NewProperties().
        Set("company_name", "Awesome Inc").
        Set("employees", 11),
})
```

Using groups with go requires latest version of `posthog-go`. Update dependencies via:

```shell
go get -u github.com/posthog/posthog-go
```

### Other libraries

Not all libraries support group analytics yet, but you can work around this issue by sending events in specific formats.

This section uses our [Capture APIs](https://posthog.com/docs/api/post-only-endpoints) for examples but you can adapt this
approach to any library.

#### Capturing events with groups

```shell
POST https://[your-instance].com/capture/
Content-Type: application/json
Body:
{
    "api_key": "<ph_project_api_key>",
    "event": "[event name]",
    "properties": {
        "distinct_id": "[your users' distinct id]",
        "key1": "value1",
        "key2": "value2",
        "$groups": {
            "company_id": "id:5"
        }
    }
}

```

#### Updating group properties

```shell
POST https://[your-instance].com/capture/
Content-Type: application/json
Body:
{
    "api_key": "<ph_project_api_key>",
    "event": "$groupidentify",
    "properties": {
        "distinct_id": "[your users' distinct id]",
        "$group_type": "company_id",
        "$group_key": "id:5",
        "$group_set": {
            "company_name": "Awesome Inc",
            "employees": 11
        }
    }
}

```
