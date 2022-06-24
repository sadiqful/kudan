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

```cpp
const std::string sensor_hostname = argv[1];

    // 1. Get the current config on the sensor
    std::cerr << "1. Get original config of sensor... ";

    //! [doc-stag-cpp-get-config]
    sensor::sensor_config original_config;
    if (!sensor::get_config(sensor_hostname, original_config)) {
        std::cerr << "..error: could not connect to sensor!" << std::endl;
        return EXIT_FAILURE;
    }
    //! [doc-etag-cpp-get-config]
    std::cerr << "success! Got original config\nOriginal config of sensor:\n"
              << to_string(original_config) << std::endl;
```

The next step is to empty the current sensor configuration and set a few configuration parameters. This allows you to modify the current config

```cpp
std::cerr << "\n2. Make new config and set sensor to it... ";
    //! [doc-stag-cpp-make-config]
    sensor::sensor_config config;
    config.azimuth_window = std::make_pair<int>(90000, 270000);
    config.ld_mode = sensor::lidar_mode::MODE_512x10;

    // If relevant, use config_flag to set udp dest automatically
    uint8_t config_flags = 0;
    const bool udp_dest_auto = true;  // whether or not to use auto destination
    const bool persist =
        false;  // whether or not we will persist the settings on the sensor

    if (udp_dest_auto) config_flags |= ouster::sensor::CONFIG_UDP_DEST_AUTO;
    if (persist) config_flags |= ouster::sensor::CONFIG_PERSIST;
    //! [doc-etag-cpp-make-config]

    if (!sensor::set_config(sensor_hostname, config, config_flags)) {
        std::cerr << "..error: could not connect to sensor" << std::endl;
        return EXIT_FAILURE;
    }
    std::cerr << "..success! Updated sensor to new config" << std::endl;
```

These two steps suffices but you can confirm the config from the sensor after it has been updated. This is just to make sure you have the correct config

```cpp
 std::cerr << "\n3. Get back updated sensor config... ";
    sensor::sensor_config new_config;
    if (!sensor::get_config(sensor_hostname, new_config)) {
        std::cerr << "..error: could not connect to sensor" << std::endl;
        return EXIT_FAILURE;
    } else {
        std::cerr << "..success! Got updated config" << std::endl;
    }
    ```
   
These two steps suffices but you can confirm the config from the sensor after it has been updated. This is just to make sure you have the correct config
