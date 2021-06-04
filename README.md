# Elevator problem

## Problem Statement
Problem Statement : 
Consider we are building an algorithm for an elevator system for a highrise building like Burj Khalifa. Typically these buildings would have a considerable number of floors / stops. E.g. Burj Khalifa has 165 but let us assume that the number can go upto 200. 
Given the number of lifts are N, M people who are requesting lifts rapidly. Design an algorithm which would solve these problems 
- Minimise the wait time for each lift. 
- In the mornings a lot of people would be going to their respective offices [8-9] so in the morning load of people going up would be high 
- In the evenings a lot of people would be going down so the load will be high but it will be random 
- Sometimes when a lift arrives it is full. We would like to avoid that situation also. 


## Solution

Whenever a passenger presses an elevator button it becomes a request and it is added to requests array. 
Requests Array contains list of request objects.

```
Request : {
	id
	int floorNumber
	string direction;    (up or down)
	time 
}
```
There is list of elevators. An Elevator object has following schema.

```
Elevator {
	elevatorNumber
	state : idle/ up/ down
	fullCapacity : true / false
	floorNumber
	stops : [ ]     (floor numbers in sorted order)
	assigned : elevatorNumber
}

```


Initialize all elevators as below

```
{
	elevatorNumber = 1, 2, etc.  (unique identifier)
	state = idle
	fullCapacity = false
	floorNumber = 0, 1, etc. (divide number of floors by number of elevators and assign number from 0 to division result)
	stops [ ]  - empty array
}

```

## Algorithm 


```
for each request in requests array {
	assignElevator(Elevators, Request)
}

```
	
Assign an elevator to a passenger


```
assignElevator(Elevators, Request) {
	nearestElevator = findNearestElevator(Elevators, Request) 
	nearestElevator.stops.add(request.floorNumber)
	if elevator.state == idle
		elevator.state = request.direction
	Request.assigned = elevatorNumber
}

```

method to find the nearest elevator based on elevator's current floor position, direction, and capacity

```
findNearestElevator(Elevator, Request) {
	nearestElevator = null
	minFloorDiff = INT.MAX
	for each elevator (
		if (fullCapacity == true)
			continue;
		if (request.direction == elevator.state  || elevator.state == 'IDLE') {
			if (elevator.state = up and elevator.floorNumber < request.floorNumber)
				floorDiff = request.floorNumber - elevator.floorNumber
			else if (elevator.state = down and request.floorNumber < elevator.floorNumber)
				floorDiff =   elevator.floorNumber - request.floorNumber
			else 
				wait till an elevator is idle or an elevator changes direction
			if (floorDiff < minFloorDiff) {
				minFloorDiff = floorDiff
				nearestElevator = elevator
			}
		} 
	}
	return nearestElevator;
}
```

All elevators are running in parallel. 

method called when elevator stops at a floor. We need to reassign a passenger request if elevator becomes full.

```
updateElevatorState(Elevator) {
	remove floorNumber from stops array;
	if (stops array is empty)
		elevator.state = idle
	check elevator capacity is not full
		elevator.fullCapacity = false
	else
		elevator.fullCapacity = true
	if elevator.fullCapacity == true  {
		for each request with elevatorNumber == elevator.elevatorNumber {
			//reassign elevator
			assignElevator(Elevators, request)
		}
	}
}
```

```
method called when a passenger presses a button after entering the lift 

addFloorToElevator(Elevator, floorNumber) {
	Elevator.stops.add(floorNumber)
	check elevator capacity is full
		if full elevator.fullCapacity = true
}

```

for peek time. morning 8 - 9, evening 4 - 5  

```
for each elevators {	
	if (elevator.idle) {
		if  time between 8 to 9
			elevator.state = down until floor number is 0
		if time between 4 to 6
			elevator.state = up until floor number is top floor
	}
}
```

Also at peek times assign elevators to some floors instead of assigning all elevators to all floors. floor numbers will be shown outside the lift.

i. e divide the floors to zones 
number of zones  =  divide the floors / elevators 
each zone will be provided elevators = elevators / number of zones
	
For example : 
	say 200 floors and 40 elevators, so number of zones = 200/ 40 = 5
	divide floors to 5 zones. each zone will have 40/5 = 8 elevators.
