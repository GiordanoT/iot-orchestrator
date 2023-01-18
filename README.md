# Solar Panels and Wind Turbines: to the future

## Introduction

Our system takes care of monitoring different values coming from solar panels and wind turbines.
Wind farms consist of many individual wind turbines, which are connected to the electricity transmission grid. Wind energy is primarily the use of wind turbines to generate electricity. It is a popular, sustainable, renewable energy source that has far less impact on the environment than the burning of fossil fuels. Indeed, it aims to improve energy security and reduce the consumption of coal due to growing concerns about climate change and air pollution. Through IoT connectivity, wind farm developers can effectively monitor the performance of wind turbines and related connected devices installed in remote locations without requiring any manual intervention.

Also, solar panels produce energy and by combining them with IoT technology it is possible to maximize energy production with a consequent reduction in costs and energy consumption.
It should also be considered that solar systems are constantly exposed to atmospheric agents and over the years, problems such as accumulation of dirt, voltage drops, and malfunctioning of the components can compromise the optimal performance of the system, affecting the effective production of power. For these reasons, the most recent models of systems are combined with monitoring systems based on IoT technology capable of detecting the entire system’s performance, identifying any malfunctions, and, consequently, increasing its efficiency and energy production.

Monitoring systems, therefore, represent a fundamental innovation to ensure optimal plant performance but can also offer other benefits. In addition to mapping energy production in real-time, they collect data and parameters regarding self-consumption and on-site exchange. In this way, we will always be aware of how much energy our system produces and how it is used.
The IoT applied to the solar panel also allows you to keep track of environmental parameters, such as the presence of rain, temperature, and solar radiation, allowing you to prevent any congestion or malfunctions of the system, for example by limiting the production of the system on rainy days or, conversely, maximizing the power on days characterized by low solar radiation.

## Technology Used
+ MQTT

The protocol used by the Mosquitto broker is MQTT. We used this messaging protocol to get data from sensors. Then we loaded the data via python to send it to the other components where the data can be fetched and processed from.

+ Node-RED 

We used Node-RED to process the output data from MQTT. We also created the data flow through the use of Node-RED.

+ InfluxDB 

InfluxDB is used to store the continuous flow of data coming and going through Node-RED. The main benefit of using InfluxDB is the ease with which data can be sorted and found. 

+ Grafana (Dashboard) 

We used Grafana to visualize and understand the data. The main benefit of Grafana that we found was that, in addition to providing better visualization, it provides a way to create multiple dashboards at once which allowed us to better manage the information.

## System Architecture

![https://github.com/micheleintrevado/SE4IOT/](https://github.com/micheleintrevado/SE4IOT/blob/main/System_Architecture.png)

Our system architecture includes wind turbines and solar panels connected to sensors. 

**DATA PUBLISHER** 

The Data Publisher component is a Docker container that runs a Python project. This project utilizes the Paho library to publish data from various sources, such as wind turbines, solar panels, and the system. The data includes key performance indicators like production, light and temperature for solar panels, production, speed and vibration for wind turbines, and weather, day, and wind data for the system. This containerized setup allows for easy deployment, management, and scaling of the project, and it can be integrated with other systems that use Docker. This allows for a centralized, unified, and efficient data collection and dissemination of all the relevant data, which can be used for monitoring, analysis, forecasting, and optimization of the performance of the wind turbines, solar panels, and the system. 

**MQTT BROKER** 

The MQTT broker component is a Docker container that runs the Mosquitto MQTT broker. MQTT (Message Queuing Telemetry Transport) is a lightweight protocol for publish-subscribe messaging. The Mosquitto broker is an open-source implementation of the MQTT protocol. The Mosquitto container allows for easy deployment of the Mosquitto broker, allowing devices and applications to connect to it and publish and subscribe to MQTT messages. The broker can be configured to authenticate clients, set QoS (Quality of Service) levels, and define topics to which clients can subscribe. It also allows for the persistence of messages if needed. The Mosquitto container can be used in conjunction with the "Data Publisher" container described earlier to facilitate communication and data transfer between devices, applications, and other systems. The data publisher python application will publish data to the Mosquitto broker on specific topics, and other devices or applications can subscribe to these topics to receive the data. 

**GATEWAY – INFLUXDB - GRAFANA** 

The gateway component is a Docker container that runs the Node-RED programming tool. Node-RED is a visual programming tool that allows for the creation of data flows between various devices and services using a web-based interface. In our setup, the Node-RED container subscribes to all the data coming from the MQTT broker and stores it in InfluxDB, a time-series database. This allows for the data to be stored and easily queried for later analysis and visualization. Additionally, the Node-RED container is also integrated with Grafana, a visualization tool, that allows for the creation of interactive and informative dashboards to monitor and analyze the data coming from the solar panels and wind turbines. Moreover, in the Node-RED container, we have actuators that can publish on the MQTT broker, allowing for the control and management of the solar panels (shield) and wind turbines (running). The Node-RED container also allows for the integration of predictive models, which can predict the energy production of the solar panel based on the data like light level. Grafana, InfluxDB, and Node-RED are running in separate containers. 

**DATA PREDICTIONS** 

The data predictions component is a Docker container that runs a Flask (python) project dedicated to data predictions. This container contains a machine-learning model that is trained using the scikit-learn library. The model can predict the energy production of the solar panel based on the light level. In this setup, the Flask project creates an endpoint for a GET request, which allows the Node-RED container to access the prediction by making a GET request to this endpoint. For instance, the HTTP request: “GET http://localhost:5000/predictions/solar/3”, will produce an HTTP response in which we have a predicted value near 30.

## Insturctions to use the system
+ git clone https://github.com/GiordanoT/iot-orchestrator.git.
+	In the directory run -> docker-compose up.
+	Only at the first running you need to access Grafana (admin:admin) (localhost:3000).
+	Go to localhost:1880/ui to see the application.
