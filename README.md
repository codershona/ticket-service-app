# Ticketing App:

# Section 5: Architecture of Multi-service Apps:


## Big Tickets Items:

  * The big challenges in micorservices is data.
  * Different ways to share data between services. We are going to focus on Async communication.
  * Async communication focuses on communicating changes using events sent to an event bus.
  * Async communication encourages each service to be 100% self-sufficient.
  * Relatively easy to handle temporary downtime or new service creation.
  * Docker makes it easier to package up services.
  * Kubernetes is a pain to setup, but makes it really easy to deploy + scale services. 


#### Painful Things from App #1:
 
 * Lots of duplicate code.
 * Really hard to picture the flow of events between services.
 * Really hard to remember what properties an event should have.
 * Really hard to test some event flows.
 * My machine is getting laggy running kubernetes and everything else.and 

 * What if someone created a  comment after editing 5 others after editing a posts while balancing on a tight rope...

#### Solutions:

 * Build a central library as an NPM module to share code between our different projects.

 * Precisely define all of our events in this shared library.
 * Write everythings in Typescript.
 * Write tests for as much as possible/reasonable.
 * Run a k8s cluster in the cloud and develop on it almost as quickly as local.
 * Introduce a lot of code to handle concurrency issues.

## Ticketing App Overview:

 * Users can list a ticket for an event (concert, sports) for sale.
 * Others users can purchase this ticket.
 * Any user can list tickets for sale and purchase tickets.
 * When a user attempts to purchase a ticket, the ticket is 'locked' for 15 minutes. The user has 15 minutes to enter their payment info.

 * While locked, no other user can purchase the ticket. After 15 minutes, the ticket should 'unlock'.
 * Ticket prices can be edited it they are not locked. 


## Resources Types: 

* <b>User</b>: 
    * Name  ===== Type
      * email ===== string
      * password ===== string

* <b>Ticket</b>: 
    * Name  ===== Type
      * title ===== string
      * price ===== number
      * userId ===== Ref to User
      * orderId ===== Ref to Order

* <b>Order</b>: 
    * Name  ===== Type
      * usersId ===== Ref to User
      * status ===== Created | Cancelled | AwaitingPayment | Completed
      * ticketId ===== Ref to Ticket
      * expiresAt ===== Date 

* <b>Charge</b>: 
    * Name  ===== Type
      * orderId ===== Ref to Order
      * status ===== Created | Failed | Completed
      * amount ===== number
      * stripeId ===== string
      * stripeRefundId ===== string


* User  ----- > Ticket -----> Order -------> User

## Service Types:

* <b>auth</b>: Everything related to user signup/signin/signout

* <b>tickets</b>: Ticket creation/editing. Knows whether a ticket can be updated.

* <b>orders</b>: Order creation/editing.
* <b>expiration</b>: Watches for orders to be created, cancels them after 15 minutes.

* <p>payments</p>: Handles credit card payments. Cancels orders if payments fails, completes if payment succeeds.

* We are creating a separate service to manage each type of resource.

* Should we do this for every microservice app?

* Probably not? Depends on your case, number of resources, business logic tied to each resource etc.

* Perhaps 'feature-based' design would be better.

## Events and Architectural Design:

#### Events :
   * UserCreated
   * UserUpdated
   * OrderCreated
   * OrderCancelled 
   * OrderExpired
   * TicketCreated
   * TicketUpdated
   * ChargeCreated
## Auth Service Setup:

#### auth :

| Route | Method                 | Body | Purpose               |
| --- | ------------------------- | ------- | --------------------- |
| /api/users/signup | POST       | { email: string, password: string}  | Sign up for an account     |
| /api/users/signin  | POST       | { email: string, password: string} | Sign up for an existing account   |
| /api/users/signout  | POST                      | {} | Sign out |
| /api/users/currentuser  | GET            |   -   | Return info about the user |

 * Created ticket-service-app 
 * Then create ```mkdir auth``` directory
 * Go to ```cd auth``` directory.
 * Run ```npm init -y```.
 * Install some dependencies ```npm install typescript ts-node-dev express @types/express```
 * Go back to ticket-service-app directory ```cd ..```
 * Go again ```cd auth``` directory.
 * Run ```tsc --init``` to generate config files.
 * Write some code (created src folder)
 * Run npm start to check in browser.


 ## Auth k8s Setup:

* First write a docker file. Inside auth directory create dockerfile. 

* Write docker steps into file to initial setup.
* Run ```docker build -t <mydockerid>/auth .``` to build the  docker image..
* Run ```docker build -t islamh/auth .``` to build the  docker image.

* We are going to create a deployment means it is going to create a set of pods for us automatically.

* Create a new directory inside ticket-service-app which is called ```infra```. Inside infra make a folder name ```k8s```. Then create a new file ```auth-depl.yaml```. Inside this file we will create our auth deployments. We need to write a bunch of configurations. 

## Adding Skaffold:

* <b>GOALS:</b> 

Skaffold files helps us to find all the different things we want to throw into our cluster, build them up and then throw them in and also handle some live code reload for any changes that we make to our project as well.


* After writing configurations into auth-depl.yaml file, setup skaffold configs files.

* Go to terminal after writing configurations into skaffold.yaml file. Go to ticket-service-app directory by running ```cd ..``` and then, run ```skaffold dev```.

* To debug run ```skaffold dev -v=debug```.
* Now if you open another console and type ```kubectl get pods```. You can see all the pods of our application

## Ingress-Nginx Setup:

* Go to auth directory into src folder in index.ts file.

## Hosts File and Security Warning

# Leveraging a Cloud Environment for Development:

##

##