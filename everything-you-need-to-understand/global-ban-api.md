---
icon: gavel
coverY: 0
---

# Global Ban API

The Global Ban API is a system made for OpenAC Consumers to ban previous offenders by saving them into a database of known cheaters. By default, OpenAC will contribute to the service by sending a request to our own Web API with your own SECRET API Key in order to authenticate your request. For security reasons, please do not share the URL that is located in the `ModuleScript` where the API is implemented, OR your API Key.

{% hint style="danger" %}
Please note that if we find out that any part of OpenAC has been leaked and the leak traces back to you, we will be, for security reasons, forced to blacklist you from OpenAC entirely, this means removing your API Key as well. Abusing the API to report non-exploiters will also result in a blacklist.
{% endhint %}

***

The two functions related to the Global Ban API are `OpenAC::IsUserGlobalBanned` and `OpenAC::ReportGlobalBanAPI`. Both take a `Player`, and for security reasons need to be in a protected call via any of the `pcall` functions.

Usage example:

```lua
--[[
    Example of checking if a user is banned with the Web API via `OpenAC::IsUserGlobalBanned`
]]
local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(Player)
    if OpenAC.Methods:IsUserGlobalBanned(Player) then
        Player:Kick("Please rejoin.")
        Players:BanAsync({
            UserIds = {Player.UserId},
            Duration = -1, --// Permanent Ban
            DisplayReason = "You have been banned permanentely for Cheating in another experience. Please contact us if you believe this is incorrect.",
            PrivateReason = "OpenAC Global Ban: " .. Detection
        })
    end
end)

--[[
    Example of reporting a user to the Web API via `OpenAC::ReportGlobalBanAPI`
]]
--// Please note the `OpenAC::ReportGlobalBanAPI` function
--// will error if the AntiCheat hasn't detected any cheating, or if the detection
--// isn't a native detection (aka an OpenAC one)
local Players = game:GetService("Players")

OpenAC.Signals.OnDetection:Connect(function(Player, Detection, IsCustomDetection)
    if not IsCustomDetection then
        local Success, Error = pcall(function() --// Can error
            return OpenAC.Methods:ReportGlobalBanAPI(Player)
        end)

        if not Success then
            warn("[!] An error occured: " .. Error)
        end
    end

    Player:Kick("Please rejoin.")
    Players:BanAsync({
        UserIds = {Player.UserId},
        Duration = -1, --// Permanent Ban
        DisplayReason = "You have been banned permanentely for Cheating. Please contact us if you believe this is incorrect.",
        PrivateReason = "Automatic AntiCheat Ban: " .. Detection
    })
end)
```
