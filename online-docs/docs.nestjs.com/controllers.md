#### Controllers
- Handling incoming requests and returning responses to the client
- Having more than one route
  - different routes -> different actions
  - sort of a group of multiple route-action pairs??
- Decorators enable Nest to create a routing map

#### Routing
- Controller decorator
  - to define a basic ctlr
  - optional arg: route path prefix to group related routes
- HTTP request decorator ex) GET
  - to cr a hdlr for a specified endpoint ( HTTP method + route path )
    - route path: ctlr decorator prefix + req decorator specified path
  - user must declare a method to bind the route to
  - NestJS response manipulation
    - standard(recommended): js object / array / primitives
	  - auto serialization for object/array
	  - bypass for primitive types(stirng, number, boolean)
	  - default status code 200
	- using Res() decorator for library specific response obj
      - auto disabled using both Res and Next, rather use passthrough option

#### Request object
- 

#### Resources

#### Route wildcards

#### Status code

#### Headers

#### Redirection

#### Route parameters

#### Sub-Domain Routing

#### Scopes

#### Asynchronicity

#### Request payloads

#### Handling errors

#### Full resource sample

#### Getting up and running

