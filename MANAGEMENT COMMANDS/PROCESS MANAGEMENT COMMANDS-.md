PROCESS MANAGEMENT IN LINUX (20-02-25)

Process management is essential for monitoring and control 1 inq system performance in Linux 

On Linux computers there are special commands to control the programs that are running. These commands are called process management commands. With the help of process management commands you can look at the list of the programs that are currently running on your computer. You can also start new programs and run them and stop programs that are already running and make some programs more important than others.

Process management commands help you understand and manage the different programs running on your Linux computer. They allow you to see what's happening and control the programs as needed.   
	Below are commands for managing processes

		Running Commands in the Background & Foreground
			
			Runs ping in the foreground.
			
				# ping 8.8.8.8


			


		Runs ping in the background (background process) .
			
			# Ping 8.8.8.8 &

			# to kill use (fg = foreground)

			# jobs to see background job

		Sends a large sequhnce to /dev/null in the background.
			
			# ping 8.8.8.8 >/dev/null &
			
			# seq 100000000000000000000000 > /dev/nutl & 

		Viewing Background Jobs, Lists all background jobs.
			
			# jobs

		Brings the last background job to the foreground, Brings job number 2 to the foreground.

			# fg 2

		Viewing Processes with ps Command
			
			Shows processes running in the current shell session.
				
				# ps
		
		Displays more details about running processes.

				# ps -l

		Lists all processes with CPU and memory usage (BSD-style) .
				
				# ps -aux

		Lists all processes using standard syntax.
				
				# ps -e

		Displays processes in a hierarchical tree format.
			
			# ps -axjf

		Shows processes owned by root.
			
			# ps -U root -u root u
		
		Shows processes owned by tanishq.
			
			# ps -U tanishq -u tanishq u


	Monitoring Processes in Real-Time
		
		Displays real-time system processes.
			
			# top


	Using htop (Better Alternative to top)
		
		# apt install htop 				# Debian/Ubuntu
		
		# yum install htop 				# RHEL/Cent*


		Better version of top 

			# htop

		tree struucture 

			# htop -t

		specific user 

			# htop -t -u user-name


		Displays details of a specific process by PID.
			
			# htop -t -p 2780


	KILLING PROCESSES 

		kill a particular process 

			# kill PID

		kill forcefully 

			# kill -9 PID 


		Kills all instances of seq.
			
			# yum install psmisc
			
			# killall seq

		Kills all processes with names matching firefox (regex) .
			
			# killall -r firefox

		Kills all processes owned by user.
			
			# killall -u username

		kill all process of ping using pkill

			# pkill ping

		Kills exact match for ping process owned by user.
			
			# pkill -u username -x ping


-------------------------------------------------------------------



-------------------------------------------------------------------

