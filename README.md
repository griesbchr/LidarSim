# LiDARSim2D #

A simple 2D lidar simulator using raytracing to model lidar sensors with consideration of motion distortion effects.

* Christoph Griesbacher &mdash; christoph.griesbacher@student.tugraz.at
 
## Description ##

LiDARSim2D is a simple generic lidar simulator, written in Python. It uses radial raytracing to approximate a real lidars sensor output. The calculations are done with Special consideration of motion distortion. 

LidarSim2D also inclused a GUI implemented in PyQt5 that visualizes the simulation results and set simulation Parameters. A framework to generate simple scenarios consisting of static and dynamic polygons is also included.  

Note that LidarSim2D does not model a specific sensor system. However, it
can be easily modified to model a sensor. Lidar parameters such as rotation speed, field of view, angular resolution, maximum scanning distance and turning direction can be choosen arbitrarily. 

## Requirements ##
  * Numpy 1.19: General data handling
  * Numba 0.53: To Speed up ray intersection calculations 
  * Matplotlib 3.3.3: To visualize results, A matplotlib figure is embedded in the PyQt5 GUI
  * PyQt5 5.15: To implement the GUI
  * SciPy 1.6: Dependency for Numba
  * PyYAML 5.4

## Usage ##

The LidarSim2D can be called using one of two ways:

1. via the main.py function
    * select simulation parameters in head section of main.py
OR
    * select simulation parameters "model", "scenario" and "T_SIM" via commandline using the following format:			
       * main.py "model" "scenatio" "T_SIM" 	eg.: main.py realsensor scenario_crossing 5 (each argument is passed as a string)
       * commandline parameters are optional, if parameters are left out the the parameters given by the script are used.
    *  execution time and other simulation parameters will be loged in the Logs.txt file
    * visual representation of the results can be plotted by setting the "viewer" flag to 1 in the file head
    * profiling of the getPointcloud function can be plotted by setting the "profiling" flag to 1 in the file head
    * Pointclouds are calculated using the getPointCloudRealSensor_rt and getPointCloudModelX_rt functions
    * used for benchmarking and testing of the functionality of the program (RunBenchmarks.py)

2. via the SceneViewer.py function
    * calls the GUI which further calls the program
    * parameters can selected within the GUI
    * no logging of the execution times because execution time is limited by the plotting speed of the plotting framework
    * Pointclouds for models are calculated using the getPointCloudModelX_rt functions
    * Pointcloud for the realsensor is calculated using the getPointCloudRealSensor (NOT the getPointCloudRealSensor_rt) function
    * provides option to directly compare the PC (pointcloud) of the realsensor calculation option with the lidar model calculation options
    * used to provide visual demonstration examples of a scenatio
    * timeslider can be controlled using the arrow keys (also in combination with the "alt" key) to navigate in scenarios

The lidar parameters can be tuned in the data.py file in the egocar class.

## Stucure ##

The project can be devided into 3 categories:

1. Framework that provides data about the scenario to the getPointcloud functions:
    *  data.py: contails all the classes and class Methods
    * scenarios.py: scenarios (instances of scenario class) are initialized here eg. scenario_crossing, scenario_overtake
    * viewer.py: plots scenario and pointcloud (used if viewer flag is activated in main.py file)

2.  getPointcloud functions and its subfuctions:
    * getPointcloud.py: contains getPointCloud() functions
    * coordTrans.py: subfunction of getPointCloud(), contains all kind of coordinate transformations
    * getRayIntersections.py: subfunction of getPointCloud()

3. files that call the program:
    * main.py
    * sceneviwer.py

## Additional notes ##
* coordinate systems: 
  * global coordinate system: Ground Truth coordinate system, used to describe Scenarios and all its attributes
  * vehicle coordinate system: Origin is at the center of the rear axis of a vehicle, ego_x_y_th/target_x_y_th describe translation and rotation of vehicle coordinate system vs the global coordinate system, used to describe car vertices and lidar mounting position (relative to the car). 
  * sensor (or hit) coordinate system: origin is the position of the lidar, rotation is always 0 (that means no rotation relative to the global coordinate system), getRayIntersection() function returns hit coordinates in this coordinate system. 

* numba: numba can be "deactivated" by commenting out the "@njit(....)" headers of the functions. 


