# Synapse X Tools

This extension allows the possibility of using Visual Studio Code as executor and console output, with Synapse X

# How to make it work

1 ° Install the extension__
2 ° Copy the code below and paste it in the folder of Synapse X "autoexec"__
3 ° Enter a roblox map and have fun

```lua
local ws

spawn(function()
    repeat wait() until game:IsLoaded()
    while wait(1) do
        pcall(function()
            ws = syn.websocket.connect("ws://localhost:33882/")

            ws:Send("auth:" .. game.Players.LocalPlayer.Name)
            ws.OnMessage:Connect(function(msg)
                local func, err = loadstring(msg)

                if err then
                    ws:Send("compile_err:" .. err)
                    return
                end

                func()
            end)

            ws.OnClose:Wait()
        end)
    end
end)

game:GetService("LogService").MessageOut:Connect(function(m)
    if ws then
        ws:Send("log:"..m)
    end
end)
```
