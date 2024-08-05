# Requirements for Magazine Subscription Service

You are being asked to develop a backend using FastAPI for a (simplified) magazine subscription service. This backend service would expose a REST API that enables users to:

1. Register, login, and reset their passwords.
2. Retrieve a list of magazines available for subscription. This list should include the plans available for that magazine and the discount offered for each plan.
3. Create a subscription for a magazine.
4. Retrieve, modify, and delete their subscriptions.

## Data Models Overview

### Magazine

A `magazine` that is available for subscription. Includes metadata about the magazine such as the `name`, `description`, a `base_price` (which is the price charged for a monthly subscription), and `discount` - a percentage, expressed as a decimal - for different subscription plans (e.g. a `discount` of `0.1` means a 10% discount).

The discount for the monthly plan will be set to zero regardless of what is in the discount column in this table.

In order to calculate the price of a subscription, you would subtract the discounted amount from the base price. For example, if the base price is $100 and the discount is 0.1, the price of the subscription would be $90.

It is possible for a magazine to only offer a subset of plans, in which case the other plans will not be tracked in the database.

### Plan

Plans to which users can subscribe their magazines. A `Plan` object has the following properties: `title`, a `description`, a `renewalPeriod`, and a `tier`. The `renewalPeriod` is a numerical value. Renewal periods CANNOT be zero. For example, a `renewalPeriod` of `1` means that the subscription renews every month.

The plans that your backend should support are:

#### Silver Plan

- title: "Silver Plan"
- description: "Basic plan which renews monthly"
- renewalPeriod: 1
- tier: 1

#### Gold Plan

- title: Gold Plan
- description: "Standard plan which renews every 3 months"
- renewalPeriod: 3
- tier: 2

#### Platinum Plan

- title: Platinum Plan
- description: "Premium plan which renews every 6 months"
- renewalPeriod: 6
- tier: 3

#### Diamond Plan

- title: Diamond Plan
- description: "Exclusive plan which renews annually"
- renewalPeriod: 12
- tier: 4

### Subscription

A `Subscription` tracks which `Plan` is associated with which `Magazine` for that user. The subscription also tracks the price at renewal for that magazine and the next renewal date. For record keeping purposes, subscriptions are never deleted. If a user cancels a subscription to a magazine, the corresponding `is_active` attribute of that `Subscription` is set to `False`. Inactive subscriptions are never returned in the response when the user queries their subscriptions.


## Business Rules

1. If a user modifies their subscription for a magazine, the corresponding subsciption is deactivated and a new subscription is created with a new renewal date depending on the plan that is chosen by the user.
    1. For this purpose assume that there is no proration of the funds and no refunds are issued.