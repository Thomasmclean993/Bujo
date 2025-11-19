Leader made the decision to move from Grafana and sentry to Datadog as our observability Platform. Most of the organize is written in Ruby on Rails and Javascript. For DD, worked right out of the box for most teams. Not our team, our apps was written in elixir, which is not support be DD. So I had to think outside the box for the project. 

Keep in mind: 
- our Devops team informed us that Open telemetry was not an option for us. I believe it was a decision from on high. 
- We had a hard deadline, that was looming. So we need something working soon. Before we love observability. 
So I knew the High level problem but some bug me. How was datadog generating metrics from other languages? Was there a way for me to close the gap?

Discovered that datadog would use a daemon, to listen and parse out the events on the docker container. DD would add logic for the daemon to know what to expect depending on the language. DD has a implementation that is lesser know called DogstatsD, 