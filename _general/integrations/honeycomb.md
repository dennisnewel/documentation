---
title: Using Honeycomb For Logging And Event Tracking With Codeship Pro
shortTitle: Logging And Event Tracking With Honeycomb
menus:
  general/integrations:
    title: Using Honeycomb
    weight: 11
tags:
  - events
  - logging
  - analytics
  - reports
  - reporting
  - integrations
---

* include a table of contents
{:toc}

## About Honeycomb

[Honeycomb](https://www.honeycomb.com) lets you collect and track data and events for debugging and trend analysis. During your continuous deployment workflow with Codeship Pro, you can use Honeycomb to log information related to your deployments or tests.

[Their documentation](https://honeycomb.io/docs/) does a great job of providing more information, in addition to the setup instructions below.

## Codeship Pro

### Setting Your Team Write Key

You will need to add your Honeycomb Team Write Key to your [encrypted environment variables]({{ site.baseurl }}{% link _pro/builds-and-configuration/environment-variables.md %}) that you encrypt and include in your [codeship-services.yml file]({{ site.baseurl }}{% link _pro/builds-and-configuration/services.md %}).

###  Sending Events And Markers

Next, you will need to add your Honeycomb API calls to a new script, placed in your repository, that you will call from your [codeship-steps.yml file]({{ site.baseurl }}{% link _pro/builds-and-configuration/steps.md %}). For example:

```bash
curl https://api.honeycomb.io/1/batch/Dataset%20Name -X POST \
  -H "YOUR TEAM: YOUR_WRITE_KEY" \
  -d '[
        {
          "time":"2017-02-09T02:00:00Z",
          "data":{"key1":"val1","key2":"val2"}
        },
        {
          "data":{"key3":"val3"}
        }
      ]'
```

Once you have put an API call similar to the one above in a script in your repository, you will call that script from a new step in your [codeship-steps.yml file]({{ site.baseurl }}{% link _pro/builds-and-configuration/steps.md %}). For instance:

```yaml
- name: honeycomb
  service: app
  command: honeycomb.sh
```

If you want to limit this step only to certain branches, for instance on deployments, you can use the [tag]({{ site.baseurl }}{% link _pro/builds-and-configuration/steps.md %}/#limiting-steps-to-specific-branches-or-tags) option. For instance:

```yaml
- name: honeycomb
  service: app
  tag: master
  command: honeycomb.sh
```

## Codeship Basic

### Setting Your Team Write Key

You will need to add your Honeycomb Team Write Key to your to your project's [environment variables]({{ site.baseurl }}{% link _basic/builds-and-configuration/set-environment-variables.md %}).

You can do this by navigating to _Project Settings_ and then clicking on the _Environment_ tab.

###  Sending Events And Markers

Next, you will need to add your Honeycomb API calls to your test or deployment commands.

If you are logging events from your test pipelines, you will want to add the Honeycomb API call - or call a script in your repository with the API call commands in it - via your [test commands]({{ site.baseurl }}{% link _basic/quickstart/getting-started.md %}).

Or, if you are calling the Honeycomb API during deployments you will want to use a [custom-script deployment step]({{ site.baseurl }}{% link _basic/builds-and-configuration/deployment-pipelines.md %}) in your deployment pipelines.

Whether in your test or deployment pipelines, you will want to call the Honeycomb API, similar to:

```bash
curl https://api.honeycomb.io/1/batch/Dataset%20Name -X POST \
  -H "YOUR TEAM: YOUR_WRITE_KEY" \
  -d '[
        {
          "time":"2017-02-09T02:00:00Z",
          "data":{"key1":"val1","key2":"val2"}
        },
        {
          "data":{"key3":"val3"}
        }
      ]'
```