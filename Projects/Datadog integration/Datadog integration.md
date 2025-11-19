Leader made the decision to move from Grafana and sentry to Datadog as our observability Platform. Most of the organize is written in Ruby on Rails and Javascript. For DD, worked right out of the box for most teams. Not our team, our apps was written in elixir, which is not support be DD. So I had to think outside the box for the project. 

Keep in mind: 
- our Devops team informed us that Open telemetry was not an option for us. I believe it was a decision from on high. 
- We had a hard deadline, that was looming. So we need something working soon. Before we love observability. 
So I knew the High level problem but some bug me. How was datadog generating metrics from other languages? Was there a way for me to close the gap?

Discovered that datadog would use a daemon, to listen and parse out the events on the docker container. DD would add logic for the daemon to know what to expect depending on the language. DD has a implementation that is lesser know called DogstatsD. The statsd server will accept metrics and the datadog agent will read the metrics from said server. So I know what I needed to do.
1. Connect to the Datadog agent via UDP
2. Create an adaptor to translate metrics into the format Datadog expects. 
3. Add metric calls to our business logic
I used the Static Library that handles the formatting of the metrics

There was multiple ways to create and connect the adapter, I chose to use plugs. Which I add to the pipeline of the endpoint it self. 

A Plug is an core component for Elixir, it is a abstract layer that separates middleware logic from the business logic. You add the module that will to the pipeline in the Router.ex. 

In the module itself, you add init and call. 
Init initializes the arguements while call handles the transformation itself.

The plug will pull what I call global metrics from the connection data. These will then be added to the process dictionary. A local Key value store for the process. 

## Request Specific
I created a separate Adapter for Request specific metrics. Like Successful response parsing or failure to supplementary response retrieval. When increment/2 is called, we will combine the request specific metrics with the global metrics, then send them to the datadog agent via Statix. 
