# ESP32-bitRate

## Function to measure time 

Using the API guide for [Maximizing Execution Speed](https://docs.espressif.com/projects/esp-idf/en/v4.4.2/esp32/api-guides/performance/speed.html) and a few adaptations I statig by testing the code that measure time. I also increase the clock speed, later I can text some other features (see Improving Overall Speed).

```c
void measure_important_function(void) {
    const unsigned MEASUREMENTS = 2;
    uint64_t start = esp_timer_get_time();

    for (int retries = 0; retries < MEASUREMENTS; retries++) {
        //important_function(); // This is the thing you need to measure
        vTaskDelay(1000/ portTICK_PERIOD_MS);
    }

    uint64_t end = esp_timer_get_time();

    printf("%u iterations took %llu milliseconds (%llu microseconds per invocation)\n",
           MEASUREMENTS, (end - start)/1000, (end - start)/MEASUREMENTS);
}
```

As result, we can confirm that the function works. 2 iterations of 1 second took 2 seconds.

![image](https://github.com/Rafaelatff/ESP32-bitRate/assets/58916022/5f3d7757-ed34-433b-91e0-3a1d54810e77)

I run a few tests to know the required timming for a few functions to run.

* Calling only the `measure_important_function()`, 0 microseconds per invocation -> sometimes 1 uS (very rare).
* `xTaskCreate(udp_server_task, "udp_server", 4096, (void *)AF_INET, 5, NULL);`, 2 milliseconds (2912 microseconds per invocation).
* `ESP_ERROR_CHECK(init_wifi());`, 249 milliseconds (249124 microseconds per invocation).
* `wifi_scan();`, 2930 milliseconds (2930294 microseconds per invocation).

![image](https://github.com/Rafaelatff/ESP32-bitRate/assets/58916022/30db8473-9237-4f53-a0fa-b09b52ddedeb)

## WiFi

Now using the API guide for [How to improve Wi-Fi performance](https://docs.espressif.com/projects/esp-idf/en/v4.4.2/esp32/api-guides/wifi.html#how-to-improve-wi-fi-performance)...

()
