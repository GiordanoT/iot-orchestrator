# Solar Panels and Wind Turbines: to the future

## System Architecture

<img src="Desktop/System_Architecture.png" alt="SystemArchitectureIoT" style="height: 200px; width:400px;"/>

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
