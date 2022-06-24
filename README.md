Description
========

Ouster is a library that allows users interact with sensor hardware and recorded sensor data. This is suitable for prototyping, evaluation and other non-safety critical applications using Python or C++. The C++ library has a wide range of use cases for implementing and intereacting with sensors while the Python library supports frame-based access to lidar data as numpy datatypes and a responsive visualizer for pcap and sensor.


What you can achieve with the Library
--------

 - Sensor configuration
 - Recording and reading data in pcap format
 - Reading and buffering sensor UDP data streams reliably
 - Conversion of raw data to range/signal/near_ir/reflectivity images (destaggering)
 - Efficient projection of range measurements to Cartesian (x, y, z) corrdinates
 - Visualization of multi-beam flash lidar data
 - Frame-based access to lidar data as numpy datatypes
 - A responsive visualizer utility for pcap and sensor

Usage syntax and Sample
------------

Ouster library supports two languages, Python and C++. The complete sample for all the implementation will be provided, the usage snytax and sample aims to explain some of the critical aspects of the example to facilitate easy use. 


Sensor Configuration using C++
------------------------------

The first step is to get the sensor starting. By getting the sensor started, it works with the current config on the hostname. 

    ```
    std::cerr << "1. Get original config of sensor... ";
    sensor::sensor_config original_config;
    if (!sensor::get_config(sensor_hostname, original_config)) {
        std::cerr << "..error: could not connect to sensor!" << std::endl;
        return EXIT_FAILURE;
    }
    std::cerr << "success! Got original config\nOriginal config of sensor:\n"
              << to_string(original_config) << std::endl;
              
              ```
