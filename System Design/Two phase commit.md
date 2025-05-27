
Born to resolve transaction problems in microservices
# Basics
- have two phase: **prepare phase** & **commit phase**
- need 1 component **coordinator** have responsible for manage **local transaction** of **operation service** -> orchestrator take request and send transaction request control to **coordinator**
# Two phase process
We will example process like this: User process order some amount of product and will be charge that same amount of those products. We have 2 service: payment service and order service
## Prepare phase
### First step
- **Coordinator** send request to payment service and product service.
- **Payment service** `begin transaction` process check user money is enough for that order or not.
	- If yes -> reduce that amout of money from user -> send OK to coordinator
	- If not -> send ERROR to coordinator
- **Order service** `begin transaction` check product is enough for order or not
	- If yes -> reduce that amount of products ->  send OK to coordinator
	- If not -> send ERROR to coordinator
### Second step
- If one of request fail -> send rollback signal to all service
- If all success -> send to next phase
## Commit phase
In this step after all sucess. **Coordinator** send request commit to all service. But have some point we should notice
- We can see the problems with this pattern is network fail, power shutdown. We should have mechanism to retry commit again
- What if coordinator shutdown everything will be lost ? -> we can resolve with durable storage to log process status -> continuous mechanism on **coordinator**
