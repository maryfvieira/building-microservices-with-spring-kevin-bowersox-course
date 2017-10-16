# Circuit Breaking, Fault Tolerance and Health Monitoring with Hystrix

- Start simple-service microservice project
- Start hystrix microservice project

Absence of fault condition, circuit is closed: ![closed:](https://github.com/excelsiorsoft/building-microservices-with-spring-kevin-bowersox-course/blob/master/curcuit-breaker-fault-tolerance-with-hystrix/circuit-closed.PNG)

- Kill simple-service

Presence of fault condition, circuit is open: ![open:](https://github.com/excelsiorsoft/building-microservices-with-spring-kevin-bowersox-course/blob/master/curcuit-breaker-fault-tolerance-with-hystrix/circuit-open.PNG)

- Restart simple-service

Absence of fault condition, circuit is closed again: ![closed:](https://github.com/excelsiorsoft/building-microservices-with-spring-kevin-bowersox-course/blob/master/curcuit-breaker-fault-tolerance-with-hystrix/circuit-closed.PNG)


- Adding timeout to HystrixCommand: 

```
@SuppressWarnings("unchecked")
	@RequestMapping("/startClient")
	@HystrixCommand(fallbackMethod="failover", commandProperties= {
			@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "500")
	})
	public List<String> startClient(@RequestParam long time) throws InterruptedException{
		Thread.sleep(time);
		return restTemplate.getForObject("http://localhost:8899/service", List.class);
	}
	
	public List<String> failover(long time){
		return Arrays.asList("Default1", "Default2");
	}
```

![https://github.com/excelsiorsoft/building-microservices-with-spring-kevin-bowersox-course/blob/master/curcuit-breaker-fault-tolerance-with-hystrix/timing.PNG](https://github.com/excelsiorsoft/building-microservices-with-spring-kevin-bowersox-course/blob/master/curcuit-breaker-fault-tolerance-with-hystrix/timing.PNG)