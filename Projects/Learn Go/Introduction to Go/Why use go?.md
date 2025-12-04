## Why? Why do we use Go for:
- Web Development 
- Cloud & Network Services 
- DevOps & Site Reliability 
- Command-line Interface

### Binary Executable 
This is one of the most unappreciated aspects of Go. Being able to compile to a single executable binary means
- No runtime interpreter in needed so a binary can be far slimmer than a nest of project's sub-directories. This is benefical for containerisation and ochestration performance. 
- No additional runtime is needed for execution as it's machine code, therefore a binary executable can perform and recover with resilience. 
## Minimalist
If you played with basics of Go, you will notice that the language is not trying to be overly complex or impressive. It's just enough to get the job done. 

## Automatic garbage collection
Go is a higher-level language with automatice memory management without the need for intervention. So you can focus on the more important aspects without too much compromise on performance. 

## Format 
There is one built in formatting engine, no need to use things like "prettier.js"
and there's no need to re-invent the wheel. 

## Built-in testing and benchmarking
Libraries aren't required for unit-testing or benchmarking so you likely have familiar testing and benchmarking setups when collarborating on variety of projects. 

## Advanced concurrency techniques
Goroutines are like cheap (performance-wise) virtual threads that can be mutliplexed across real threads. Goroutines, channels, mutexes, watigroups etc encourahge patterns that allows parts of your codebase to talk to each other efficiently. Go has a vastly improved workflow when compared to concyrrency in anasynchronous platform such as node.js.

## Networking Api
go features a dedicated Networking API aims at network programming built into it's library. This is part of the external sub-repositories that belong to the Go project.

## 