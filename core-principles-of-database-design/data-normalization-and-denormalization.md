---
icon: file
---

# Data Normalization And Denormalization

Normalization and Denormalization are two core database design strategies which have a big impact on:

* Data Integrity
* Storage Efficiency
* Query Performance
* System Complexity

If normalization is introducing organization and cleanliness to a database, then denormalization is synonymous with introducing a whole world of mess to it.

Let's talk about making our databases clean and well-structured, then we can talk about why is this not always a good a choice.

## Normalization

Normalization is a process during which you break a table into logically divided sub-section. This is where you decide which column belongs to which table and if you need some extra tables or not.

Normalization our database comes with some benefits i.e. reduced _redundancy_ and improved _data consistency_. But what do they actually mean?

Redundancy is when, you have duplicate columns across different tables. Reducing redundancy means,  you remove the duplicates from other tables and keep that data in just one place.

Consistency is a by-product of reduced redundancy. It is when there is a single column for a single piece of information e.g. age should be found in the `user_profile` table and nowhere else.

Let's understand this with an example:

<figure><img src="../.gitbook/assets/Screenshot_10.png" alt=""><figcaption></figcaption></figure>

Would you say that this schema contains redundancy? Obviously you would point out the `price` column which is present in both the `meals` table and the `meal_orders` table. Moreover there is a derived column in the `orders` table named as `total_price` , which you can probably guess is the total sum of all the meals ordered in a single order.

So in conclusion the above schema contains redundancy. And as it happens, normalizing it is very simple. You just eliminate the `price` column from `meal_orders` table and `total_price` from `orders`table.

So the next time you want a total sum of all the meals ordered in a single order, you would do a query like this:

```sql
SELECT sum(price) FROM meal_orders AS mo INNER JOIN meals AS m 
ON mo.meal_id = m.id 
WHERE order_id = 123;
```

## Denormalization

Just to keep us both on the same page, denormalization in databases is not always seen as a defect or something wrong. Yes, it has its downsides, but it is rarely talked about in schools when should we denormalize our database _on purpose_.

To demonstrate, we will use the example I shared above which includes `meals`, `meal_orders` and `orders` tables. The redundancy introduced in it can be necessary for a number of reasons.

* Admin may change the price of `meals` on the fly. So in that case, what will we do if want to know how much money the user paid for a certain meal at a certain time?
* User may use a discount on the total price of the order. In this case, having the `total_price`  column inside `orders` table can be helpful. &#x20;
* Having these redundant columns in our database actually help us keep a report or how much money came into the system and how much was discounted. The app owner might need this report for his accountant.

So you see that denormalization can be just as necessary as normalization in certain scenarios. And before you ask, no there is no way to know when to use what. Just be aware of your use-case and the clients requirements and learn to think for yourself.



