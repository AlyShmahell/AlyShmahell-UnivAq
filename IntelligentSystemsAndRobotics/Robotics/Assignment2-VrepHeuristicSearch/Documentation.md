The maze solver I implemented depends on the wallhugger method, as suggested by a colleague of mine, which basically follows the following pseudocode:

sense(leftWall)
if open(leftWall): 
	turn(left)
elif clear(path):
	move(forward) 
else:
	move(right).

this algorithm, had it been implemented on a simple python CLI board game, it'd have been so easy, even with a limited knowledge in python.

however, integrating it with an already functioning obstacle-avoidance function proved to be a much harder task.

one of the main difficulties was calibrating the sensors, they don't seem to be responding well when initiated in opmode_streaming then used in opmode_buffer. they do work well in opmode_streaming alone, however, the first few vital steps of the robot are lost in transmission and the poor robot loses its heading. at least that's what happened on my device, running Ubuntu 14.04.

to fix this, I eventually figured out that not only do i need to find an opening in the wall, i also need to make the robot detect it using two sensors instead of one, to do this I implemented the following:

while(sensing):
	if(firstSensor):
		avoid(obstacles)
		moveState=True
	else:
		if(secondSensor):
			turn(Left)
             		# above tells the robot to turn as soon as the first sensor detects an opening.
		elif moveState:
			Turn(Left)
			# above will insure the robot turns when both sensors are detecting nothing.
			moveState=False
			Move(Forward)
			# above line will insure the robot doesn't keep turning.



