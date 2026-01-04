# Signal  

The **Signal** is a lightweight utility designed to create **custom event systems** in Roblox, similar to `RBXScriptSignal`. It provides an intuitive API to manage connections, fire events, and track listener states with optional priority handling.  

---

## Key Benefits  
- Simplifies custom event creation and management.  
- Supports one-time (`Once`) and persistent listeners.  
- Prioritized listener invocation.  
- Efficient connection management with full cleanup capabilities.  

---

## API Reference  

### Constructor  
- **`.new(): Signal`**  
  Creates a new Signal object.  

---

### Connecting Listeners  
- **`:Connect(callback: Callback, priority: number?): Connection`**  
  Connects a persistent listener to the signal. Optionally, define a priority to control call order.  

- **`:Once(callback: Callback): Connection`**  
  Connects a listener that is **automatically disconnected after the first invocation**.  

---

### Firing Events  
- **`:Fire(...: any)`**  
  Fires all connected listeners, passing any arguments provided.  
  - Listeners connected with `:Once` are automatically removed after firing.  
  - Listeners are invoked in **priority order** if specified.  

- **`:Wait(): (...: any)`**  
  Yields until the signal is fired and **returns the arguments** passed to `:Fire`.  

---

### Connection Management  
- **`:DisconnectAll()`**  
  Disconnects **all listeners** attached to the signal.  

- **`:HasConnections(): boolean`**  
  Returns `true` if the signal has any active connections.  

- **`:GetConnectionCount(): number`**  
  Returns the **number of active connections**.  

- **`:IsConnected(callback: () -> ()): boolean`**  
  Returns `true` if the provided callback is currently connected via `:Connect` or `:Once`.  

- **`:Destroy()`**  
  Cleans up the signal by disconnecting all listeners and clearing internal references.  

---

## Example Usage  

```lua
local Signal = require(path.to.Signal)
local mySignal = Signal.new()

-- Connect a listener
local conn = mySignal:Connect(function(msg)
    print("Received:", msg)
end)

-- Connect a one-time listener
mySignal:Once(function(msg)
    print("This prints only once:", msg)
end)

-- Fire the signal
mySignal:Fire("Hello World")

task.delay(1, function()
  mySignal:Fire("Hey!")
end)

-- Wait for the signal
local msg = mySignal:Wait()
print("Wait received:", msg)

-- Cleanup
mySignal:DisconnectAll()
mySignal:Destroy()
```
