# Designing MyClothes.com

- MyClothes.com allows people to buy clothes online
- There's a shopping cart
- Our website is having hundreds of users at a time
- We need to scale, maintain horizontal scalability and keep our web app as stateless as possible
- Users should not lose their shopping carts
- Users should have their details (address etc) in a DB


## Solution

Start

![426f5f54edd09e806da28ba8eda54282.png](../../images/426f5f54edd09e806da28ba8eda54282.png)


Introduce stateful sessions using Stickiness (Session affinity), an ALB feature

![d3e25d8456810648f83344b4659e4c4f.png](../../images/d3e25d8456810648f83344b4659e4c4f.png)


Another way to keep state is to keep it in user cookies

![1a231aa4b6c0594a5eaea319f3d9649c.png](../../images/1a231aa4b6c0594a5eaea319f3d9649c.png)

Instead we can use a `session_id` in cookies, and then use ElastiCache or DynamoDB to store user's cart

![ae2508f969989a4a99cb68f51bb95dd0.png](../../images/ae2508f969989a4a99cb68f51bb95dd0.png)

We could also use Amazon RDS to store user info

![9b4f29328c70eff37ab858eace6328d6.png](../../images/9b4f29328c70eff37ab858eace6328d6.png)

We can scale reads with Read replicas

![8c2005805bd80bfde59cd63ef2c036d1.png](../../images/8c2005805bd80bfde59cd63ef2c036d1.png)


Alternative - lazy loading i.e. check data in cache first

![4b76b6e3ea8d308955d04f4ddf8a41eb.png](../../images/4b76b6e3ea8d308955d04f4ddf8a41eb.png)


We can use MultiAZ feature and Security groups for Disaster Recover and Security respectively

![12f8159aac87d52dd343ea11f69dd854.png](../../images/12f8159aac87d52dd343ea11f69dd854.png)

