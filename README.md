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

