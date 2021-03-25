# Errors 

1. Domain Errors

    - Errors which are to be expected as part of the business process and therefore must be included in the design of the domain. 
    - The business will already have procedures in place to deal with this kind of thing, and the code will need to reflect these processes.
    - Example: an order rejected by billing, or an order that contains an invalid product code.

2. Panics

    - These are errors that leave the system in an unknown state, such as an unhandleable system errors (e.g "out of memory") or errors caused by programmer oversight (e.g. divide by zero, or null reference)

3. Infrastructure Errors
  
      - These are errors that are to be expected as part of the architecture but are not part of any business process and are not included in the domain, such as a network timeout or an authentication failure.