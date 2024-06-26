---
layout: post
title: Are You Ready to Low-Code Green? Introduction to Sustainable Canvas App and Power Automate Development
author: milla.vaananen
excerpt: >
   Some insight and ideas how to implement green coding practicisies in Canvas App and Power Automate Development.
tags:
 - Green Coding
 - Canvas App
 - Power App
 - Power Platform
 - Power Automate
---

![Illustration of how green software divides to energy efficiency, hardware efficiency and carbon awareness](/img/2024-Power-Platform/Green%20Sofware%20Principles.png)
> Picture: [Green Software Foundation](https://learn.greensoftware.foundation/introduction)

The ICT sector (including data centers, communication networks, and user devices) is estimated to account for [around 4-6% of global electricity usage.](https://post.parliament.uk/research-briefings/post-pn-0677/) When discussing CO2 or greenhouse gas emissions, the ICT sector's emissions primarily stem from electricity consumption. As an app developer, I don’t have the power to dictate the type of electricity end-users or data centers use; therefore, I should focus on reducing the energy consumption of the app.

When working with [the Power Platform](https://powerplatform.microsoft.com/en-us/what-is-power-platform/), one might easily think that developers can't do much. The platform takes care of most things like scaling, data savings, and cloud servers. Here, green coding comes into play.

Green coding is a method of coding that aims to minimize the energy required to process a single line of code. While there are no studies available on the efficiency of [Power FX](https://learn.microsoft.com/en-us/power-platform/power-fx/overview) (if I were to speculate, there might not be much to brag about it, [given that high-level coding languages, like JavaScript, are not typically the most efficient](https://sites.google.com/view/energy-efficiency-languages/home)), and we lack the ability to select the language in Canvas apps, our attention should be directed towards enhancing the energy efficiency of our apps through alternative means.

When improving carbon and energy efficiency, developers should aim to emit the least amount of carbon possible and use the least amount of energy possible. While studying for the Green Software for Practitioners exam, I became familiar with the six principles of green software. I decided to focus on the efficiency side of these principles and found a way to divide them into four different points:

-   Data transfer efficiency

-   Code efficiency

-   Physical Data Center efficiency

-   Hardware efficiency

## Data transfer efficiency

![Photo of Powerplatform icons: Power Apps, Power Automate, Power FX, Power Platform and Dataverse](/img/2024-Power-Platform/Power%20Platform%20products.png)
>   Power Platform products included in discussion

When developing [Canvas Apps](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/getting-started) or Power Automates, it's easy to forget to limit the amount of transferrable data. There's a temptation to fetch data "just in case" for the app. Canvas App uses OData and API requests to transfer data via connectors. Therefore, limiting the amount of data transferred from the data source is key to improving efficiency.

In the Canvas app and Power Automate context, I believe the following checkups are in place:

-   What things are fetched when the app opens? Are they all needed, or is there something that is fetched but not necessarily used? Consider removing unnecessary data fetches or replacing them closer to the event when the data is used.

-   How much data is fetched?

    -   Canvas app has the Explicit column setting that helps automate fetching only necessary columns when, for example, the lookup function is used, rather than fetching the whole record. Nowadays, it is enabled by default.

    -   When showing lists, consider the maximum size of data needed to show to the end-user without requiring search or filtering arguments. You can also implement a paging option to limit the amount of fetched data.

    -   When using pictures, videos, and GIFs, consider their sizes and formats that support good quality in smaller sizes. Simple file types usually have smaller sizes for transfer.

    -   In Power Automate, it's often possible to use OData Expand or fetch XML to get information from related entities instead of looping results and using the get record action.

-   How data is filtered or sorted. When possible, sort and filter the data on the server side to limit the amount of data and handling operations done on the app client side.

    -   Use views and delegable functions to limit the amount of fetched data, e.g., in galleries or lists. You can also prefilter and sort it in the data source.

    -   Think about the filtering order; filter data in a way that you always get as little data as possible.

-   How often the data is transferred?

    -   What does the app do when no one is using it? Ensure that there are no functions fetching data when the app is idle. Implement time controls to end at some point or ask the user if they are still active.

    -   How are services that do not respond handled? How many pings are needed?

    -   In the Power Automate context, how often are scheduled triggers executed? Is it really necessary, or could a webhook be an alternative trigger? If scheduled triggering is frequent, ensure that condition criteria are checked at the beginning.

-   Use data caches when possible.

    -   Differentiate between global and local variables. When a global variable updates, it computes all the places where the variable is used, whereas a local variable only affects the screen you are currently working on. Consider using local variables transferred to another screen with the Navigate function instead of global variables. However, global variables still have their uses, so use them accordingly.

    -   Utilize app formulas to calculate values only when needed and keep them updated when the database changes. This eliminates the need to refresh to reset variables after changes in the database and optimizes how and when calculations are made.

    -   Use the With function when you need in-function variables, instead of fetching the same lookup information repeatedly.

    -   Consider using collections instead of fetching data from data sources all the time. You can also patch the collection to the data source at once, instead of using "for all," which sends each row separately.

## Code efficiency

When aiming to improve code energy efficiency, focus on reducing code repetition.

-   Consider whether calculations should be done on the server side, especially if the data comes from multiple sources.

-   Evaluate how often something is calculated and the required precision. For example, in loading, allow the user to select refresh intervals instead of pinging the database every second. Limit decimals in numbers in the app and data source.

-   The faster your code, the less energy it requires to be published. Remove unused and duplicated code snippets. Refactor the code to be clearer and more efficient as you develop new features.

-   Use delayed outputs when suitable to reduce the frequency of code execution, e.g., with OnChange events.

-   Limit the number of controls on the screen. This affects the memory consumption of the app.

## Data center efficiency

In the European region, the global cloud for Power Apps and Automation is in North Europe (Ireland) and West Europe (Netherlands). When comparing these countries in terms of the proportion of energy sourced from renewables or nuclear sources, the Netherlands is slightly better [(Netherlands 44%, Ireland 39%).](https://ourworldindata.org/grapher/share-electricity-low-carbon?region=Europe&country=~OWID_WRL) [Microsoft also provides data center sustainability information, which shows that there is not a significant difference between North and West Europe regions.](https://datacenters.microsoft.com/globe/fact-sheets)

When creating power platform environment from User interface, users cannot select which Datacenter / Gateway region the environment is generated (at least not in Europe Region). You can check the gateway region e.g. by “starting” the new link to data lake.

![A screenshot of power platform page, where user can link to data lake](/img/2024-Power-Platform/PowerApps-RegionInfo.png)
> Place where I think is easiest to check the accurate region info you environment locates

## The hardware Efficiency

Hardware efficiency considers the efficiency of the data center and end devices, which is mostly beyond the developer's control. The Power Platform operates in Azure, a public cloud, meaning that the level of server utilization is determined by Microsoft. Since most Power Platform projects do not directly control how end devices are managed, the only option where app makers can impact hardware efficiency is by ensuring app efficiency. You want to ensure that users are not tempted to update their end devices simply because the app you created consumes too much memory for their device to handle.

# Carbon aware power platform?

Carbon awareness simply means doing more with our apps when electricity is cleaner and less when it is dirtier (i.e., includes more carbon). At this time, I have not seen instances where there have been such massive data loads that this kind of handling would possibly be worth building or using.

But a girl can dream of a greener future. Could Microsoft provide an eco-mode feature for Power Apps? This would allow end users to decide if they are willing to shift their usage to a cleaner time or reduce the app's performance level. Or could Microsoft provide carbon-aware scheduling for Power Automate, allowing scheduled runs with renewable energy?

[Microsoft is already testing this kind of feature in Windows 11 updates,](https://www.techradar.com/news/windows-11-is-getting-an-eco-friendly-update-but-could-microsoft-do-more) so perhaps someday we will see more carbon awareness in the Power Platform as well.

# Closing words and learning content

Hopefully, this has provided you with new ideas for finding greener ways to low code. As development progresses towards more sustainable software, we anticipate the emergence of additional best practices and automated solutions to further reduce carbon emissions and energy consumption in Power Platform solutions.

If you're interested, here is more information about the subject.


Energy data: <https://ourworldindata.org/energy/>

Principles of Sustainable Software Engineering Learning materials:

-   <https://learn.microsoft.com/en-us/training/modules/sustainable-software-engineering-overview/>

-   [10 Recommendations for Green Software Development \| GSF](https://greensoftware.foundation/articles/10-recommendations-for-green-software-development)

-   [Welcome \| Learn Green Software](https://learn.greensoftware.foundation/)

-   [Sustainability workload documentation - Microsoft Azure Well-Architected Framework \| Microsoft Learn](https://learn.microsoft.com/en-us/azure/well-architected/sustainability/)

Microsft Updates Scheduling based on carbon intensivity: [This Windows 11 update is trying to save the world \| TechRadar](https://www.techradar.com/news/windows-11-is-getting-an-eco-friendly-update-but-could-microsoft-do-more)

Datacenters sustainably information: [Azure global infrastructure experience (microsoft.com)](https://datacenters.microsoft.com/globe/fact-sheets).
