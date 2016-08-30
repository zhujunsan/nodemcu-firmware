# NodeMCU 文档

NodeMCU 是一个基于 [eLua](http://www.eluaproject.net/) 的 [Espressif 出品的 ESP8266 WiFi 芯片](http://espressif.com/en/products/esp8266/) 的固件。 The firmware is based on the Espressif NON-OS SDK and uses a file system based on [spiffs](https://github.com/pellepl/spiffs). The code repository consists of 98.1% C-code that glues the thin Lua veneer to the SDK.

The NodeMCU *firmware* is a companion project to the popular [NodeMCU dev kits](https://github.com/nodemcu/nodemcu-devkit-v1.0), ready-made open source development boards with ESP8266-12E chips.

## 编程模型
The NodeMCU programming model is similar to that of [Node.js](https://en.wikipedia.org/wiki/Node.js), only in Lua. It is asynchronous and event-driven. Many functions, therefore, have parameters for callback functions. To give you an idea what a NodeMCU program looks like study the short snippets below. For more extensive examples have a look at the `/lua_examples` folder in the repository on GitHub.

```lua
-- 一个简单的 HTTP 服务器
srv = net.createServer(net.TCP)
srv:listen(80, function(conn)
	conn:on("receive", function(sck, payload)
		print(payload)
		sck:send("HTTP/1.0 200 OK\r\nContent-Type: text/html\r\n\r\n<h1> Hello, NodeMCU.</h1>")
	end)
	conn:on("sent", function(sck) sck:close() end)
end)
```
```lua
-- 连接 WiFi access point
wifi.setmode(wifi.STATION)
wifi.sta.config("SSID", "password")
```

```lua
-- register event callbacks for WiFi events
wifi.sta.eventMonReg(wifi.STA_CONNECTING, function(previous_state)
	if(previous_state==wifi.STA_GOTIP) then 
	    print("Station lost connection with access point. Attempting to reconnect...")
	else
	    print("STATION_CONNECTING")
	end
end)
```

```lua
-- 就像 Arduino 一样操作硬件
pin = 1
gpio.mode(pin, gpio.OUTPUT)
gpio.write(pin, gpio.HIGH)
print(gpio.read(pin))
```

## 起步入门
1. [编译固件](build.md) 选择你需要的模块。
1. [烧入固件](flash.md) 到芯片中。
1. [上传代码](upload.md) 到固件中。


