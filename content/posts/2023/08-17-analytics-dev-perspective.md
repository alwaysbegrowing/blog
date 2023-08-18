---
title: Analytics = Good; No Analytics = Bad • dAppling Analytics Development
description: An overview of the dappling-analytics package from a dev perspective.
date: 2023-08-18
tags:
  - analytics
  - development
author: Bookcliff
---

One of the most consistently used tools here at dAppling is our site analytics - using `@vercel/analytics` to track page views, users, and custom events over time. Every Monday we sit down and say to ourselves, was last week a 'good' week or a 'bad' week. A 'good' week is more users, more page views, more events. A 'bad' week, less users, less page views, less events. Quickly glancing at our analytics page provides a simple categorization of our past week's marketing, business development, and engineering strategy - was it 'good' or was it 'bad'. And from this quick glance, we formulate our next week's strategy which will be similarly evaluated the following Monday. Week in, week out we follow this process.

While this is largely an oversimplification of our company strategy and development, and our decision-making each week is much more complex than whether the previous week was 'good' or 'bad', the fact remains that site analytics provide a useful, quantifiable metric driving our product development. In conjunction with other metrics, feedback, and data points, site analytics offer vitally important insights into performance and engagement.

##### Analytics Development

Because of the importance we place on site analytics, as we developed this blog which is hosted through [dAppling](https://dappling.network), we wanted an easy, simple method for tracking page view analytics in a privacy preserving way. We considered integrating with another service, but ultimately decided it made more sense to integrate site analytics natively into our core platform.

There are 2 main parts to the analytics.

1. [NPM package for users' frontends](https://github.com/alwaysbegrowing/dapplingAnalytics)

   This package is installed similarly to other NPM packages, using your favorite package manager (`npm install dappling-analytics`, `yarn install dappling-analytics`). Once installed, the package contains an `<Analytics />` component which injects a script into the head of your application. Whenever a user accesses a page, it triggers an API call to track information about that page view. The `<Analytics />` component was built using react, so for React and NextJS projects, the integration is seamless. However, for other frameworks, additional configuration is required to inject the script.

   Currently, our analytics package is in its infancy and only tracks page views and unique users. Overtime, we plan to iterate and track additional, useful datapoints, such as customizable events and simpler integration for non-React/NextJS applications.

2. API Endpoint & Database Storage
   The inject analytics script triggers a POST request to our lambda function which is set up to send the information about the page view to our database. There, the page view information is stored using the project ID associated with the domain of the page viewed. This can then be displayed to the project owner when they visit the [dAppling frontend](https://dappling.network). This was developed with the goal of providing simple, quick-to-interpret analytics for any site on [dAppling](https://dappling.network).

##### Conclusion

Here at dAppling we prioritize site analytics as a primary metric for understanding the performance and engagement of our application. We wanted to create a feature for our users that have similar priorities to us. Thus, the birth of dappling-analytics. At the end of the day, it's a very simple script meant to track users in a very simple way. It's nothing new or ground-breaking, just a quick, easy integration for gaining the information you need to tell if your website has had a 'good' or 'bad' week.

Use dappling-analytics today and watch your garden grow &#129716; &#127793;.

For more information about using dappling-analytics, check out our [docs](https://docs.dappling.network/guides/site-analytics).
