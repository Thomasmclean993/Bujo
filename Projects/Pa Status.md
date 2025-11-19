Is an api that allows us to retrieve updates/responses on the status of a prior authorization. 

## Context
In the old days, the CMM Hubble platform was solving the problems it was designed for. Prior Authorizations would often fail due to:
- Missing Data
- Malformed Forms
etc. While the platform was increase the rate of successes, there was still hang ups that occurred that only the Humans can solve, with a phone. Most insurance companies will only give updates over the phone, which meant that everyone was calling them. Providers, Pharmacists and our service agent would call the status lines from 8am to 7pm. Most companies could not handle the amount of phone calls. Some were considering putting their numbers on the DNC list. 

So our team came up with an idea, let's retrieve these statuses via api. We will be able to continue assist clients with their status checks while providing a service to the insurance companies. 

## The problems
During the requirement phase of the project, we discovered this will not be a simple retrieve of the status of the PA. There was business logic that need to be included in the api:
- Insurance companies require not only patient information but Providers info/identification to retrieve the statuses. 
- If there are multiple PAs for the same drug, there will be multiple statuses, these will need to be filtered out as well. We would need to remove the status for different providers as well. 
- Since this api will be making data from multiple sources available, normalization of the data would be necessary as well.
- With Each endpoint using a different auth strategy, the api would need to be able to handle retrieving the auth tokens, not matter the source.
# Design 

Our client will sent pa status request to an internal app that will call our api for the status update. Our api will attempt to send request to our partners (external) apis and return