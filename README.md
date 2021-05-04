# plugins
The readme file contains the procedure to download the necessary plugins for autonomous drone simulation in arduipilot


Install Gazebo plugin for APM (ArduPilot Master) :

                          cd ~
                          
                      git clone https://github.com/khancyr/ardupilot_gazebo.git

                      cd ardupilot_gazebo



Ubuntu 18.04 only checkout dev

                       git checkout dev


build and install plugin

              
              mkdir build
              
              cd build
               
               cmake ..
               
               make -j4

            sudo make install



     echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc


Set paths for models:

           
           echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc


          . ~/.bashrc



Run Simulator


NOTE the iris_arducopter_runway is not currently working in gazebo11. The iq_sim worlds DO work

In one Terminal (Terminal 1), run Gazebo:


             gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world



In another Terminal (Terminal 2), run SITL:


                        cd ~/ardupilot/ArduCopter/


                         sim_vehicle.py -v ArduCopter -f gazebo-iris --console



Dependencies installation


Install mavros and mavlink from source:


                          cd ~/catkin_ws


                     wstool init ~/catkin_ws/src


                 rosinstall_generator --upstream mavros | tee /tmp/mavros.rosinstall


                   rosinstall_generator mavlink | tee -a /tmp/mavros.rosinstall


                     wstool merge -t src /tmp/mavros.rosinstall


                       wstool update -t src


                rosdep install --from-paths src --ignore-src --rosdistro `echo $ROS_DISTRO` -y



catkin build
Add a line to end of ~/.bashrc by running the following command:


                 echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc


update global variables


                       source ~/.bashrc


install geographiclib dependancy

                
                sudo ~/catkin_ws/src/mavros/mavros/scripts/install_geographiclib_datasets.sh


4. Clone IQ Simulation ROS package


                  cd ~/catkin_ws/src

     
        git clone https://github.com/Intelligent-Quads/iq_sim.git


That repository should now be copied to ~/catkin_ws/src/iq_sim/ (don't run this line. This is just saying that if you browse in the file manager, you will see those folders).

run the following to tell gazebo where to look for the iq models


            
            echo "GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:$HOME/catkin_ws/src/iq_sim/models" >> ~/.bashrc



5. Build instructions

Inside catkin_ws, run catkin build:

                 
                 cd ~/catkin_ws

               catkin build


update global variables

                    source ~/.bashrc



Launch Gazebo World

This time we will launch our gazebo world by launching it with ROS as follows



               roslaunch iq_sim runway.launch



Do

                   cp ~/catkin_ws/src/iq_sim/scripts/startsitl.sh ~



so that  we can launch the ardupilot sitl by running


                        ~/startsitl.sh


Intoduction to ROS Commandline Tools

In a new terminal (ctrl+shift+T)

                          rostopic list

It will pop out different topics that are currently being published. We can see the data published in the topics by running 

               rostopic echo /(name of the published rostopic list )
               

fly the drone by running the following in the mavproxy terminal.

                     mode guided
                   
                    arm throttle

                      takeoff 15
 
 
 
 This takeoff 15 takes the drone to a height of 15m.
 
 Note that the topic /gazebo/model_states is the true model position in the simulator. This isn't something we can use in real life. In real life, we have to use the estimate of the drone's position which is formulated from a combination of its sensors. This position is trasmitted using a communication protocol called mavlink. These messages are stripped down and are optimized for radio transmission. MAVROS is a middle man which translates the MAVlink messages into ROS messages, which are easy to use and common between different robot systems. To start mavros run
 
 
                                    
                                    roslaunch iq_sim apm.launch



Now when you run rostopic list you should see a bunch of mavros topics

Now we can see the drones position in it's local frame by running



                        rostopic echo /mavros/global_position/local



In the following tutorials we will be accessing the data on these topics in our C++ programs. To see the type of message being published run



                            rostopic list -v /mavros/global_position/local


we see that the topic is publisng the message in the form of nav_msgs/Odometry

to see the structure of the message you can run the following


                          rosmsg show nav_msgs/Odometry


This will be usefull when writing publishers and subscribers in the future
