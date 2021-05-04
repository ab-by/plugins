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


