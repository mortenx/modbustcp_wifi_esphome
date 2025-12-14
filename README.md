WORKS WITH ESPHOME 2024 not 2025

ESPHOME MODBUSTCP_WIFI gateway


esphome:
  name: modbus_tcp_with_interval
  platform: ESP32
  board: esp32dev
  includes:
    - modbus_tcp_component.h

wifi:
  ssid: "your_wifi_ssid"
  password: "your_wifi_password"

logger:
  level: DEBUG  # Enable debug-level logs to see raw data in the ESPHome logs

# Web Server for Monitoring
web_server:
  port: 80

# Modbus Sensors Configuration
sensor:
  - platform: custom
    lambda: |-
      auto host = "192.168.1.50";
      auto port = 502;


      // Add sensors with custom update intervals
      auto mg_l = new ModbusTCPSensor(host, port, 4, "AB_CD", 15000);  // 15s interval
      App.register_component(mg_l);
      mg_l->set_name("Dissolved Oxygen (mg/L)");

      auto o2_percent = new ModbusTCPSensor(host, port, 7, "AB_CD", 20000);  // 20s interval
      App.register_component(o2_percent);
      o2_percent->set_name("Oxygen (%)");

      auto temp_o2 = new ModbusTCPSensor(host, port, 10, "AB_CD", 10000);  // 10s interval
      App.register_component(temp_o2);
      temp_o2->set_name("Oxygen Temperature (°C)");

      auto co2 = new ModbusTCPSensor(host, port, 13, "CD_AB", 30000);  // 30s interval
      App.register_component(co2);
      co2->set_name("CO2");

      auto co2_percent = new ModbusTCPSensor(host, port, 16, "CD_AB", 25000);  // 25s interval
      App.register_component(co2_percent);
      co2_percent->set_name("CO2 (%)");

      auto temp = new ModbusTCPSensor(host, port, 19, "CD_AB", 12000);  // 12s interval
      App.register_component(temp);
      temp->set_name("Temperature (°C)");

      auto redox = new ModbusTCPSensor(host, port, 23, "CD_AB", 18000);  // 18s interval
      App.register_component(redox);
      redox->set_name("Redox Potential");

      auto ph = new ModbusTCPSensor(host, port, 33, "CD_AB", 20000);  // 20s interval
      App.register_component(ph);
      ph->set_name("pH");

      return {mg_l, o2_percent, temp_o2, co2, co2_percent, temp, redox, ph};
    sensors:
      - name: "Dissolved Oxygen (mg/L)"
      - name: "Oxygen (%)"
      - name: "Oxygen Temperature (°C)"
      - name: "CO2"
      - name: "CO2 (%)"
      - name: "Temperature (°C)"
      - name: "Redox Potential"
      - name: "pH"
