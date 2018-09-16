## MicroPython on the ESP8266

Install micropython, e.g. Wemos D1 Pro (4 MB):
```   
python -m esptool --port COM<xx> erase_flash
python -m esptool --port COM<xx> --baud 115200 write_flash -fm dio --flash_size=4MB 0 <yyy>.bin
```
